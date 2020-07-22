---
title: "mdmonitor"
date: 2020-07-22T20:19:24+02:00
draft: false
tags: "demond, linux, raid"
---

Un error común entre novatos al utilizar **md** para sus raids software es configurar incorrectamente o no configurar el *daemon* de *md*, también llamado *mdmonitor*.

<!--more-->

### mdmonitor

El modo --monitor de *mdadm* corre de manera continua con el objetivo de detectar problemas en nuestro RAID y alertarnos. Su comportamiento será según tengamos configurado nuestro **mdadm.conf**. Este fichero no es requerido para el funcionamiento de *mdadm*. Pero si lo es si queremos guardar la configuración de nuestro RAID, y que se automonte en el arranque. Veamos la configuración básica que necesitamos.

#### mdadm.conf

Este fichero se puede almacenar en */etc/mdadm/mdadm.conf* o en */etc/mdadm.conf*. No se genera de manera automática, así que tendremos que añadir ahí nuestras configuraciones cuando generemos nuestro raid.

Para guardar la confuiguración de nuestros raids basta con añadir a nuestro fichero la salida del comando:

```bash
mdadm --detail --scan 
```

**Con esto no basta**

Hasta ahora hemos dicho que hay que monitorizar, pero no como nos alertan. 

La configuración válida más sencilla requiere configurar las alertas por correo. Basta con añadir al inicio del *mdadm.conf*:

```bash
MAILADDR ejemplo@ejemplo.com
```

También es válida una cuenta local, por ejemplo podemos enviar los avisos al root y se generará un fichero en su cuenta con las alertas.

```bash
MAILADDR root
```

Entonces tendríamos un fichero básico de configuración que podrñia verse así por ejemplo:


```bash
# instruct the monitoring daemon where to send mail alerts 
MAILADDR root

# definitions of existing MD arrays
ARRAY /dev/md/raid1 metadata1.2 name=host1:raid1 UUID=b7de2fc:60b303ac:3c176049:dc5b6c8b
```


### Arrancando el demonio

Ahora sí podríamos habilitar nuesto *mdmonitor* y activarlo para que empiece a trabajar.

```bash
# por ejemplo en centos
systemctl enable mdmonitor.service
systemctl start mdmonitor.service
```

*Como es obvio hay más opciones disponibles, podemos consultarlas en la documentación. Algunas muy interesantes. Pero esto es lo mínimo necesario para que nuestro mdmonitor funcione.*
