---
layout: default
title: Listar En Docker
permalink: /docker_list/
parent: Docker
nav_order: 3
---

## Ver la version de Docker 

Ejecutemos el comando para listar la version the docker instalado localmente.

```bash
-> docker version
Client: Docker Engine - Community
 Version:           28.0.0
 API version:       1.48
 Go version:        go1.23.6
 Git commit:        f9ced58
 Built:             Wed Feb 19 22:10:30 2025
 OS/Arch:           linux/amd64
 Context:           default
permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.48/version": dial unix /var/run/docker.sock: connect: permission denied
```

Tambien se puede ver de esta manera.
```
-> docker --version
Docker version 28.0.0, build f9ced58bc
```

## Listar Imagenes de Docker

El comando `docker image ls` lista las imagenes de docker presentes en el sistema.

{: .note }
La lista es dinámica y crece a medida que agregamos o borramos imagenes para correr contenedores.

```bash
-> sudo docker image ls
REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
hello-world   latest    74cc54e27dc4   4 weeks ago   10.1kB
```

Definición de terminos basado el la informacíon arriba.

<div class="code-example" markdown="1">
<dl>
<dt>REPOSITORY</dt>
<dd>el repositorio de origen donde localizar la imagen</dd>
<dt>TAG</dt>
<dd>palabra alfanumérica identificando la imagen</dd>
<dt>IMAGE ID</dt>
<dd>suma de sha identificando la imagen internamente en el sistema</dd>
<dt>CREATED</dt>
<dd>fecha cuando la imagen fue creada</dd>
<dt>SIZE</dt>
<dd>tamaño de la imagen en megabytes</dd>
</dl>
</div>

[Return to main page]({{site.baseurl}}/).