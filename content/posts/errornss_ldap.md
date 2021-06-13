---
title: "Error nss_ldap"
date: 2021-06-13T11:57:29+02:00
draft: false
tags: "ldap, linux, nss, libnss, libnss-ldap, libnss-ldapd"
---

Trabajando con libnss-ldap en un servidor ubuntu me he encontrado recientemente un error que puede dar más de un quebradero de cabeza pero realmente es muy sencillo de solucionar.

<!--more-->

## El error

Mirando los logs me he encontrado lo siguiente:

```bash
nss_ldap: could not connect to any LDAP server as (null) - Can't contact LDAP server
```

Seguido de algún   que otro fallo más. Aunque todo parece funcionar bien, el fallo es recurrente y está afectando algunos servicios.

## Una posible solución rápida

Después de leer un poco decidí probar a sustituir libnss-ldap con libnss-ldapd.

```bash
apt install libnss-ldapd
systemctl restart systemd-logind
```

Tras la instalación pedirá reconfigurar el servicio, pero si ya teníamos configurado correctamente ldap deberían salirnos las mismas opciones que ya teníamos seleccionadas.


Saludos
[DarFig](https://github.com/DarFig)