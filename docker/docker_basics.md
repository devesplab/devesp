---
layout: default
title: Docker Básico
permalink: /docker_basics/
parent: Docker
nav_order: 2
---

# Uso Básico de Docker en Ubuntu

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
## Como Empezar Docker

Pues bién, hemos instalado Docker en nuestro sistema. Para simplificar las cosas, supongames que hemos instalado Docker a un sistem de Ubuntu.

Y ahora que hacemos? Cómo lo usamos? 

Lo primero que hacemos es asegurarnos que el servicio de Docker esta corriendo.

Entremos el comando para ver es estado del serivicio. Si dice `inactive (dead)` indica que no esta corriendo.

```
-> sudo service docker status
○ docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: inactive (dead) since Tue 2025-03-04 04:56:58 UTC; 5s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
    Process: 73 ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock (code=exited, status=0/SU>
   Main PID: 73 (code=exited, status=0/SUCCESS)
        CPU: 461ms
```

Lo que hacemos a seguir es entrar el comando para empezar el servicio.
```
-> sudo service docker start
```

Ahora estamos listo para correr contenedores... pero cuál contenedor? Primero tenemos que decidir lo que queremos hacer. Por ejemplo, correr hacer una prueba simple de correr un sitio web con Nginx.

## Buscar Imagenes de Docker

Para correr un contenedor necesitamos un Imagen de Docker. Una imagen de Docker es un paquete de software liviano, independiente y ejecutable que incluye todo lo necesario para ejecutar un programa, incluido el código, el entorno de ejecución, las bibliotecas, las variables de entorno y los archivos de configuración. Las imágenes de Docker se utilizan para crear contenedores, que son las instancias en ejecución de estas imágenes.

Empezamos por buscar las imagenes disponibles de Nginx. Para ello usamos el comando con la sintáxis siguente:
```
sudo docker search <nombre-de-imagen>
```

Al ejecutar la busqueda, veremos un lista corta o larga. Aqui vemos una lista parcial de la imagenes disponibles de Nginx.
```
-> sudo docker search nginx
NAME                                     DESCRIPTION                                     STARS     OFFICIAL
nginx                                    Official build of Nginx.                        20633     [OK]
nginx/nginx-ingress                      NGINX and  NGINX Plus Ingress Controllers fo…   100
nginx/nginx-prometheus-exporter          NGINX Prometheus Exporter for NGINX and NGIN…   47
nginx/unit                               This repository is retired, use the Docker o…   65
nginx/nginx-ingress-operator             NGINX Ingress Operator for NGINX and NGINX P…   2
nginx/nginx-quic-qns                     NGINX QUIC interop                              1
(...snip...)
```

La salida del comando tiene varias columnas:

NAME
: el nombre de la imagen

DESCRIPTION
: desciption concisa de las capabilidades the la imagen

STARS
: número indicativo de la popularidad de la imagen

OFFICIAL
: marca designando la imagen oficial del producto, posiblemente lanzada por la compañia originadora

## Bajando una Imagen de Docker 

De la lista anterior de imagenes de Ngins, decidimos escoger la imagen oficial `nginx`.<br>
Pero tambien necesitamos saber la etiqueta de la imagen deseada.<br>
Vamos pues a [https://hub.docker.com/_/nginx] y vemos una larga lista de etiquetas alpha numericas.

{: .note }
En Docker, una etiqueta es un rótulo que se utiliza para identificar distintas versiones de una imagen de Docker. Las etiquetas permiten gestionar y diferenciar entre distintas compilaciones o iteraciones de una imagen. El sistema de etiquetado es especialmente útil para controlar las versiones y garantizar que se puedan implementar o hacer referencia a versiones específicas de una imagen con facilidad. 

Por simplicidad escogemos la etiqueta `latest`, o lo ultimo.

```
->  docker  pull nginx:latest
```

## Corriendo un Contenedor

Pero no tenemos que bajar la imagen de antemano. Podeos correr el comando como vemos a seguir.

```
-> sudo docker run --name ngnix-local -d -p 8080:80 nginx:latest
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
7cf63256a31a: Pull complete
bf9acace214a: Pull complete
513c3649bb14: Pull complete
d014f92d532d: Pull complete
9dd21ad5a4a6: Pull complete
943ea0f0c2e4: Pull complete
103f50cb3e9f: Pull complete
Digest: sha256:9d6b58feebd2dbd3c56ab5853333d627cc6e281011cfd6050fa4bcf2072c9496
Status: Downloaded newer image for nginx:latest
ea6a1e952b04e9f4b4af461a3b77c4bac826532aa6831b99d75e1a112f25999a
```

Vemos que docker bajó la imagen porque no la encontro localmente. Para otros intentos que sigan, la imagen ya estara disponible localment y no sera necesario bajar la imagen otra vez.

Enseguida veamos el contenedor que se ha inicializado.
```
-> sudo docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                  NAMES
ea6a1e952b04   nginx:latest   "/docker-entrypoint.…"   14 minutes ago   Up 14 minutes   0.0.0.0:8080->80/tcp   my-nginx

```

Ahora podemos probar acceso a Nginx.
La salida del comando `curl` muestra `HTTP/1.1 200 OK` lo cual indica que el sitio esta corriendo.

```
-> curl -sI 172.45.4.5:8080
HTTP/1.1 200 OK
Server: nginx/1.27.4
Date: Tue, 04 Mar 2025 05:33:00 GMT
Content-Type: text/html
Content-Length: 615
Last-Modified: Wed, 05 Feb 2025 11:06:32 GMT
Connection: keep-alive
ETag: "67a34638-267"
Accept-Ranges: bytes
```

Ahora podemos hacer login al contenedor
```
-> sudo docker exec -it ngnix-local /bin/bash
```

Enseguida podemos chequear la configuracion de Nginx.
```
root@ea6a1e952b04:/# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

Tambien podemos chequer los archivos de registro
```
root@ea6a1e952b04:/#  tail -f /var/log/nginx/access.log

root@ea6a1e952b04:/# tail -f /var/log/nginx/error.log
```

Ahora salgomos del contenedor
```
root@ea6a1e952b04:/# exit
```

Y terminemos el contenedor
```
-> sudo docker stop ngnix-local
```

Por ultimo verifiquemos que el contenedor no esta corriendo. Si el contenedor no esta listado, indica que no esta corriendo. 
```
-> sudo docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

El ejemplo que hemos visto muestra el flujo típico de correr un contendor de Docker.<br>
Empezamos for decidir lo que queremos hacer, luego buscar la imagen, correr el contendor, entra al contenedor, salir de el, y por ultimo terminarlo.

[Return to main page]({{site.baseurl}}/).