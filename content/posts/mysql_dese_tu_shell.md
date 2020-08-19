---
title: "Mysql desde tu shell"
date: 2017-03-13T22:08:06+01:00
draft: false
tags: "bbdd, linux, mysql"
---

## De que va esto

Levantar un servidor **MySQL** para tus proyectos es tan sencillo como tener cualquier distribución GNU/linux o similares y asegurarte de seguir algunos pasos sencillos.

<!--more-->

## Primero lo primero
##### **Instalando**

Segun tu sistema(y si no lo tienes instalado) tendrás que utilizar:

```shell
  apt-get -y install mysql-server #instalar
  service mysql start #añadir al arranque
```

```shell
  yum install mysql-server #instalar
  /sbin/service mysqld start #arrancar
  chkconfig mysqld on #añadir al arranque
```

O similares para otros sistemas.

## Pasemos al uso
### Configuración

Primeramente el root tendrá los permisos y deberás crear las bases y configurar los permisos para otros usuarios usando esta cuenta.

```shell
  mysql -u root -p #acceso  con la cuenta root
```

**Ahora pasamos a un poco de mysql**

```mysql
  CREATE DATABASE nombreEjemplo; --nueva base
  GRANT USAGE ON nombreEjemplo.* TO nombreUsuario@localhost IDENTIFIED BY 'password'; --credenciales para otro usuario
  GRANT ALL PRIVILEGES ON nombreEjemplo.* TO nombreUsuario@localhost ; --cuidado con los privilegios, esto es un ejemplo
  FLUSH PRIVILEGES;
```

Ya lo tenemos configurado, ahora podemos acceder con nuestras *nuevas credenciales* y listo.

```shell
  mysql -u nombreUsuario -p'password' 'nombreEjemplo'
```

Saludos
[DarFig](https://github.com/DarFig)
