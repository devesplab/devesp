---
layout: default
title: Paquetes en Ubuntu
permalink: /ubuntu-manejar-paquetes/
parent: Manejando Paquetes
grand_parent: Linux
has_children: false
has_toc: false
nav_order: 98
---

# Manejo de Paquetes en Ubuntu
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

## Advanced Packaging Tool – APT

The utilidad Advanced Packaging Tool (APT) se usa para instalar, mejorar o actualizar paquetes en Ubuntu.

A seguido veamos los ejemplos más usados.

Instalar un Paquete
```
sudo apt install nmap
```
Remover un Paquete
```
sudo apt remove nmap
```

Actualizar el index 
```
sudo apt update
```
Actualizar el sistem entero
```
sudo apt upgrade
```
Buscar un paquete
```
sudo apt search coreutils
```

## Otras Opciones Usadas Con APT

Mostrar las opciones para apt con el parámetro `--help`
```
sudo apt --help
```

Estas opciones son útiles.

list 
: listar paquetes basado en el nombre

search 
: buscar en la descripcion de los paquetes

show
: mostras los detalles de un paquete

reinstall
: reinstaller paquestes

autoremove 
: automáticament borrar paquetes que no estan en uso

full-upgrade 
: actualizar el sistem borrando/instalando/actualizando paquetes

Cada actividad que resulta de usar apt va en el archivo de registro `/var/log/dpkg.log`.

Ubuntu mantiene un sitio [^1] en donde podemos buscar información de paquetes.

## Mostrar Información De Paquetes

Buscar un paquete.
```bash
-> sudo apt search coreutils
Sorting... Done
Full Text Search... Done
coreutils/jammy,now 8.32-4.1ubuntu1 amd64 [installed]
  GNU core utilities

policycoreutils/jammy 3.3-1build1 amd64
  SELinux core policy utilities  
```
En el primer caso, podemos ver que el paquete está instalado como lo indica la palabra "installed".
En el segundo caso, solo muestra que el paquete esta disponible para ser instalado.

Mostrar información acerca del paquete.
```bash
-> sudo apt show coreutils
Package: coreutils
Version: 8.32-4.1ubuntu1
Priority: required
Essential: yes
Section: utils
Origin: Ubuntu
Maintainer: Ubuntu Developers <ubuntu-devel-discuss@lists.ubuntu.com>
Original-Maintainer: Michael Stone <mstone@debian.org>
Bugs: https://bugs.launchpad.net/ubuntu/+filebug
Installed-Size: 7283 kB
Pre-Depends: libacl1 (>= 2.2.23), libattr1 (>= 1:2.4.44), libc6 (>= 2.34), libgmp10 (>= 2:6.2.1+dfsg), libselinux1 (>= 3.1~)
Homepage: http://gnu.org/software/coreutils
Task: minimal, server-minimal
Download-Size: 1438 kB
APT-Manual-Installed: yes
APT-Sources: http://archive.ubuntu.com/ubuntu jammy/main amd64 Packages
Description: GNU core utilities
```

## Mostrar los Archivos De Un Paquete

A veces queremos saber sin un paquete ofrece un archivo o utilidad en particular.
Esto lo podemos saber al mostrar la lista entera de archivos que el paquete a instalado en el sistema.

Este ejemplo muestra una lista parcial de archivos que son parte de `coreutils`.

```
-> dpkg-query -L coreutils
/.
/bin
/bin/cat
/bin/chgrp
/bin/chmod
/bin/chown
/bin/cp
/bin/date
/bin/dd
...
```

A seguido veamos los archivos que están en un paquete que no hemos instalado todavía.

Primero installemos `apt-file` y actulizemos el sistema.
```
-> sudo apt-get install apt-file
-> sudo apt-file update
```
Luego listemos los archivos de algún paquete que nos interese.
```
-> apt-file list <nomber-del-paquete>
```

## Buscar Paquetes En Linea

Ubuntu ofrece la facilidad de buscar información acerca de paquetes or archivos específicos.

Primero verifiquemos la arquitectura de nuestro sistema
```
-> uname -a
Linux ubuntu2204-1 5.15.49-linuxkit #1 SMP Tue Sep 13 07:51:46 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
```
Esto muestra que solo podemos usar paquetes de 64Bits.

Sigamos los pasos siguientes:

* Ir a http://packages.ubuntu.com/
* Ir a la sección **Search package directories** -- (Buscar paquetes de directorio)
* Entrar el nombre del paquete en la caja **Keyword** y seleccionar **Only Show exact matches** (Mostrar solo coincidencias exactas)
* Seleccionar la **Distribution** y cliquear el botón Search
* Seleccionar el paquete deseado cuando el resultado aparezca
* Hacia el final de la página, cliquera el enlace `list of files` que corresponde a la arquitectura de nuestro sistema
* La lista de archivos se mostrará en la página siguiente

Este commando mostraría lo mismo
```
curl -s https://packages.ubuntu.com/$(lsb_release -cs)/$(dpkg --print-architecture)/<NOMBRE-DE-PAQUETE>/filelist
```
Por ejemplo, esto es para `policycoreutils`.
```
curl -s https://packages.ubuntu.com/$(lsb_release -cs)/$(dpkg --print-architecture)/policycoreutils/filelist
```

## Referencias

[^1]: https://packages.ubuntu.com/