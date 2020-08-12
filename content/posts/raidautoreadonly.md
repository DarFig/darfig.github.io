---
title: "Raid en auto-read-only"
date: 2020-08-12T22:09:36+02:00
draft: false
tags: "mdadm, linux, raid"
---

Si manejamos raid software en linux alguna vez nos podemos encontrar con que se nos ha puesto en modo solo lectura.

<!--more-->

### auto-read-only

El típico estado *active (auto-read-only)* que puede aparecer en algún momento. Asumamos que nuestro raid en cuestión es un raid5 llamado **md12**.

```bash
md12 : active (auto-read-only) raid5 sda1[0] sdb1[1] sdc1[2] sdd1[3] sde1[4] sdf1[5] 5555225 [5/5] [UUUUU]

```

La solución a esto es sencilla y rápida. Previamente debemos asegurarnos de que nuestro raid no tenga problemas. Revisar con **mdadm --detail** el estado y cualquier verificación necesaria. Finalmente lo podemos arreglar con un simple comando:

```bash
mdadm --readwrite /dev/md12
```

Podemos verificar que ya no aparece *active (auto-read-only)* en nuestro raid en el fichero */proc/mdstat*.

Saludos
[DarFig](https://github.com/DarFig)