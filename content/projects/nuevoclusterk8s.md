---
title: "Nuevo Clúster K8S"
date: 2021-02-01T16:24:34+01:00
draft: false
image: "kubeadm.png"
tags: "proyectos, k8s, kubernetes, clúster, kubectl, kubeadm"
---

## El proyecto

Voy a trastear con un nuevo clúster k8s, y he decidido desplegarlo con *kubeadm*, o no, ya veremos. Nodo a nodo para ir probando algunas cosas. Puede que luego vaya "automatizando" algunas tareas según vea lo que necesite.

<!--more-->

## Documentación

Primero dejaré por aquí los enlaces de la documentación que vaya consultando. Aunque realmente hay cosas de las que me acuerdo y otras que no suelo mirar, creo que es mejor validar muchas cosas antes de hacerlas.

- [instalación de kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)
- [clúster kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/)

## Dónde

Hay muchas maneras de tener un clúster de kubernetes, y muchos sitios para desplegarlo, con herramientas automáticas y con alojamiento o sin el. En este caso me voy a aprovisionar mi propia infraestructura, al menos inicialmente. Recurriré en parte a nodos en *paravirtualización* y en parte a nodos físicos medio volátiles que serán *workers* intercambiables.

Para empezar un sistema de 3 nodos **master** y 3 nodos **worker**, más un nodo para el **load balancer** y algún otro servicio extra que me pueda interesar. Todos tendrán Ubuntu 20.04 como sistema operativo.


nombre     | ip              |  funciones  
 ------------ |-------------| ------------
 klb-1         | 172.17.1.200 | load balancer
 kubemaster-1  | 172.17.1.201 | control/master/etcd  
 kubemaster-2  | 172.17.1.202 | control/master/etcd  
 kubemaster-3  | 172.17.1.203 | control/master/ etcd  
 kubeworker-1 | 172.17.1.101 | worker  
 kubeworker-2 | 172.17.1.102 | worker  
 kubeworker-3 | 172.17.1.103 | worker  

## Instalación

Para empezar recomiendo configurar desde un nodo acceso ssh mediante clave rsa. Luego hay que preparar el load balancer, un runtime(utilizaré containerd), e instalar los componentes en los nodos de kubernetes; finalmente pasar a desplegar y configurar los nodos.

### Instalando el loadbalancer

Para empezar en el loadbalancer **klb-1** instalamos y configuramos el haproxy.

```bash
apt update && apt install -y haproxy
frontend kubernetes-frontend
    bind 172.17.1.200:6443
    mode tcp
    option tcplog
    default_backend kubernetes-backend

backend kubernetes-backend
    mode tcp
    option tcp-check
    balance roundrobin
    server kubemaster-1 172.17.1.201:6443 check fall 3 rise 2
    server kubemaster-2 172.17.1.202:6443 check fall 3 rise 2
    server kubemaster-3 172.17.1.203:6443 check fall 3 rise 2

```

### Preparando los nodos

Hay que asegurarse de desactivar el swap si existe. En caso de estar utilizando ufw probablemente también nos interese abrir puertos o quitarlo y añadir el firewall por delante del loadbalancer. Asumo que tampoco hay configuraciones raras en el iptables, de todas maneras hay una lista de los puertos requeridos por kubernetes, especialmente en la red de comunicacón entre los nodos.

```bash
#swap
swapoff -a; sed -i '/swap/d' /etc/fstab

#ufw
ufw disable
```

Hay que actualizar también sysctl, para el networking de k8s.
```bash
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
net.bridge.bridge-nf-call-iptables  = 1
net.ipv4.ip_forward                 = 1
net.bridge.bridge-nf-call-ip6tables = 1
EOF

# finalmente
sudo sysctl --system
```
En este caso he decidido usar containerd en lugar de docker como runtime:

```bash
sudo apt-get update && sudo apt-get install -y containerd

# Configure containerd
sudo mkdir -p /etc/containerd
containerd config default | sudo tee /etc/containerd/config.toml

# Restart containerd
sudo systemctl restart containerd


cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

cat <<EOF | sudo tee /etc/modules-load.d/containerd.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter
```

### Setup de kubeadm y otros componentes

Para la instalación hay que seguir las instrucciones de la documentación oficial casi en un paso a paso.

```bash
sudo apt-get update && sudo apt-get install -y apt-transport-https curl
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
``` 

## Creando y configurando el clúster

### Masters/controllers

En cualquiera de los nodos master, en mi caso el primero, iniciamos el clúster. Apuntamos a la IP del loadbalancer como endpoint para inicializar.

```bash
kubeadm init --control-plane-endpoint="172.17.1.200:6443" --upload-certs --apiserver-advertise-address=172.17.1.201 --pod-network-cidr=10.0.0.0/8
```

Nos devolverá el token y los datos que necesitaremos a continuación para añadir nuevos nodos. Copiarlos para luego. Asegurandose de mantener a salvo el token y los certificados.

**Añadir kubemaster-2 y kubemaster-3**

Es importante recordar que apuntaremos al loadbalancer para unirnos, y además pasaremos la opción --apiserver-advertise-address= con la ip de cada master.

```bash
# kubemaster-2
sudo kubeadm join 172.17.1.200:6443 --token <token> \
    --discovery-token-ca-cert-hash <cert> \
    --control-plane --certificate-key <key> --apiserver-advertise-address=172.17.1.202

# kubemaster-3
sudo kubeadm join 172.17.1.200:6443 --token <token> \
    --discovery-token-ca-cert-hash <cert> \
    --control-plane --certificate-key <key> --apiserver-advertise-address=172.17.1.203
```

### Workers/computers

Para unir nuevos workers utilizamos igualmente el comando con los datos devueltos, en este caso sin las opciones de --control-plane ni --apiserver-advertise-address.

```bash
sudo kubeadm join 172.17.1.200:6443 --token <token> \
    --discovery-token-ca-cert-hash <cert>

```

### Añadir la red

Finalmente añadiremos el cni para la red interna de kubernetes. En este caso probé en su moento calico, y ahora utilizaré flannel.

```bash
sudo kubectl --kubeconfig=/etc/kubernetes/admin.conf create -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

## Administración

### Como un usuario regular

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

### Remota

Podemos administrar nuestro clúster desde ota máquina copiando la configuración, siempre y cuando tengamos además kubectl instalado en dicha máquina.

```bash
mkdir ~/.kube
scp root@172.17.1.201:/etc/kubernetes/admin.conf ~/.kube/config

# probar
kubectl cluster-info
kubectl get nodes
```


## Se vienen cambios 

**continuará...**

Saludos
[DarFig](https://github.com/DarFig)