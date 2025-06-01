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

## Listar imagenes de Docker en el sistema local 

Lo siguiente muestra como listar todas las imagenes de docker guardadas localmente en nuestra maquina.
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

Con la opcion de `--filter` podemos filtrar la salida del comando para que nos de las imagenes que son mas viejas que el tiempo especificado. Aqui vemos imagenes viejas de mas de 8 horas.
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

Empezemos un contenedor y le damos un nombre.
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

Correr un comando en un contenedor activo, desde el docker host, sin entrar al contenedor.
```
  docker exec <containerID> head /etc/profile
```

Ver los registros para mostrar la historia de actividad del contenedor.
```
  docker logs <containerID>
```

Mostrar todos los ajustes del contenedor.
```
  docker inspect <containerID>
```

Mostrar los contenedore que han corrido, pero ahora estan terminados.
```
  docker ps -a
```

## Empezar Contenedor Con Redireccion De Puertos

Para asignar el puerto 80 al puerto 8081 al empezar un contenedor se hace asi:

```
-> sudo docker run -d -p 8081:80 nginx:latest
```

## Remover un contenedor

{: .warning }
No podemos borrar un contenedor activo.

Para borrar el contenedor solo proveemos el nombre o el id.
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

No es buena practica crear imagenes de docker sin etiqueta. Por esta razon es mejor deshacernos de ellas.
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

## Borrar Todos los Contenedores Activos

Este comando para y borra todos los contenedores activos -- usa esto con cuidado.

```
sudo docker stop $(sudo docker ps -a -q) && \
sudo docker rm $(sudo docker ps -a -q) && \
sudo docker rmi $(sudo docker images -q) -f
```

## Evitando Usar Sudo Con Docker

Al correr comandos de Docker como un usuario regular, podemos ver este tipo de error:

```
permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.50/containers/json?all=1": dial unix /var/run/docker.sock: connect: permission denied
```

Para arreglar eso podemos hacer lo que sigue.

Agrega tu usuario al grupo de docker (solución permanente).
```
-> sudo usermod -aG docker $USER
```

Aplica los cambios -- puede que tengas que salir del sistema y entrar otra vez, o usa este comando.
```
-> newgrp docker
```


## Informacion del Contenedor

Conseguir información acerca de  una imagen en tu biblioteca local.
- que servicios contiene la imagen
- puertos expuestos
- información del ambiente
- version de la aplicacion (per ejemplo, version de nginx, no se puede decir con la etiqueta `latest`)

La salida completa viene con formato JSON que puede ser analyzado con otras herrmientas.
```
-> docker pull nginx:latest
-> docker inspect nginx
```

## Conseguir la Dirección de IP del Contenedor

Para referencia ver este [posteo de Stackoverflow](https://stackoverflow.com/questions/17157721/how-to-get-a-docker-containers-ip-address-from-the-host).

En este ejemplo usamos un id de contenedor imaginario.

```
-> docker inspect 9c729a87fc6f  | grep "IPAddress"
            "SecondaryIPAddresses": null,
            "IPAddress": "",
                    "IPAddress": "172.18.0.3",
```

Tambien podemos usar el nombre del contenedor.

```
-> docker inspect devesp_container  | grep "IPAddress"
```

## Correr on Comando Usando una Imagen

Puedes ejecutar un comando contra una IMAGEN y salir inmediatamente.<br>
El comando se ejecuta, y el contenedor se termina.

```
Sat 2025May31 21:00:58 UTC
devuser@ubuntu2204-allpurpose  git(main)
hist:52 -> sudo docker run ubuntu:22.04 echo "Hiya, there."
Hiya, there.
```

Al consultar con docker ps, se muestra el COMANDO que se ejecutó para ese contenedor.

```
Sat 2025May31 21:01:09 UTC
devuser@ubuntu2204-allpurpose  git(main)
hist:53 -> sudo docker ps -a
CONTAINER ID   IMAGE          COMMAND                 CREATED             STATUS                         PORTS     NAMES
f203fa01eaa5   ubuntu:22.04   "echo 'Hiya, there.'"   10 seconds ago      Exited (0) 8 seconds ago                 pensive_leakey
```

## Verificar si estamos en un contenedor o no

Si por una razón u otra no estamos seguros si estamos en el indicador de un contenedor o no, podemos hacer un chequeo como se muestra en esta sección.

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


Puedes poner este código en un escrito de bash y ejecutarlo.
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