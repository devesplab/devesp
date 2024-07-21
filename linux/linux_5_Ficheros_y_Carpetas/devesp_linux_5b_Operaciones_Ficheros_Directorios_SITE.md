---
layout: default
title: Operaciones Con Ficheros y Directorios
permalink: /operaciones_ficheros_directorios/
parent: Ficheros y Carpetas
grand_parent: Linux
has_children: true
has_toc: false
nav_order: 1
---

# Operaciones Con Ficheros y Directorios

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


## Ubicación En El Sistema De Archivos

En cualqiuer momenta dado cuando entramos a un sistema de Linux, nos encontraremos en un directorio el cual puede o no contener archivos.

Es importante mantener la referencia a el directorio de inicio principal, lo podemos llamar directorio hogar que se denota con `~` or `$HOME`. En cualquier comando que entremos, podemos usar `~` como el punto de referencia a nuestro directorio de inicio.

No importa donde estemos en el estructura de directorios siempre podemos volver a la base usando el comando `cd` de esta manera:
```sh
-> cd

-> cd ~

-> cd $HOME
```

## Comandos Para Navegar Directorios y Ficheros

Los comands básicos de navegación en la linea de comandos son `ls` y `cd`.
Podemos usar una variedad de opciones o banderas para influenciar la salida de los comandos.

Con mucha frecuencia usaremos los comados siguientes:

ls
: listar archivos y ficheros

mkdir
: crear directorios

cd
: cambiar a un directorio

cp
: copiar ficheros o directorios

mv
: renombrar ficheros o directorios

rm
: borrar ficheros o directorios

touch
: utilidad para crear un fichero o actualizar fecha de creación

echo
: usado para representar texto en la terminal

tree
: utilidad para mostrar el árbol completo de la localidad designada

{: .note }
Estos comandos son de uso universal y funcionan de la misma manera en cualquier sistema operativo variante de RHEL o Debian.

Cada comando tiene opciones que alteran la salida del comando.

## Crear Directorios y Ficheros

An algun momento habrá la necesidad de agrupar información acerca de un tema. Esto se logra creado directorios en los que podemos guardar los archivos de interes.

La sintaxis general para crear directorios es asi:
```bash
mkdir [opciones] /paso/a/diretorio
```

Crear un directorio.
```bash
-> mkdir data
```
Crear un subdirectorio (directorio dentro de otro directorio).
```bash
-> mkdir data/pets
```
Crear una cadena de directorio y subdirectorios en un solo paso usando la opción `-p`.
```bash
-> mkdir -p dir/subdir2/subdir2
```

Cambiar a un directorio.
```bash
-> cd data
-> cd dir/subdir2/
```

Crear un fichero con el comando `touch`.
```bash
-> touch datafile1.txt
```

Crear un fichero con el comando `echo`. En este ejemplo, el fichero esta en un subdirectorio.
```bash
echo "fichero de Camoa" > data/pets/camoa.txt
```

Crear un directory escondido
```bash
-> mkdir .hidden_directory
```

Crear un fichero escondido usando el editor VI.
```bash
-> vi .hidden_file
```

## Listar Directorios y Ficheros

En sistemas donde no hay interfaz gráfico solo podemos usar el [indicador](../linux_7_Shell/devesp_shell_7d_cli_intro_SITE.md). La interacción en el indicador, o linea de comandos, es en texto puro. Asi que estamos limitados a usar comando para navegar archivos y carpetas.

La sintaxis general para listar directorios y ficheros es asi:
- el comando `ls`
- opciones del comando
- localidad a listar
```bash
ls [opciones] /paso/a/diretorio
ls [opciones] /paso/a/diretorio/fichero
```

Veremos a continuación el uso general del comando `ls` para listar contenido.

Listar el contenido de un directorio.<br>
La linea que contiene el símbolo `/` al final, indica que es un directorio. Los demas son ficheros.
```bash
-> ls data/
datafile1.txt
datafile2.txt
pets/
```

Listar un solo fichero en un directorio.
```bash
-> ls data/datafile1.txt
data/datafile1.txt
```

Listar ficheros en formato largo usando la opción `-l`.
```bash
->  ls -l data/
total 8
-rw-r--r-- 1 root root    0 Jun 11 02:06 datafile1.txt
-rw-r--r-- 1 root root   13 Jun 11 02:06 datafile2.txt
drwxr-xr-x 2 root root 4096 Jun 11 02:37 pets/
```

