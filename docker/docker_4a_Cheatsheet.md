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

## Etiquetas de Docker

Obtén la ayuda del comando de esta manera:
```
-> docker tag --help
```

Etiqueta las imágenes así:
```
-> docker tag <IMAGE ID> custom/ubuntu:v1.0
-> docker tag <IMANGE NAME> custom/ubuntu:v1.0
```

Primero veamos los detalles the las imagenes.
```
-> docker images
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
ubuntu       custom    17249a388181   22 minutes ago   141MB
ubuntu       22.04     c42dedf797ba   4 weeks ago      77.9MB
```

Ejemplo de etiquetar una imagen usando el ID del contenedor.<br>
```
-> docker tag 17249a388181 custom/ubuntu:v1.0
```

Ejemplo de etiquetar una imagen usando el nombre de la imagen
```
-> docker tag custom/ubuntu:v1.0 ubuntu2204-allpurpose/ubuntu:v1.0.1b
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

## Ejecutar un contenedor separado

Aquí discutimos cómo ejecutar un contenedor y mantenerlo en ejecución sin que se detenga.

Si ejecutas un contenedor de esta manera, se cierra inmediatamente.
```
-> docker run -d ubuntu:22.04 /bin/bash
```

Cuando ejecutas un contenedor con un comando como /bin/bash, se iniciará y luego se cerrará inmediatamente porque el proceso bash se ejecuta de forma interactiva o como un proceso en primer plano, pero sin ningún comando que lo mantenga vivo o sin una terminal interactiva conectada.

Puedes iniciar una sesión bash interactiva de este modo:
```
-> docker run -it ubuntu:22.04 /bin/bash
```
Esto mantendrá el contenedor ejecutándose de forma interactiva hasta que salgas. Con este comando aterrizaz en el indicador del contenedor.

Si deseas que el contenedor se ejecute en modo desconectado usando `-d` y permanezca activo, puedes sobrescribir el comando con algo que no se cierre inmediatamente, como `tail -f /dev/null`:
```
-> docker run -d ubuntu:22.04 /bin/bash -c "tail -f /dev/null"
```
De esta manera, el contenedor se ejecuta en segundo plano y permanece activo.

Una tercera forma es ejecutar un comando que se repita indefinidamente:
```
-> docker run -d ubuntu:22.04 /bin/bash -c "while true; do sleep 600; done"
```
Esto mantiene el contenedor en ejecución mediante un bucle de espera.

Talvez la manera mas efectiva es correr el contenedor en segundo plano mientras mantenemos acceso al indicador del docker host. Esto se logra usando las banderas `-dit` como vemos aqui:
```
-> docker run -dit ubuntu:22.04 /bin/bash
```

## Inspeccionando Eventos de Docker

Todos los eventos de Docker se registran cuando detienes, inicias o matas un contenedor.

Puedes monitorear los eventos de Docker en tiempo real pasando el argumento `events` a docker. <br>
Esto es similar a usar `tail -f logfile.log` en Linux.

Ver la ayuda del commando para eventos
```
docker events since --help
```

Mostrar lo que ha sucedido en la última hora.
```
-> docker events --since '1h'
```

Lo mismo que arriba, pero solo eventos por los ultimos 30 minutos.
```
docker events --since '30m'
```

{: .note }
generalmente no se admite la especificación directa de segundos (p. ej., "30 s") en el parámetro `--since`. El uso más común implica minutos, horas o marcas de tiempo específicas.


(i) Si el indicador en tu sistem muestra la fecha, la marca de tiempo en el indicador se vuelve útil para relacionarla con los registros de eventos.

¿Qué pasa si solo queremos ver eventos de conexión (`attach`)?
Puedes iniciar el escaneo de eventos, filtrando por el evento `attach`.
```
docker events --filter event=attach --since '1h'
```

Agrega más filtros como este para ver más eventos.

```
-> docker events --filter event=attach --filter event=die --filter event=stop --since '1h'
```

## Inspeccionar el Historial de una Imagen Docker

Puede ser útil saber qué se utilizó para construir una imagen de Docker.
Podemos usar la opción `history` con otras banderas para obtener la información histórica.

Cada entrada representa una capa utilizada para crear la imagen.
```
-> docker history ubuntu:custom 
```

Utiliza la bandera de no truncamiento para ver los comandos completos utilizados.
De interés es el sha256 completo de la imagen.
```
-> docker history --no-trunc ubuntu:custom
```

Ver solo el ID de la IMAGEN con una cadena SHA corta.
```
-> docker history --quiet ubuntu:custom
```
Lo mismo que el anterior, pero con la cadena SHA larga completa.
```
-> docker history --quiet --no-trunc   ubuntu:custom
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