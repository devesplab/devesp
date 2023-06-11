---
layout: default
title: Archivos y Directorios
permalink: /archivos-y-directorios/
parent: Comandos y Procesos
grand_parent: Linux
has_children: true
has_toc: false
nav_order: 1
---

# Archivos y Directorios
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

## Propósito De Directorios y Ficheros

Cuando ha pasado tiempo que hemos trabajado en un projecto, acumulamos mucha informacíon de tópicos diferentes. A un cierto punto es imperativo agrupar información relacionada para que la podamos manejar mas facil.

Luego entonces tenemos que:
- Un fichero contiene informacíon de un tema en particular.
- Un directorio no ayuda a agrupar ficheros relacionados a un tema.

Los ficheros pueden ser de varios tipos: texto, binario, pdf, zip, jpeg, etc. Este tipo de clasificación no se aplica a directorios.

## Comandos Para Navegar Directorios y Ficheros

Los comands básicos de navegación en la linea de comandos son `ls` y `cd`.
Podemos usar una variedad de opciones o banderas para influenciar la salida de los comandos.

Usaremos los comados siguientes:

ls
: listar archivos y ficheros

cd
: crear directorios

touch
: utilidad para crear un fichero o actualizar fecha de creación

echo
: usado para representar text en la terminal

tree
: utilidad para mostrar el árbol completo de la localidad designada

{: .note }
Estos comandos son de uso universal y funcionan de la misma manera en cualquier sistema operativo.

La sintaxis general para listar directorios y ficheros es asi:
```
ls [opciones] /paso/a/diretorio/fichero
```
Las opciones so una variedad de banderas que alteran la salida del comando.

## Crear Directorios y Ficheros

Crear un directorio.
```
-> mkdir data
```
Crear un subdirectorio (directorio dentro de otro directorio).
```
-> mkdir data/pets
```

Cambiar a un directorio.
```
-> cd data
```

Crear un fichero con el comando `touch`.
```
-> touch datafile1.txt
```

Crear un fichero con el comando `echo`.
```
echo "fichero de Camoa" > data/pets/camoa.txt
```

Crear un directory escondido
```
-> mkdir .hidden_directory
```

Crear un fichero escondido usando el editor VI.
```
-> vi .hidden_file
```

## Listar Directorios y Ficheros

Listar el contenido de un directorio.
```
-> ls data/
datafile1.txt
datafile2.txt
pets/
```
La linea que contiene el símbolo `/` al final, indica que es un directorio. Los demas son ficheros.

Listar un solo fichero en un directorio.
```
-> ls data/datafile1.txt
data/datafile1.txt
```

Listar ficheros en formato largo usando la bandera `-l`.
```
->  ls -l data/
total 8
-rw-r--r-- 1 root root    0 Jun 11 02:06 datafile1.txt
-rw-r--r-- 1 root root   13 Jun 11 02:06 datafile2.txt
drwxr-xr-x 2 root root 4096 Jun 11 02:37 pets/
```

{: .highlight }
> un guión al inicio de la linea indica que es un fichero común y corriente
>
> la letra `d` al principio de la linea indica que es un directorio; notese el simbolo `/` al final del articulo

Listar ficheros multiples usando el asterisco `*`.
```
-> ls -l data/datafile*
-rw-r--r-- 1 root root  0 Jun 11 02:06 data/datafile1.txt
-rw-r--r-- 1 root root 13 Jun 11 02:06 data/datafile2.txt

-> ls -l *.txt
-rw-r--r-- 1 root root  0 Jun 11 02:06 data/datafile1.txt
-rw-r--r-- 1 root root 13 Jun 11 02:06 data/datafile2.txt

-> ls -l data*.txt
-rw-r--r-- 1 root root  0 Jun 11 02:06 datafile1.txt
-rw-r--r-- 1 root root 13 Jun 11 02:06 datafile2.txt
```