{: .highlight }
> un guión `-` al inicio de la linea indica que es un fichero regular
>
> la letra `d` al principio de la linea, y el simbolo `/` al final de la linea. indica que es un directorio.

Listar ficheros multiples usando el asterisco `*`.
```bash
-> ls -l data/datafile*
-rw-r--r-- 1 root root  0 Jun 11 02:06 data/datafile1.txt
-rw-r--r-- 1 root root 13 Jun 11 02:06 data/datafile2.txt

-> ls -l data/*.txt
-rw-r--r-- 1 root root  0 Jun 11 02:06 data/datafile1.txt
-rw-r--r-- 1 root root 13 Jun 11 02:06 data/datafile2.txt

ls -l data/data*.w
-rw-r--r-- 1 root root  0 Jun 11 02:06 data/datafile1.txt
-rw-r--r-- 1 root root 13 Jun 11 02:06 data/datafile2.txt
```

Listar ficheros usando expresiones regulares [^1].
```bash
-> ls -l data/datafile[0-9]*.txt
-rw-r--r-- 1 root root  0 Jun 11 02:06 data/datafile1.txt
-rw-r--r-- 1 root root 13 Jun 11 02:06 data/datafile2.txt
```
[^1]: Discutiremos expresiones regulares extensamente en otro articulo.

Usar el comando `tree` para listar el contenido entero de una localidad.
```bash
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

Listar contenido en forma recursiva con la opción `-R`. Esto es útil cuando queremos encontrar un fichero pero no sabemos exactamente donde se encuentra.
```bash
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

Listar solo directorios usando la opcíon `-d`
```bash
-> ls -ld *
drwxr-xr-x 2 root root 4096 Jun 11 00:12 Downloads/
drwxr-xr-x 2 root root 4096 Jun 11 00:12 MyDocuments/
drwxr-xr-x 2 root root 4096 Jun 11 02:06 data/
drwxr-xr-x 2 root root 4096 Jun 11 00:12 temp/
```
Listar solo directorios usando `^d`.
```bash
-> ls -l | grep ^d
drwxr-xr-x 2 root root 4096 Jun 11 00:12 Downloads/
drwxr-xr-x 2 root root 4096 Jun 11 00:12 MyDocuments/
drwxr-xr-x 2 root root 4096 Jun 11 02:06 data/
drwxr-xr-x 2 root root 4096 Jun 11 00:12 temp/
```

Listar solo ficheros usando `^-`.
```bash
-> ls -l | grep ^-
-rw-r--r-- 1 root root    0 Jun 11 02:06 datafile1.txt
-rw-r--r-- 1 root root   13 Jun 11 02:06 datafile2.txt
```

Listar ficheros con la opción `-t` usada para mostrar los articulos que han sido modificados mas recientememte. Por defecto, mostrara la entrada mas reciente al principio de la lista.
```bash
-> ls -lt
total 16
drwxr-xr-x 3 root root 4096 Jun 11 02:35 data/
drwxr-xr-x 2 root root 4096 Jun 11 00:12 temp/
drwxr-xr-x 2 root root 4096 Jun 11 00:12 Downloads/
drwxr-xr-x 2 root root 4096 Jun 11 00:12 MyDocuments/
```
Lo mismo que arriba, pero usando la opción `-r` para clasificar la salida en reverso con lo mas reciente listado por ultimo.
```bash
-> ls -ltr
total 16
drwxr-xr-x 2 root root 4096 Jun 11 00:12 MyDocuments/
drwxr-xr-x 2 root root 4096 Jun 11 00:12 Downloads/
drwxr-xr-x 2 root root 4096 Jun 11 00:12 temp/
drwxr-xr-x 3 root root 4096 Jun 11 02:35 data/
```

Listar el inode correspondiente a un fichero usando la opción `-i`.
```bash
-> ls -li data/pets/camoa.txt
3044362 -rw-r--r-- 1 root root 94 Jun 11 02:37 data/pets/camoa.txt
```

