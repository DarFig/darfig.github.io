---
title: "Homelab Openstack"
date: 2020-11-08T20:37:34+01:00
draft: false
image: "openstacklog.png"
tags: "proyectos, openstack, cloud, buidyourself"
---

## El proyecto

Llevo un tiempo pensando en desplegar un openstack de manera completa, nada de [devstack](https://docs.openstack.org/devstack/latest/), que es muy útil para conocer el sistema y hacer pruebas de gestión.

<!--more-->

Todo montado desde cero en nodos "reales", más como proyecto de arquitectura/diseño que para probar temas de gestión posterior. Aunque siempre es mejor probar la gestión en un entorno "real" por así decirlo.

## Iniciando

Como todo proyecto hay que empezar por documentarse. He estado consultando bastante información sobre arquitecturas, requisitos, y ejemplos. En general creo que para este tipo de proyectos no hace falta hacer inversiones en libros o similares. Asumiendo que ya se tiene una base de conocimientos asentada cuando se intenta montar algo así. Las principales fuentes están en la propia documentación de openstack. Aunque hay que resaltar que de primeras puede ser algo abrumador, si se lee con calma está todo ahí.

Para empezar:

- https://docs.openstack.org/install-guide/
- https://wiki.openstack.org/wiki/Main_Page

### Otras fuentes de información

Inicialmente no tenía pensado recurrir a otras fuentes de información externas a la documentación que podemos tener todos a mano. Pero tras las primeras pruebas que hice me entró más curiosidad, y empecé a leer algunos libro. Consulté algo de bibliografía, que en general en este tema es escasa o poco accesible (por ejemplo si pensamos en bibliotecas públicas, incluso en la de ingeniería). 

En esta búsqueda estaba interesado, principalmente, en ejemplos de despliegues para ver diferentes configuraciones y utilidades. Terminé encontrando un libro de hace unos años pero que en general me parece bastante interesante si se está empezando en esto. EL libro en cuestión es [Common OpenStack Deployments](https://www.amazon.es/Common-OpenStack-Deployments-Administrators-Engineers-ebook/dp/B01ITNCBVA/ref=sr_1_1?__mk_es_ES=%C3%85M%C3%85%C5%BD%C3%95%C3%91&dchild=1&keywords=Common+OpenStack+Deployments&qid=1610274966&sr=8-1) y creo que responde bastante bien a las dudas que podría tener un administrador de sistemas de toda la vida que de repente se encuentra ante openstack.
 

## Dónde

Hace tiempo que utilizo virtualización como base. Simplifica la replicación y manipulación de nodos, así como la gestión de snapshots y backups. He acumulado experiencia en el uso de [xcp-ng](https://xcp-ng.org/), una solución opensource orientada a para-virtualización basada en xenserver; así que lo he visto como mi solución por defecto. Pero en general cualquier otra solución parecida valdrá.


## Empezando (old)

Para empezar hay que prepara un sistema operativo base. He seleccionado CentOS 8.2 como mi punto de partida. Cuenta con soporte directo para instalar desde los repositorios, y es bastante estable como distribución a largo plazo. Tras instalar CentOS, openstack, y la configuración básica del nodo lo he convertido en una *template* que me valdrá para generar desde aquí nuevos nodos sin reinstalar cada vez el sistema.


```bash
# paquetes en centos 8.2
# openstack ussuri
yum install centos-release-openstack-ussuri
yum config-manager --set-enabled PowerTools
yum upgrade
yum install python3-openstackclient
yum install openstack-selinux
```

La arquitectura básica inicial debe tener como mínimo un nodo controlador y un nodo de cómputo. Nuevamente de cada uno de estos nodos he generado nuevas *templates*, con sus respectivas diferencias en configuración.

Para empezar estos dos nodos tendrán 4 cores, hasta 16GB de ram y hasta 64GB de disco. Cada nodo requiere dos interfaces de red, una como red de gestión y otra como red *externa*

## Cambios importantes

En los últimos días han pasado cosas que me han obligado a replantearme la visión inicial, así como a desechar parte del trabajo realizado. Entre estas está que RedHat da un volantazo y deja sin soporte CentOS 8, y mata básicamente CentOS. Sin entrar en detalles esto me deja con las opciones de opensuse, ubuntu server, o fedora server. En un futuro cercano  [rocky linux](https://rockylinux.org/) podría ser una solución viable como sustituto espiritual de CentOS, pero en el momento de escribir estas palabras es más una idea que un sistema operativo. Analizaré pros y contras y decidiré que usar. En este caso no puedo recomendar ninguna de las otras distribuciones a priori porque sería una opinión altamente subjetiva.

### Otra alternativa para un “despliegue”

Una posible alternativa, si quieres probar cosas en un entorno básico, es utilizar [microstack](https://microstack.run/). Permite crear un pequeño clúster con las funcionalidades básicas que necesitas para hacer pruebas y aprender a trabajar en openstack.

Pero yo quiero entender openstack, no alquilarme a mi mismo algo que no entiendo como está montado, &#129322; .  Comentarios tontos aparte, microstack está demasiado verde aún para ser una opción viable incluso para un clúster de casa. Pero la sencillez con la que se pueden instalar unos cuantos nodos y empezar a levantar máquinas lo hace muy interesante para hacer pruebas en “local”. Espero que en unos años sea una solución más completa.

**continuará...**

Saludos
[DarFig](https://github.com/DarFig)