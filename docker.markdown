---
layout: page
title: Docker
permalink: /docker/
nav_order: 2
has_children: true
has_toc: false
---

[comment]: # (Adds topnav bar above the main image)
<div class="topnav">
 <a class="active" href="../index">Home</a>
 <a href="#news">News</a>
 <a href="../about">About</a>
</div> 

# Docker
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

En este document hablaremos the Docker.
_El objectivo es enternder la manera de usar esta herramienta para crear un ambiente para probar varios aspectos de Docker._

## Listar la version de Docker 

Ejecutemos el comando para listar la version the minikube instalada localmente.

```bash
-> docker version
Client:
 Cloud integration: v1.0.29
 Version:           20.10.22
 API version:       1.41
 Go version:        go1.18.9
 Git commit:        3a2c30b
 Built:             Thu Dec 15 22:28:41 2022
 OS/Arch:           darwin/amd64
 Context:           default
 Experimental:      true
```

Tambien se pueded listar de esta manera.
```
-> docker --version
Docker version 20.10.22, build 3a2c30b
```

## Listar Imagenes de Docker

El comando `docker image ls` lista las imagenes de docker presentes en el sistema.

{: .note }
La lista es dinámica y puede cambiar a medida que agregamos o borramos imagenes

```c
-> docker image ls
REPOSITORY                                      TAG              IMAGE ID       CREATED         SIZE
prom/prometheus                                 latest           a5624f16ebec   2 months ago    223MB
ubuntu                                          22.04            6b7dfa7e8fdb   2 months ago    77.8MB
ubuntu                                          20.04            d5447fc01ae6   2 months ago    72.8MB
grafana/grafana                                 latest           179ad45e2c74   2 months ago    315MB
docker/volumes-backup-extension                 1.1.1            afd08623b0ee   2 months ago    118MB
portainer/portainer-ce                          alpine           ecdb4fbad999   3 months ago    292MB
```

Definición de terminos

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