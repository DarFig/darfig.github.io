---
title: "Clonar Discos en Linux"
date: 2020-04-30T11:00:33+02:00
draft: false
tags: "discos, linux, dd"
---

Ya sea porque necesitas una copia o quieres sustituir una unidad de almacenamiento, en algún momento puedes necesitar clonar una unidad de almacenamiento en linux, tanto estructura como datos.

<!--more-->

## Clonar discos con dd

### Cosas a considerar

Mi método favorito es el comando **dd**, no requiere instalar más herramientas y permite tanto copiar unidades completas como particiones. Pero así mismo se debe manejar con mucho **cuidado**. Un error con este comando puede borrar totalmente tu unidad de almacenamiento. El error más común es no tener claro cual es la unidad origen y cual la destino de la copia y ponerlas en sentido inverso.

Otro buen aliado puede ser clonezilla, o herramientas similares, pero no es el tema en el que me voy a centrar.

Un dato que suelen olvidar algunos es que **dd** trabaja con ficheros, es decir convierte y copia ficheros. Lo cual quiere decir que se puede leer del descriptor de un disco y almacenarlo en un simple fichero como imagen copia (por ejemplo copiasda.img). Así podemos crear backups completos en unidades de red e incluso automatizarlo sin recurrir a herramientas más complejas (aunque un buen sistema de copias de seguridad incluye muchas más opciones y herramientas cuya importancia no debemos ignorar).

Lógicamente para copiar el disco el destino debe ser igual o mayor en capacidad que el origen. En caso de hacer una copia completa también hay que tener en cuenta que se copiará todo, las particiones y los espacio vacíos. Si queremos copiar trozos, que no coinciden con particiones, recomiendo leer en la documentación sobre las opciones *bs* y *count* para la copia por bloques.

### Obtener información

Para asegurarnos de si el disco es origen o destino nos podemos valer de las típicas herramientas de consola. Principalmente **lsblk** y **fdisk -l** así sabremos las unidades de almacenamiento, las particiones y sus tamaños, así como otros datos que nos pueden ayudar a orientarnos.

### Clonando

El comando **dd** tiene una sintaxis simple **dd if=origen of=destino** y requiere permisos así que utilizaremos sudo o root. Una opción que puede ser interesante tener en cuenta es *bs* que define los bytes que se utilizarán para leer y escribir cada vez. Es como definir un tamaño de bloque y cada operación de lectura/escritura será de 1 bloque. Esto quiere decir que bloques grandes disminuirán el tiempo de copia pero también disminuirán la precisión, lo que puede provocar errores o perdidas de datos. El tamaño por defecto es de 512 bytes.

Así, si queremos copiar sda en sde nos quedaría:

```bash
sudo dd if=/dev/sda of=/dev/sde
# o
sudo dd if=/dev/sda of=/dev/sde bs=64
# cuando termine se pude comprobar la copia
sudo fdisk -l /dev/sda /dev/sde

```


### Puede ser interesante 

Si copiamos con origen /dev/null, /dev/zero, o similar borraremos todo el contenido del destino. Pero puede ser útil.

Por ejemplo si queremos crear un fichero de swap:

```bash

sudo dd if=/dev/zero of=/swapfile bs=4k count=2048M
mkswap /swapfile
swapon /swapfile

```

Saludos
[DarFig](https://github.com/DarFig)