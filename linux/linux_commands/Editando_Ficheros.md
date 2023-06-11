---
layout: default
title: Editando Ficheros
permalink: /editando-ficheros/
parent: Comandos y Procesos
grand_parent: Linux
has_children: true
has_toc: false
nav_order: 2
---

# Editando Ficheros
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

## Permiso de Editar

El comando `ls` muestra los permisos, asi come el grupo y usuario dueño de los ficheros.
```
-> ls -lA
total 8
-rw-r--r-- 1 root root    0 Jun 11 02:06 datafile1.txt
-rw-r--r-- 1 root root   13 Jun 11 02:06 datafile2.txt
drwxr-xr-x 2 root root 4096 Jun 11 03:53 pets/
```

Podemos usar la opción `-n` para ver el número del usuario y de grupo para verificar que tienes acceso a los ficheros. 

En este ejemplo, el grupo es zero, asi come el usuario. Esto indica que cada articulo pertenece a `root`.
```
-> ls -lAn
total 8
-rw-r--r-- 1 0 0    0 Jun 11 02:06 datafile1.txt
-rw-r--r-- 1 0 0   13 Jun 11 02:06 datafile2.txt
drwxr-xr-x 2 0 0 4096 Jun 11 03:53 pets/
```