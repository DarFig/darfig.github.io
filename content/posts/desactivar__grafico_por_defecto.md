---
title: "Desactivar el incio de entorno gráfico por defecto"
date: 2017-04-28T22:12:57+01:00
draft: false
tags: "linux"
---

En entorno de servidor, o donde queramos aprovechar el rendimiento en una aplicación específica, nos interesa asegurarnos que el arranque de nuestro sistema linux no sea en entorno gráfico por defecto. Pero en algunos casos podemos requerir de este entorno para momentos muy específicos. De esta manera llegué a la duda de como desactivar el entorno gráfico pero dejarlo instalado para cuando lo necesite.

<!--more-->

## Iniciamos
### Modificando opciones de arranque

Necesitaremos tener permisos ya sea como root, o con acceso a sudo. Entramos en la consola y modificaremos con nuestro editor favorito (vi, nano, etc) el fichero **/etc/inittab**. Una de las primeras lineas es **id:2:initdefault:**, o algo similar y lo sustituimos por  **id:3:initdefault:**, básicamente es cambiar el 2 por el 3. XD


Ahora modifiquemos el gestor de sesión. Cambiamos de directorio a **/etc/rc3.d**. Si ejecutamos un **ls -l** o  **ls -al** , veremos dependiendo de si tenemos GNOME, XFCE, u otros uno de los siguientes archivos: **S19gdm3**, **S04lightdm** o similares, y debemos sustituir la **S** por una **k**.

```shell
  mv S19gdm3 K19gdm3 #según sea el caso
  mv S04lightdm  K04lightdm

```

Con esto iniciaremos por defecto en linea de comandos. Podremos arrancar el entorno gráfico en cualquier momento utilizando el comando **startx**, o ejecutando nuestro gestor por ejemplo **/etc/init.d/gdm3 start**.




Saludos
[DarFig](https://github.com/DarFig)