Listar ficheros usando expresiones regulares [^1].
```
-> ls -l data/datafile[0-9]*.txt
-rw-r--r-- 1 root root  0 Jun 11 02:06 data/datafile1.txt
-rw-r--r-- 1 root root 13 Jun 11 02:06 data/datafile2.txt
```
[^1]: Discutiremos expresiones regulares extensamente en otro articulo.

Usar el comando `tree` para listar el contenido entero de una localidad.
```
-> tree
.
|-- Downloads
|-- MyDocuments
|-- data
|   |-- datafile1.txt
|   |-- datafile2.txt
|   `-- pets
|       `-- camoa.txt
`-- temp
```

Listar contenido en forma recursiva con la opción `-R`.
```
-> ls -lR data
data:
total 8
-rw-r--r-- 1 root root    0 Jun 11 02:06 datafile1.txt
-rw-r--r-- 1 root root   13 Jun 11 02:06 datafile2.txt
drwxr-xr-x 2 root root 4096 Jun 11 03:53 pets/

data/pets:
total 4
-rw-r--r-- 1 root root 94 Jun 11 02:37 camoa.txt
```

Listar ficheros y directorios escondidos usando la bandera `-a` en conjunto con la bandera `-l.
```
-> ls -la /root
total 88
drwx------ 1 root root  4096 Jun 11 02:37 ./
drwxr-xr-x 1 root root  4096 Jun 10 23:55 ../
-rw-r--r-- 1 root root  2896 Jun 11 00:01 .aliasrc
-rw------- 1 root root   591 Jun 11 00:09 .bash_history
-rw-r--r-- 1 root root   149 Jun 11 00:11 .bash_profile
-rw-r--r-- 1 root root   466 Jun 11 00:09 .bashrc
-rw-r--r-- 1 root root 17781 Jun 11 00:01 .git-prompt.sh
-rw-r--r-- 1 root root   604 Jun 11 00:01 .gitconfig
drwxr-xr-x 2 root root  4096 Jun 11 02:31 .hidden_directory/
drwx------ 3 root root  4096 Jun 11 00:04 .launchpadlib/
-rw-r--r-- 1 root root    35 Jun 11 00:01 .profile
-rw------- 1 root root  3833 Jun 11 02:37 .viminfo
-rw-r--r-- 1 root root    87 Jun 11 00:01 .vimrc
drwxr-xr-x 2 root root  4096 Jun 11 00:12 Downloads/
drwxr-xr-x 2 root root  4096 Jun 11 00:12 MyDocuments/
drwxr-xr-x 3 root root  4096 Jun 11 02:35 data/
drwxr-xr-x 2 root root  4096 Jun 11 00:12 temp/
```

> para omitir `.` y `..` podemos usar la opción `-A` asi: `ls -lA /root`

Listar solo directorios usando la opcíon `-d`
```
-> ls -ld *
drwxr-xr-x 2 root root 4096 Jun 11 00:12 Downloads/
drwxr-xr-x 2 root root 4096 Jun 11 00:12 MyDocuments/
drwxr-xr-x 2 root root 4096 Jun 11 02:06 data/
drwxr-xr-x 2 root root 4096 Jun 11 00:12 temp/
```
Listar solo directorios usando el símbolo `^` para indicar el principio de la linea de cada artículo.
```
-> ls -l | grep ^d
drwxr-xr-x 2 root root 4096 Jun 11 00:12 Downloads/
drwxr-xr-x 2 root root 4096 Jun 11 00:12 MyDocuments/
drwxr-xr-x 2 root root 4096 Jun 11 02:06 data/
drwxr-xr-x 2 root root 4096 Jun 11 00:12 temp/
```

