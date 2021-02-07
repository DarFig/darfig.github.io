---
title: "Nuevo Clúster K8S"
date: 2021-02-01T16:24:34+01:00
draft: true
image: "k8s.png"
tags: "proyectos, k8s, kubernetes, clúster, kubectl, kubeadm"
---

## El proyecto

Voy a trastear con un nuevo clúster k8s, y he decidido desplegarlo con *kubeadm*, nodo a nodo para ir probando algunas cosas. Puede que luego vaya "automatizando" algunas tareas según vea lo que necesite.

<!--more-->

## Documentación

Primero dejaré por aquí los enlaces de la documentación que vaya consultando. Aunque realmente hay cosas de las que me acuerdo y otras que no suelo mirar, creo que es mejor validar muchas cosas antes de hacerlas.

- [instalación de kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)
- [clúster kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/)

## Dónde

Hay muchas maneras de tener un clúster de kubernetes, y muchos sitios para desplegarlo, con herramientas automáticas y con alojamiento o sin el. En este caso me voy a aprovisionar mi propia infraestructura, al menos inicialmente. Recurriré en parte a nodos en *paravirtualización* y en parte a nodos físicos medio volátiles que serán *workers* intercambiables.

Para empezar un sistema de 3 nodos **master** y 3 nodos **worker**, más un nodo para el **load balancer** y algún otro servicio extra que me pueda interesar. Todos tendrán Ubuntu 20.04 como sistema operativo.


| nombre     | ip               | funciones  |
| ------------ |:-------------:| ------------:|
| klb-1         | 172.17.1.201 | load balancer |
| kmaster-1  | 172.17.1.200 | control/master/etcd  |
| kmaster-2  | 172.17.1.201 | control/master/etcd  |
| kmaster-3  | 172.17.1.200 | control/master/ etcd  |
| kworker-1 | 172.17.1.100 | worker  |
| kworker-2 | 172.17.1.101 | worker  |
| kworker-3 | 172.17.1.102 | worker  |


**continuará...**

Saludos
[DarFig](https://github.com/DarFig)