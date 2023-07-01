---
layout: default
title: Ayuda y Paginas Manuales
permalink: /linux_help/
parent: Linux
has_toc: false
nav_order: 4
---

# Buscar Ayuda en Linux

{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

Frecuentemente no recordamos las opciones disponibles para un comando. O quiza queremos aprender el uso de un comando o utilidad. 

Linux prove las paginas manuals y otros comandos para encontrar ayuda disponible internamente o externamente.

Las páginas manuales de Ubuntu están disponibles en linea en Español [^1]

Las páginas manuales de Linux están disponibles en linea en Inglés [^2]

[^1]: [Páginas Manuales de Ubuntu en Español](https://manpages.ubuntu.com/manpages/focal/es/)
[^2]: [Páginas Manuales de Linux en Inglés](https://man7.org/linux/man-pages/index.html)

## Páginas Manuales (Man Pages)

Las páginas manuales son documentos disponibles internamente en el sistema que nos proveen información acerca de las diferentes funciones y comandos disponibles.

La sintaxis siguiente muestra como usar el comando `man` para mostrar la pagina manual de un comando:
```
man <nombre-de-comando>
```

Por ejemplo la instrucción `man ls` nos muestra la página manual del comando `ls` que usamos para lstar archivos y directorios. Abajo vemos un extracto recortado de la página manual de `ls`.
```
NOMBRE

       ls, dir, vdir - listan los contenidos de directorios

SINOPSIS

       ls [opciones] [fichero...]
       dir [fichero...]
       vdir [fichero...]

       Opciones de POSIX: [-CFRacdilqrtu1]

       Opciones  de  GNU  (en  la  forma más corta): [-1abcdfghiklmnopqrstuvwxABCDFGHLNQRSUX] [-w
       cols] [-T cols] [-I  patrón]  [--full-time]  [--show-control-chars]  [--block-size=tamaño]
       [--format={long,verbose,commas,across,vertical,single-column}]

DESCRIPCIÓN

       El  programa  ls  lista  primero  sus argumentos no directorios fichero, y luego para cada
       argumento directorio todos los ficheros  susceptibles  de  listarse  contenidos  en  dicho
       directorio. 

OPCIONES DE POSIX

       -C     Lista los ficheros en columnas, ordenados verticalmente.

       -F     Añade tras cada nombre de directorio un `/', tras cada nombre de  FIFO  un  `|',  y
              tras cada nombre de un ejecutable un `*'.

(...snip...)             
```

Es de notar que las páginas manuales no están instaladas por defecto en **máquinas virtuales**. El usuario puede instalar las páginas manuales para la localización que corresponda al idioma del usuario. Al entrar el comando `man ls` en una máquina virtual de Ubuntu, vemos este mensaje indicando que ha sido optimizada para remover espacio que no es crítico para operaciones normales.

```bash
 -> man ls
This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, including manpages, you can run the 'unminimize'
command. You will still need to ensure the 'man-db' package is installed.
```
Es bastante común remover componentes innecesarios de servidores que no requieren cosas tales como páginas manuales.

## Comando TLDR

En variantes de **RHEL** y **Ubuntu**, podemos instalar el comando `tldr`[^3].

[^3]: [TLDR fuente abierta](https://tldr.sh/)

El comando tldr es una utilidad que provee una lista corta de los usos mas comunes para ese comando.

En RHEL se instala de esta manera.

```
-> dnf install tldr

-> yum install tldr
```

En Ubuntu se instala de esta manera.
```
-> apt install tldr
```

En este ejemplo entremos el comando `tldr ls` (se usa igual en RHEL y Ubuntu).
```bash
-> tldr ls

  ls

  List directory contents.
  More information: https://www.gnu.org/software/coreutils/ls.

  - List files one per line:
    ls -1

  - List all files, including hidden files:
    ls -a

  - List all files, with trailing `/` added to directory names:
    ls -F

  - Long format list (permissions, ownership, size, and modification date) of all files:
    ls -la

  - Long format list with size displayed using human-readable units (KiB, MiB, GiB):
    ls -lh

  - Long format list sorted by size (descending):
    ls -lS

  - Long format list of all files, sorted by modification date (oldest first):
    ls -ltr

  - Only list directories:
    ls -d */
```

TLDR crea un caché en el directorio de inicio en `$HOME/.cache/tldr`.<br>
A medida que hacemos busquedas, agrega ficheros .md en el directorio `common` o `linux` dependiendo de la ayuda que buscamos.
```bash
-> ls -lR ~/.cache/tldr/
/root/.cache/tldr/:
total 4
drwxr-xr-x 4 root root 4096 Jul  1 10:30 pages/

/root/.cache/tldr/pages:
total 8
drwxr-xr-x 2 root root 4096 Jul  1 10:12 common/
drwxr-xr-x 2 root root 4096 Jul  1 10:30 linux/

/root/.cache/tldr/pages/common:
total 4
-rw-r--r-- 1 root root 1060 Jul  1 10:12 ls.md

/root/.cache/tldr/pages/linux:
total 4
-rw-r--r-- 1 root root 533 Jul  1 10:30 vgs.md
```

TLDR [^4] esta disponible en linea como fuente abierta.

[^4]: [TLDR en Github](https://github.com/tldr-pages/tldr-python-client)

## Comando INFO

Red Hat describe el paquete `info` de esta manera:<br>
_"EL proyecto GNU usa el fichera de formato textinfo para su documentación. EL paquete info provee una manera propia en forma de visualizor en la terminal para ver ficheros de textinfo"_

En RHEL9, si por alguna razón el paquete no esta presente se puede instalar asi:
```
dnf install info
```

Luego podemos ver informacion de un comando asi:
```
info vgs
```

[Return to main page]({{site.baseurl}}/).