Listar las stadisticas completas de un fichero.
```bash
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

Mostrar el tamaño de un fichero usando la opción `-h`, que indica notación humana legible.
```bash
-> ls -lh data/pets/camoa.txt
-rw-r--r-- 1 root root 94 Jun 11 02:37 data/pets/camoa.txt
```
La opción `-h` en conjunto con `-l` y `-s` muestran el tamaño del fichero como 1K 234M 2G etc. En el ejemplo anterior, el fichero es de 94K de tamaño.

## Copiar Directorios y Ficheros

La sintaxis general para copiar directorios y ficheros es asi:
```bash
cp [opciones] /paso/a/diretorio /paso/a/diretorio-nuevo
cp [opciones] /paso/a/diretorio/fichero  /paso/a/diretorio/fichero-nuevo
```

En este ejemplo, tenenos el fichero `camoa.txt` y lo compiamos a un nuevo fichero `pantera.txt`.
```bash
-> ls -l
-rw-r--r-- 1 root root 94 Jun 11 02:37 camoa.txt

-> cp camoa.txt pantera.txt

-> ls -l
-rw-r--r-- 1 root root 94 Jun 11 02:37 camoa.txt
-rw-r--r-- 1 root root 94 Jun 11 03:50 pantera.txt
```

Copiar directorios requiere la opción `-r` porque casi siempre los directorios no estan vacios. 
Aqui copiamos el directorio `pets` a `animals`.
```bash
-> ls -l
drwxr-xr-x 2 root root 4096 Jun 11 03:53 pets/

-> cp -r pets animals

-> ls -l
drwxr-xr-x 2 root root 4096 Jun 12 04:04 animals/
drwxr-xr-x 2 root root 4096 Jun 11 03:53 pets/
```

## Renombrar Directorios y Ficheros

La sintaxis general para renombrar directorios y ficheros es asi:
```bash
mv [opciones] /paso/a/diretorio /paso/a/diretorio-nuevo
mv [opciones] /paso/a/diretorio/fichero  /paso/a/diretorio/fichero-nuevo
```

Aqui decidimos cambiar el nombre del archivo `pantera.txt` a `obsidian.txt`.
```bash
-> ls -l
-rw-r--r-- 1 root root 94 Jun 11 02:37 camoa.txt
-rw-r--r-- 1 root root 94 Jun 11 03:50 pantera.txt

-> mv pantera.txt obsidian.txt

-> ls -l
-rw-r--r-- 1 root root 94 Jun 11 02:37 camoa.txt
-rw-r--r-- 1 root root 94 Jun 11 03:50 obsidian.txt
```

De manera similar, podemos renombrar un directorio o subdirectorio.
```bash
-> mv dir1 dir2

-> mv ~/dir1/subdir1  ~/dir1/subdir2
```

## Borrar Directorios y Ficheros

La sintaxis general para borrar directorios y ficheros es asi:
```bash
rm [opciones] /paso/a/diretorio /paso/a/diretorio-nuevo
rm [opciones] /paso/a/diretorio/fichero  /paso/a/diretorio/fichero-nuevo
```

Finalmente, decidimos borrar el archivo `obsidian.txt`.
```bash
-> rm obsidian.txt
```

Borrar directorios requiere la opción `r` si tiene ficheros.

{: .warning }
Debe observarse mucho cuidado al usar `rm -rf` puesto que es un comando muy destructivo causando daños irreversibles.

> el símbolo `~` indica que esta directamente bajo el directorio de inicio

```bash
-> rm -r ~/directorio
```

Podemos usar la opción `-f` para evitar la confirmación por cada articulo a borrar.
```bash
-> rm -rf [fichero|directorio]
```

Se puede borrar mas de un fichero o directorio a la vez.
```bash
-> rm -rf fichero1 fichero2 dir/fichero3

