---
title: "Trabaja en github con autenticación por ssh"
date: 2017-04-23T22:12:01+01:00
draft: false
---

Primero hay que asegurarse de tener ssh(por ejemplo opnessh) instalado.

## Proceso

### Creando una clave ssh

Si no se ha generado anteriormente el primer paso es generar nuestras claves
privada y pública, que se almacenarán en el directorio oculto: ~/.ssh. Podemos
generar nuestras claves mediante:

```shell
  cd ~/.ssh
  ssh-keygen #nos pedirá el nombre de los archivos que lo dejaremos por defecto
             #también una contraseña para cifrar nuestra clave, muy recomendable
  cat id_rsa.pub #esto nos muestra la clave pública

```
El contenido de id_rsa.pub es nuestra clave pública, su contenido en total es
lo que debemos llevar a nuestro perfil de GitHub: dentro de Settings, SSH and GPG keys
creamos una nueva clave ssh y pegamos el contenido.

### Agregando nuestra clave al agente

Para que nuestra autenticación funcione debemos agregar nuestra clave privada
al agente ssh.

```shell
  eval $(ssh-agent -s) #iniciamos el agente de ssh
  ssh-add ~/.ssh/id_rsa #agregamos la clave privada al agente
                        #nos pedirá la contraseña con la que lo ciframos
```

### Trabajando con ssh

La proxima vez que clones un repositorio a local utiliza su dirección ssh, que a
diferencia de la https utiliza un formato **git@github.com:user/repositorio.git**
El resto del funcionamiento es identico, salvo que no tienes que recurrir a la
autenticación por contraseña.
    Si quieres cambiar la dirección de un repositorio en local entre https o ssh
siempre puede recurir a:

```shell
    git remote set-url origin git@github.com:USERNAME/OTHERREPOSITORY.git
    #o
    git remote set-url origin https://github.com/USERNAME/REPOSITORY.git
```

Saludos
[DarFig](https://github.com/DarFig)