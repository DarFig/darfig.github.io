---
title: "Deepops slurm y otras cosas"
date: 2021-01-17T11:00:29+01:00
draft: true
tags: "nvidia, linux, deepops, dgx, slurm"
---

Voy a contar el resumen de lo que he pasado los últimos días para desplegar [slurm](https://slurm.schedmd.com/documentation.html), utilizando la herramienta [deepops](https://github.com/NVIDIA/deepops). Pero lo voy a utilizar sólo como un ejemplo para hablar de los problemas que puedes encontrarte con otras herramientas similares.

<!--more-->

## El contexto

En resumen **slurm** es una herramienta para gestionar cargas de trabajo, basado en colas, por ejemplo en un clúster  de súper computación.  Que a su vez se ha incluido en la herramienta **deepops**, una serie de playbooks de [ansible](https://www.ansible.com/) y scripts pensados para ayudar en la configuración y despliegue de software en los servidores, dgx de nvidia, y similares. Esto es una gran idea, tienes un servidor y una serie de herramientas que te ayudan a desplegar software siguiendo lo que podríamos llamar “buenas prácticas”.

Así que me dispuse a utilizar **deepops** 20.11 para desplegar **slurm**, siguiendo las recomendaciones de la [documentación](https://github.com/NVIDIA/deepops/tree/master/docs/slurm-cluster) .

## Las sorpresas

Las primeras sorpresas que me encontré fueron enlaces rotos, y poca documentación sobre las variables que debía configurar si decidía modificar un poco el “guión” que viene pre-establecido.  Claro que si me leo los *playbooks* me voy a enterar de lo que hace cada tarea, pero se supone que el objetivo no es que yo haga ingeniería inversa, sino que  me lea una documentación de como utilizar la herramienta. 

Creo que aquí mi confusión es por venir enlazado desde la documentación de nvidia y ver que el repositorio estaba bajo el perfil de nvidia, cuando la herramienta claramente la mantiene la comunidad. A la que sin duda hay que estarle muy agradecidos porque hace un gran trabajo. Este tipo de situaciones se hacen cada vez más comunes. Es normal que si llegas a un repositorio, mantenido por la comunidad, en el cual hay mucho movimiento y se lanzan mejoras cada poco tiempo, la documentación no esté al día. Nos toca poner un poco de nuestra parte y respetar el trabajo que hacen, muchas veces de manera voluntaria, esas personas. Incluso si lo vemos claro podemos poner de nuestra parte actualizar la versión y enviar un *pull request*.

Una vez entendido esto, voy  a comentar errores comunes que podemos encontrarnos con este tipo de herramientas y que pueden darnos algún quebradero de cabeza. 


