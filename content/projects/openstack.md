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
 

## Dónde

Hace tiempo que utilizo virtualización como base. Simplifica la replicación y manipulación de nodos, así como la gestión de snapshots y backups. He acumulado experiencia en el uso de [xcp-ng](https://xcp-ng.org/), una solución opensource orientada a para-virtualización basada en xenserver; así que lo he visto como mi solución por defecto. Pero en general cualquier otra solución parecida valdrá.


## Empezando

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

**continuará...**

Saludos
[DarFig](https://github.com/DarFig)