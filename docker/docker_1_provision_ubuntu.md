---
layout: default
title: Provisionar Docker en Ubuntu
permalink: /docker_provision/
parent: Docker
nav_order: 1
---

# Instalacion y Configuración de Docker en Ubuntu
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
## Instalar Docker

Uno de los metodos de instalación es el [escrito de conveniencia](https://docs.docker.com/engine/install/ubuntu/#install-using-the-convenience-script) que Docker hace disponible.
Usemos el escrito de conveniencia para instalar Docker

```bash
-> curl -fsSL https://get.docker.com -o get-docker.sh
```

El comando CURL baja el escrito de instalación a nuestro sistema local.
```bash
->  ls -l
total 24
-rw-rw-r-- 1 devuser devuser 22592 Feb 25 05:37 get-docker.sh
```

Corramos el escrito usando `sudo`.
```bash
-> sudo sh get-docker.sh
# Executing docker install script, commit: 4c94a56999e10efcf48c5b8e3f6afea464f9108e
+ sh -c apt-get -qq update >/dev/null
+ sh -c DEBIAN_FRONTEND=noninteractive apt-get -y -qq install ca-certificates curl >/dev/null
+ sh -c install -m 0755 -d /etc/apt/keyrings
+ sh -c curl -fsSL "https://download.docker.com/linux/ubuntu/gpg" -o /etc/apt/keyrings/docker.asc
+ sh -c chmod a+r /etc/apt/keyrings/docker.asc
+ sh -c echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu jammy stable" > /etc/apt/sources.list.d/docker.list
+ sh -c apt-get -qq update >/dev/null
+ sh -c DEBIAN_FRONTEND=noninteractive apt-get -y -qq install docker-ce docker-ce-cli containerd.io docker-compose-plugin docker-ce-rootless-extras docker-buildx-plugin >/dev/null
================================================================================
To run Docker as a non-privileged user, consider setting up the
Docker daemon in rootless mode for your user:

    dockerd-rootless-setuptool.sh install

Visit https://docs.docker.com/go/rootless/ to learn about rootless mode.


To run the Docker daemon as a fully privileged service, but granting non-root
users access, refer to https://docs.docker.com/go/daemon-access/

WARNING: Access to the remote API on a privileged Docker daemon is equivalent
         to root access on the host. Refer to the 'Docker daemon attack surface'
         documentation for details: https://docs.docker.com/go/attack-surface/
================================================================================
```

## Ver la Version de Docker

Veamos la version de Docker que hemos instalado.

```bash
-> docker --version
Docker version 28.0.0, build f9ced58
```

Al usar `sudo` podemos ver una version extendida de la version de Docker.

```bash
->  sudo docker version
Client: Docker Engine - Community
 Version:           28.0.0
 API version:       1.48
 Go version:        go1.23.6
 Git commit:        f9ced58
 Built:             Wed Feb 19 22:10:30 2025
 OS/Arch:           linux/amd64
 Context:           default

Server: Docker Engine - Community
 Engine:
  Version:          28.0.0
  API version:      1.48 (minimum version 1.24)
  Go version:       go1.23.6
  Git commit:       af898ab
  Built:            Wed Feb 19 22:10:30 2025
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.7.25
  GitCommit:        bcc810d6b9066471b0b6fa75f557a15a1cbf31bb
 runc:
  Version:          1.2.4
  GitCommit:        v1.2.4-0-g6c52b3f
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

## Manejar  el Servicio de Docker

Antes de poder inicializar contenedores, debemos asegurarnos que el servicio de Docker esta corriendo.

Los comandos siguientes se usan para empezar, terminar o ver el estatus del servicio correspondientemente.
```bash
-> sudo service docker start
-> sudo service docker stop
-> sudo service docker status
```

Ahora empezemos el servicio de Docker.
```bash
-> sudo service docker start
```

Verifiquemos que el servicio esta corriendo.
```bash
-> sudo service docker status
● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2025-02-25 05:40:06 UTC; 3s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 9200 (dockerd)
      Tasks: 11
     Memory: 21.2M
        CPU: 346ms
     CGroup: /system.slice/docker.service
             └─9200 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Feb 25 05:40:06 ubuntu2204-3-devesp dockerd[9200]: time="2025-02-25T05:40:06.723813195Z" lev>
Feb 25 05:40:06 ubuntu2204-3-devesp dockerd[9200]: time="2025-02-25T05:40:06.723925050Z" lev>
Feb 25 05:40:06 ubuntu2204-3-devesp dockerd[9200]: time="2025-02-25T05:40:06.728691643Z" lev>
```

## Poner la Instalación a Prueba

Corramos un contenedor para poner a prueba el entorno de Docker.

{: .highlight }
Esta acción bajará la imagen de Docker necesaria si no esta disponible localmente.

```bash
-> sudo docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
e6590344b1a5: Pull complete
Digest: sha256:e0b569a5163a5e6be84e210a2587e7d447e08f87a0e90798363fa44a0464a1e8
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

La salida arriba indica que hemos corrido un contenedor básico con exito. Como parte del proceso, Docker ha bajado la imagen porque no la encontro localmente. Si corremos otro contenedor usando la misma imagen sera mas rápido porque la imagen de Docker se encuentra ya en el sistema local.

Veamos ahora la imagen de docker que bajamos al correr el contenedor arriba.
```bash
-> sudo docker image ls
REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
hello-world   latest    74cc54e27dc4   4 weeks ago   10.1kB
```

Veamos el contenedor que ya ha terminado su proceso.

```bash
-> sudo docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED                  STATUS                              PORTS     NAMES
ebd8ac02d3d3   hello-world   "/hello"   Less than a second ago   Exited (0) Less than a second ago             awesome_clarke
```

## Problemas con el Servicio de Docker

Es posible que veamos el error siguiente al atentar correr un contenedor. 

```bash
-> docker run hello-world
docker: Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
```

El error simplemente indica que el service de Docker no esta corriendo y tenemos que empezarlo.
```bash
-> sudo service docker start
```

Una vez empezado el servicio, podemos correr el comando para empezar el contenedor.

[Return to main page]({{site.baseurl}}/).