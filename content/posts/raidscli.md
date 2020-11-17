---
title: "CLIs para tarjetas raids"
date: 2020-09-13T22:04:39+02:00
draft: false
tags: "raid, discos, linux,CLI"
---

En el mercado hay varias marcas de controladoras para raid hardware, pero en general suelen funcionar de manera similar a la hora de configurar o sustituir los discos.

Normalmente te permiten un acceso en el arranque del servidor para configurar. Pero una vez está todo funcionando no queremos recurrir a este método para sustituir un disco, porque nos obliga a parar todos los servicios. Entonces nos suelen quedar dos opciones una aplicación con acceso web, o la vieja y fiable línea de comandos, normalmente con algún programa del fabricante.

<!--more-->

## El CLI

La interfaz web está muy bien y es muy cómoda según la tarea que tengamos que hacer. Pero realmente en muchos casos no necesitamos tanto. Por ejemplo, si tenemos discos en *pass-through*, para sustituir un disco no necesitamos realmente la web. Podemos recurrir al **CLI** y acabar con un par de comandos.

La mayor parte de los fabricantes nos proporcionan su propio programa **CLI**, con todos los comandos necesarios para hacer el trabajo. Por ejemplo, si tenemos una tarjeta de Areca una ARC-1680, ARC-1120, etc, podemos ir a su página web y descargarnos el **CLI** de Areca. Este está disponible según nuestro modelo y sistema operativo. Esto funciona de la misma manera con otros fabricantes, si el software es cerrado, que es común, nos brindarán un binario.

### Funcionamiento

El software suele venir preparado para lanzarlo desde nuestra consola y dejarnos con un paquete de herramientas completo. Lo más usual es que **help** sea lo primero que usemos. Y aunque muchas veces el fabricante nos deja algún manual con documentación. Este comando muchas veces sea más útil que 80 páginas sobre las distintas opciones que, salvo casos específicos, no necesitaremos.

Otros comandos comunes suelen estar relacionados con ver información de discos, volúmenes o de los raids presentes, cosas del tipo **disk info, vol info, raid info**. De manera similar **disk create, etc** para crear o **disk del** o **disk delete** para borrar.

A estos comandos hay que pasarles la información del dispositivo que queremos utilizar. Este identificador lo obtendremos usando el comando de información.

Un punto importante a tener en cuenta es que las tarjetas raids suelen tener contraseña, o por defecto del fabricante o uno que pongamos en la primera configuración. Normalmente una aplicación web nos pedirá un usuario y contraseña para conectarnos. Pero en **CLI** puede que sólo se nos solicite para realizar acciones restringidas: crear, borrar, etc. Pero puede que no nos la solicite directamente, sino que de error. En este caso también tendremos un comando para poner la contraseña. En el caso de Areca por ejemplo será **set password=la contraseña**.


### Un ejemplo rápido

Tenemos nuestra tarjeta y un disco en *pass-through* ha fallado. Ya lo cambiamos físicamente, pero el servidor sigue sin verlo. Pues nos descargamos el CLI y en unos pocos comandos está todo listo. Veamos, en este ejemplo usaré los comandos del CLI de areca:

- Lo primero es saber que discos tenemos sin usar **disk info** nos dejará ver una lista de los dispositivos y a donde están asignados. Como nuestro disco nuevo no está asignado será fácil de identificar.
- Normalmente sale una tabla de la que nos interesa el identificador de disco, en este caso es un número **drv** que utilizaremos a continuación.
- Si ahora intentamos crear algo nos devolverá un fallo, así que primero **set password=menudacontraseña**
- ahora si creamos nuestro nuevo disco en *pass-through*, en el caso de areca sería **disk create drv=x** donde x es el identificador que vimos antes. De manera similar para crear un raid tendríamos **raid create** y una lista de drv, además de las opciones de tipo de raid, etc.
- podemos verificar nuevamente que todo esté en su sitio con **disk info**.
- si todo está bien salimos **exit** y nuestro nuevo disco debería verse en el sistema como uno más en */dev*

### Finalmente

Estas herramientas que nos proporcionan los fabricantes son bastante completas. Realmente no está de más consultar toda la documentación disponible antes de hacer nada. Simplemente para crear un raid tenemos varios argumentos, algunos obligatorios y otros opcionales que pueden ser muy interesantes conocer por adelantado. Estos muchas veces no se ven claramente con *help*, pero sí echándole un vistazo al manual. Esto nos ahorrará tiempo y errores.


Saludos
[DarFig](https://github.com/DarFig)