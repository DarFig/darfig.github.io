---
title: "Configuración rápida de ldap-client"
date: 2020-11-08T17:38:48+01:00
draft: false
tags: "ldap, usuarios, linux"
---

## LDAP

LDAP (Lightweight Directory Access Protocol ) es todo un conjunto de protocolos ampliamente utilizado. En entornos linux concretamente **OpenLDAP** es la implementación más ampliamente usada. En este caso hablaremos de la configuración rápida de un cliente ldap para conectar un nuevo nodo a un servidor openldap. 

<!--more-->

## LDAP client

La instalación de un cliente para ldap es muy sencilla mediante yum, apt, etc. El nombre de los paquetes puede cambiar según nuestra distribución. Por ejemplo en ubuntu server 18.04:

```bash
# instalación
sudo apt install ldap-auth-client nscd
# configuración
sudo auth-client-config -t nss -p lac_ldap

```

Para configuraciones específicas y más avanzadas nos interesará mirar además otros ficheros (más relacionados con la configuración de **pam** ) como:

```bash
/etc/pam.d/common-session # añadir: session required pam_mkhomedir.so skel=/etc/skel umask=077
/etc/pam.d/common-auth
/usr/share/pam-configs/mkhomedir
```

Finalmente no olvidemos reiniciar los servicios para aplicar los cambios correctamente:

```bash
# reconfigurar el pam
sudo pam-auth-update
# reiniciar nscd
sudo systemctl restart nscd # o /etc/init.d/nscd restart
sudo systemctl enable nscd
```

Saludos
[DarFig](https://github.com/DarFig)