Listar ficheros con la bandera `-t` usada para mostra los articulos que han sido modificados mas recientememte. Por defecto, mostrara el mas reciente al principio de la lista.
```
-> ls -lt
total 16
drwxr-xr-x 3 root root 4096 Jun 11 02:35 data/
drwxr-xr-x 2 root root 4096 Jun 11 00:12 temp/
drwxr-xr-x 2 root root 4096 Jun 11 00:12 Downloads/
drwxr-xr-x 2 root root 4096 Jun 11 00:12 MyDocuments/
```
Lo mismo que arriba, pero usando la bandera `-r` para clasificar la salida en reverso con lo mas reciente listado por ultimo.
```
-> ls -ltr
total 16
drwxr-xr-x 2 root root 4096 Jun 11 00:12 MyDocuments/
drwxr-xr-x 2 root root 4096 Jun 11 00:12 Downloads/
drwxr-xr-x 2 root root 4096 Jun 11 00:12 temp/
drwxr-xr-x 3 root root 4096 Jun 11 02:35 data/
```

Listar el inode correspondiente a un fichero usando la bandera `-i`.
```
-> ls -li data/pets/camoa.txt
3044362 -rw-r--r-- 1 root root 94 Jun 11 02:37 data/pets/camoa.txt
```

Listar las stadisticas completas de un fichero.
```
-> stat data/pets/camoa.txt
  File: data/pets/camoa.txt
  Size: 94        	Blocks: 8          IO Block: 4096   regular file
Device: b2h/178d	Inode: 3044362     Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2023-06-11 02:37:53.148264837 +0000
Modify: 2023-06-11 02:37:53.148264837 +0000
Change: 2023-06-11 02:37:53.149171904 +0000
 Birth: 2023-06-11 02:37:53.148264837 +0000
```

Mostrar el tamaño de un fichero usando la bandera `-h`, que indica notación humana legible.
```
-> ls -lh data/pets/camoa.txt
-rw-r--r-- 1 root root 94 Jun 11 02:37 data/pets/camoa.txt
```
La bandera `-h` en conjunto con `-l` y `-s` muestran el tamaño del fichero como 1K 234M 2G etc. En el ejemplo anterior, el fichero es de 94K de tamaño.

## Copiar Directorios y Ficheros

En este ejemplo, tenenos el fichero `camoa.txt` y lo compiamos a un nuevo fichero `pantera.txt`.
```
root@ubuntu1 [ DevEsp ]
~
hist:99 -> cd data/pets/

root@ubuntu1 [ DevEsp ]
~/data/pets
hist:99 -> ls -l
total 4
-rw-r--r-- 1 root root 94 Jun 11 02:37 camoa.txt

root@ubuntu1 [ DevEsp ]
~/data/pets
hist:99 -> cp camoa.txt pantera.txt

root@ubuntu1 [ DevEsp ]
~/data/pets
hist:100 -> ls -l
total 8
-rw-r--r-- 1 root root 94 Jun 11 02:37 camoa.txt
-rw-r--r-- 1 root root 94 Jun 11 03:50 pantera.txt

```
## Renombrar Directorios y Ficheros

Aqui decidimos cambiar el nombre del archivo `pantera.txt` a `obidian.txt`.
```
root@ubuntu1 [ DevEsp ]
~/data/pets
hist:100 -> mv pantera.txt obidian.txt

root@ubuntu1 [ DevEsp ]
~/data/pets
hist:101 -> ls -l
total 8
-rw-r--r-- 1 root root 94 Jun 11 02:37 camoa.txt
-rw-r--r-- 1 root root 94 Jun 11 03:50 obidian.txt
```

## Borrar Directorios y Ficheros

Finalmente, decidimos borrar el archivo `obidian.txt`.
```
root@ubuntu1 [ DevEsp ]
~/data/pets
hist:101 -> rm -rf obidian.txt
```

## Recursos

Los siguientes enlaces estan disponibles en linea.

* Página Manual de [ls](https://manpages.ubuntu.com/manpages/focal/es/man1/ls.1.html)

* Página Manual de [mkdir](https://manpages.ubuntu.com/manpages/focal/es/man1/mkdir.1.html)

* Página Manual de [cd](https://www.man7.org/linux/man-pages/man1/cd.1p.html)

* Página Manual de [touch](https://www.man7.org/linux/man-pages/man1/touch.1.html)

* Página Manual de [echo](https://www.man7.org/linux/man-pages/man1/echo.1.html)
 