---
layout: default
title: Trucos Técnicos de Docker
permalink: /docker_cheatsheet/
parent: Docker
nav_order: 3
---

# Trucos Técnicos de Docker
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

---

Esta página contiene una lista concisa de trucos a usar con docker. La página puede usarse como referencia para recordar la sintaxis u organización de comandos.

## Listar docker images on your local 

Listar todas las imagenes de docker guardadas localmente en nuestra maquina.
```
Sat 2025Mar08 16:54:53 UTC
devuser@ubuntu2204-3-devesp
~
hist:42 -> sudo docker images
REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
nginx         latest    b52e0b094bc0   4 weeks ago   192MB
hello-world   latest    74cc54e27dc4   6 weeks ago   10.1kB

-> sudo docker image ls
REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
nginx         latest    b52e0b094bc0   4 weeks ago   192MB
hello-world   latest    74cc54e27dc4   6 weeks ago   10.1kB
```

Con la opcion de `--filter` podemos filtrar la salida del comando para que nos de las imagenes que son mas viejas que el tiempo dado. Aqui vemos imagenes viejas de mas de 8 horas.
```
-> sudo docker images --filter "until=8h"
REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
nginx         latest    b52e0b094bc0   4 weeks ago   192MB
hello-world   latest    74cc54e27dc4   6 weeks ago   10.1kB
```

## Empezar un Contendor y Entrar

Empezemos un contenedor
```
  docker start <containerID>
```

Empezemos un contenedor y dar un nombre al contenedor.
```
  docker run --name my_container_name image_name
```

Entrar a la terminal del contenedor.
```
  docker exec -it <containerID> /bin/bash
```

Conectar como root.
```
  docker exec -u 0 -it <containerID> /bin/bash
```

Conectar al contenedor en el TTY activo (no un nuevo SHELL)
```
  docker attach <containerID>
```

Correr un comando a un contenedor activo, desde el docker host, sin entrar al contenedor.
```
  docker exec <containerID> head /etc/profile
```

Mostrar la historia de actividad del contenedor.
```
  docker logs <containerID>
```

Mostrar los ajustes del contenedor.
```
  docker inspect <containerID>
```

Mostrar los contenedore que han corrido, pero ahora estan parados.
```
  docker ps -a
```


## Remover un contenedor

{: .warning }
No podemos borrar un contenedor activo.

Para borrar el contenedor solo proveemos el nombre.
```
  docker rm <containerID>
```

Borrar todos los contenedors inactivos, los que se muestran con `docker ps -a`.
```
  docker rm $(docker ps -a -q)
```

## Remover contenedores viejos

Limpiar el ambiente borrando imagenes viejas y fuera de uso. 
Para referencia ver este [posteo de Stackoverflow](https://stackoverflow.com/questions/17236796/how-to-remove-old-docker-containers).

```
-> docker system prune
```

## Remover imagenes sin etiqueta

```
-> docker images | grep "<none>" | awk '{print $3}' | xargs docker rmi
```

## Borrar imagenes de docker

Borrar todas las imagenes
```
  docker rmi $(docker images -q)
```

Forzar el borrar una imagen.
```
  docker rmi $(docker images -q) --force
```

Listar imagenes que muestran `<none>`
```
  docker images -f "dangling=true" -q
```

Borrar imagenes que muestran `<none>`
```
  docker rmi $(docker images --filter "dangling=true" -q --no-trunc)
```

## Informatcion del Contenedor

Conseguir información acerca de  una imagen en tu libreria local.
- que servicios contiene la imagen
- puertos expuestos
- información del ambiente
- version de la aplicacion (per ejemplo, version de nginx, no se puede decir con la etiqueta `latest`)

La salida completa viene con formato JSON que puede ser analyzado con otras herrmientas.
```
-> docker pull nginx:latest
-> docker inspect nginx
```

## Conseguir la IP Address del Contenedor

Para referencia ver este [posteo de Stackoverflow](https://stackoverflow.com/questions/17157721/how-to-get-a-docker-containers-ip-address-from-the-host).

```
-> docker inspect cb4671987611  | grep "IPAddress"
            "SecondaryIPAddresses": null,
            "IPAddress": "",
                    "IPAddress": "172.18.0.3",
```


## Verificar si estamos en un contenedor o no

Docker crea el archivo vacío `.dockerenv` en la parte superior del árbol de directorios del contenedor, por lo que es posible que quieras verificar si existen.

Para referencia ver este [posteo de Stackoverflow](https://stackoverflow.com/questions/23513045/how-to-check-if-a-process-is-running-inside-docker-container)

Corramos un contenedor de Ubuntu para hacer las observaciones.

```
-> docker run -it ubuntu:latest /bin/bash
[root@9c729a87fc6d /]#
[root@9c729a87fc6d /]# ls -l /.dockerenv
-rwxr-xr-x 1 root root 0 Jan 29 00:36 .dockerenv
[root@9c729a87fc6d /]# cat .dockerenv
[root@9c729a87fc6d /]#
```


Puedes poner este código en un script bash y ejecutarlo.
```
#!/bin/bash
if [ -f /.dockerenv ]; then
    echo "inside a container ;(";
else
    echo "not in a container";
fi
```

Otra alternativa es, mientras se está conectado a un contenedor, ejecutar un comando de una sola línea como el que se muestra a continuación.

Un entorno de contenedor se verá así 
```
-> [ -f /.dockerenv ] &&  echo "this is a container" || echo "this is not a container"
this is a container
```

Un entorno que no sea contenedor se mostrará así:
```
-> [ -f /.dockerenv ] &&  echo "this is a container" || echo "this is not a container"
this is not a container
```

# Iniciar un contenedor de Ubuntu con systemd

Para referencia ver este [posteo de Stackoverflow](https://stackoverflow.com/questions/43122080/how-to-use-init-parameter-in-docker-run).

```
docker run -it --init --rm ubuntu:16.04 /bin/bash
```

[Return to main page]({{site.baseurl}}/).