-> rm -rf directorio1 dir/subdir
```

## Ficheros y Directorios Escondidos

El sistema operativo puede mantener ciertos ficheros fuera de la vista con el propósito	de proteger información importante y prevenir que sea alterada o borrada accidentalmente. Un fichero escondido es generalmente estático y no de uso general, es decir, modificaciones a ficheros escondidos son muy infrequentes.

Se dice que un fichero o directorio es escondido cuando el primer símbolo en el nombre es un punto `.` seguido por letras y números, por ejemplo `.bashrc`

Enseguida listemos ficheros y directorios escondidos usando las opciónes siguientes:
- `-a` para mostrar ficheros escondidos
- `-l` para producir una lista extendida
```bash
-> ls -la /root
total 88
drwx------ 1 root root  4096 Jun 11 02:37 ./
drwxr-xr-x 1 root root  4096 Jun 10 23:55 ../
-rw-r--r-- 1 root root  2896 Jun 11 00:01 .aliasrc
-rw------- 1 root root   591 Jun 11 00:09 .bash_history
-rw-r--r-- 1 root root   149 Jun 11 00:11 .bash_profile
-rw-r--r-- 1 root root   466 Jun 11 00:09 .bashrc
drwxr-xr-x 2 root root  4096 Jun 11 00:12 Downloads/
drwxr-xr-x 2 root root  4096 Jun 11 00:12 MyDocuments/
drwxr-xr-x 3 root root  4096 Jun 11 02:35 data/
drwxr-xr-x 2 root root  4096 Jun 11 00:12 temp/
```

Por defecto, el comando `ls` muestra `.` que indica el directorio corriente y `..` que indica un directorio arriba. Para omitir `.` y `..` podemos usar la opción `-A` asi:

```bash
-> ls -lA /root
total 88
-rw-r--r-- 1 root root  2896 Jun 11 00:01 .aliasrc
-rw------- 1 root root   591 Jun 11 00:09 .bash_history
-rw-r--r-- 1 root root   149 Jun 11 00:11 .bash_profile
-rw-r--r-- 1 root root   466 Jun 11 00:09 .bashrc
drwxr-xr-x 2 root root  4096 Jun 11 00:12 Downloads/
drwxr-xr-x 2 root root  4096 Jun 11 00:12 MyDocuments/
drwxr-xr-x 3 root root  4096 Jun 11 02:35 data/
drwxr-xr-x 2 root root  4096 Jun 11 00:12 temp/
```

Todas las operaciones tales como copiar, renombrar, mover, etc, se aplica igualmente a ficheros y directorios escondidos.

## Paso Absoluto vs Paso Relativo

El **paso absoluto** de un archivo o directorio se refiere a el paso que se sigue desde la raíz del sistema hasta su lugar final. 

El **paso relativo** se refiere a la localización en relación al punto donde nos encontramos en el sistema de archivos.

Veamos un ejemplo.

El usuario `devuser` esta en su directorio de inicio `/home/devuser` con la structura mostrada. Usamos el comando `pwd` para verificar donde nos encontramos, y el comando `tree` para visualizar la estructura del directorio. El símbolo `~` indica que nos encontramos en el directorio de inicio del usuario.
```bash
devuser@ubuntu1 [ DevEsp ]
~
hist:13 -> pwd
/home/devuser

devuser@ubuntu1 [ DevEsp ]
~
hist:14 -> tree
.
|-- Downloads
|-- MyDocuments
|-- data
|   |-- animals
|   |   `-- camoa.txt
|   |-- datafile1.txt
|   |-- datafile2.txt
|   `-- pets
|       `-- camoa.txt
|-- mydir
|   |-- dir1
|   |   |-- subdir1
|   |   |-- subdir2
|   |   `-- subdir3
|   `-- dir2
|       |-- subdir1
|       |-- subdir2
|       `-- subdir3
`-- temp
```

Esto muestra como listar el fichero `camoa.txt` usando el paso absoluto.
```bash
devuser@ubuntu1 [ DevEsp ]
~
hist:15 ->  ls -l /home/devuser/data/pets/camoa.txt
-rw-r--r-- 1 devuser devuser 94 Jun 14 04:50 /home/devuser/data/pets/camoa.tx
```

Luego el usuario se cambia al directorio `mydir/dir1` y desde allí lista el fichero `camoa.txt` usando el paso relativo, es decir, el paso con respecto as su posición en la estructura del directorio.  
```bash
devuser@ubuntu1 [ DevEsp ]
~
hist:15 -> cd mydir/dir1

devuser@ubuntu1 [ DevEsp ]
~/mydir/dir1
hist:16 -> ls -l ../../data/pets/camoa.txt
-rw-r--r-- 1 devuser devuser 94 Jun 14 04:50 ../../data/pets/camoa.txt
```

{: .highlight }
Usamos dos puntos `..` para indicar que vamos a escalar un posición arriba; hacemos esto cuantas veces sea necesario para localizar el recurso fuera del directorio donde nos encontramos.

## Referencias 

Paginas Manuales

- [ls](https://manpages.ubuntu.com/manpages/focal/es/man1/ls.1.html)
- [mkdir](https://manpages.ubuntu.com/manpages/focal/es/man1/mkdir.1.html)
- [cd](https://www.man7.org/linux/man-pages/man1/cd.1p.html)
- [touch](https://www.man7.org/linux/man-pages/man1/touch.1.html)
- [echo](https://www.man7.org/linux/man-pages/man1/echo.1.html)
 
 [Return to main page]({{site.baseurl}}/).
 