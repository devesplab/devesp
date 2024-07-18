---
layout: default
title: Comandos Basicos
permalink: /comandos_basicos/
parent: El Shell
grand_parent: Linux
has_children: true
has_toc: false
nav_order: 5
---

# LINUX :: comandos basicos del usuario 

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

**DESCRIPCION**

En esta leccion:
- Comandos para manejar archivos y carpetas
- Comandos para editar y ver contenidos de archivos
- Comandos para ver la fecha

**DEPENDENCIAS**

En Ubuntu 22.04 el paquete `bsdmainutils` debe estar instalado para poder esar el comando `date` para mostrar fechas y calendarios.

**REQUERIMIENTOS**

Esta leccion requiere acceso a la Linea de Comandos en una terminal de Linux.

En esta leccion usamos Ubuntu 22.04 y el BASH Shell.

Se requiere acceso administrativo para poder instaler paquetes cuando sea necessario.

**ADVERTENCIA**

ninguna.

## Ayuda para comandos

Linux ofrece ayuda en la forma de utilidades y manuales que proveen informacion acerca de comandos.

Podemos usar el comando `which` para saber si un comando esta disponible
```bash
which <comando>
```
La bandera `--help` facilita informacion compacta del uso del comando
```bash
<comando> --help
```
Podemos acceder las paginas manuales si estan disponibles.
```bash
man <comando>
```
Si esta disponible, tambien podemos usar el comando info
```bash
info <comando>
```

## Comandos para listar y encontrar archivos y carpetas

### comando: ls

El comando `ls` se usa para listar recursos.

```bash
-> ls
capitals1@
capitals2
country_capital_v1.lst
country_capital_v2.csv
data_profile.odt
dirOne/
identicalFile1.txt
identicalFile2.txt
identicalFile3.txt
itemized_items.lst
literature/
```

Podemos pasar combinaciones de banderas para manipular la salida de 
cualquier comando.

La bandera `-l` con el comando `ls` muestra la lista alargada de los archivos y carpetas. <br>
Agregando la bandera `-a` muestra archivos escondidos. 
```bash
-> ls -la
total 436
drwxrwxr-x 3 devuser devuser   4096 Feb 11 22:05 ./
drwxrwxr-x 4 devuser devuser   4096 Feb 11 21:51 ../
-rw-rw-r-- 1 devuser devuser    582 Feb 11 22:05 .aHiddenFile
-rw-rw-r-- 1 devuser devuser    291 Feb 11 21:25 afile005.yaml
-rw-rw-r-- 1 devuser devuser    210 Feb 11 21:25 afile01a.yaml
-rw-rw-r-- 1 devuser devuser  29294 Feb 11 21:25 afile02a.txt
-rw-rw-r-- 1 devuser devuser    325 Feb 11 21:25 afile02a.yaml
-rw-rw-r-- 1 devuser devuser    325 Feb 11 21:25 afile02b.json
-rw-rw-r-- 1 devuser devuser 376919 Feb 11 21:25 afile03a.txt
drwxrwxr-x 2 devuser devuser   4096 Feb 11 21:25 subdir2/
```
Notese el archivo `.afileHidden` en la salida del comando anterior.<br>
El punto `.` en la salida anterior indica el directorio corriente.<br>
EL doble punto `..` indica el directorio principal (arriba) de donde nos encontramos.

Usemos la bandera `-lah` para mostar el tamaño de archivos.<br>
La quinta columna muestra el tamaño del archivo. Por ejemploe el archivo `afile02a.txt` es `29K` (kilobytes) de tamaño.
```bash
-> ls -lah afile*
-rw-rw-r-- 1 devuser devuser  291 Feb 11 21:25 afile005.yaml
-rw-rw-r-- 1 devuser devuser  210 Feb 11 21:25 afile01a.yaml
-rw-rw-r-- 1 devuser devuser  29K Feb 11 21:25 afile02a.txt
-rw-rw-r-- 1 devuser devuser  325 Feb 11 21:25 afile02a.yaml
-rw-rw-r-- 1 devuser devuser  325 Feb 11 21:25 afile02b.json
-rw-rw-r-- 1 devuser devuser 369K Feb 11 21:25 afile03a.txtl
```

Usemos las bandera `-i` para mostar el inodo.<br>
EL inodo es el numero en la primera columna.
```bash
-> ls -li afile*
2265749 -rwxr-xr-x 1 devuser devuser  30 Jan  3 01:36 afile01.txt*
2265817 -rwxr-xr-x 1 devuser devuser 901 Feb  2 05:10 afile03*
2265764 -rwxr-xr-x 1 devuser devuser 901 Jan  3 01:36 afile03.txt*
```

Usemos las bandera `-tr` para mostar la lista de archivos con fecha de modification clasificada en forma descendente.<br>
```bash
-> ls -ltr afile*
-rwxr-xr-x 1 devuser devuser  30 Jan  3 01:36 afile01.txt*
-rwxr-xr-x 1 devuser devuser 901 Jan  3 01:36 afile03.txt*
-rwxr-xr-x 1 devuser devuser 901 Feb  2 05:10 afile03*
```

### comando: find

El comando `find` es muy bueno para encontrar archivos o carpetas que son dificiles de encontrar. Este comando es usado muy frecuentemente por administradores en programas de mantenimiento.
```bash
-> find . -name myDirectory

-> find . -name myFile

-> find . -name "afile03*"
./afile03
./afile03.txt
```

{: .note }
Vamos a explorar el comando `find` extensivamente como parte de otro ejercicio.

### comando: locate

El comando `locate` provee una forma rapida para mostrar si un archivo esta presente en el sistema.
```bash
-> locate myFile
```

## Manejando Carpetas

Linux tiene varios comandos para manipular recursos tales como carpetas (o directorios) y archivos.

Normalmente, un usuario hace todo su trabajo en su directorio de inicio.<br>
Usuarios mas sofisticados tales como administradores de sistemas y desarrolladores se mueven alrededor casi siempre sabiendo donde quieren aterrizar.

El usuario puede crear, borrar, y cambiar carpetas y archivos en su directorio hogar u otra localidad donde tenga el permiso apropiado.

{: .note }
Los directorios usados en este ejemplo pueden ser facilmente creados en su terminal.

### crear, copiar renombrar, mover, borrar archivos y carpetas

El comando `mkdir` se usa para crear archivos.<br>
Por ejemplo, procedamos a crear algunas carpetas de uso tradicional.
```bash
comando>> mkdir Documents
comando>> mkdir Downloads
comando>> mkdir Pictures
```

#### comando: cd y pwd

El comando `cd` facilita moverse (o cambiar) de una carpeta a otra.<br>
Solo tenemos que saber el nombre de la carpeta y pasarla como argumento al comando `cd`.<br>

Cambiemos a una de las carpetas que creamos.<br>
Usemos el comando `pwd` para mostrar que el cambio tuvo efecto.
```bash
comando>> cd Documents/
comando>> pwd
/home/user1/Documents
```

Usemos el comando `cd` para regresar al directorio base `$HOME`.

{: .note }
El comando `cd` siempre nos regresa a `$HOME` sin importar donde estamos en el sistema de archivos

1. el comando `pwd` muestra que estamos en el directorio `Documents`
2. el `cd` sin ningun argumento nos regresa a `$HOME`
3. el comando `pwd` confirma que hemos regresado a `$HOME`
```bash
comando>> pwd
/home/user1/Documents
comando>> cd
comando>> pwd
/home/user1
```

## Comandos para editar y ver contenido de archivos

### comando: vi

El comando `vi` es usado para editar archivos de texto usando el estandard ANSI.<br>
Solo podemos editar un archivo a la ves en la misma terminal.
```bash
comando>> vi pets.txt
```

### comando: cat

Usamos el comando `cat` para ver el contenido de un archivo.
```bash
comando>> cat pets.txt
cat
dog
```

### comando: touch, cp

El comando `touch` se usa para crear archivos si no existen.<br>
El comando `cp` copia carpetas o directorios. 

{: .note }
Es necesario usar la bandera `-r` para copiar carpetas.<br>

```bash
cd /home/user1/Documents

comando>> mkdir animals
comando>> ls 
animals pets.txt
comando>> mv pets.txt animals/

comando>> cp file1.txt file2.txt
comando>> cp -r animals animals2
```

El comando tambien refresca la fecha del recurso.
```bash
comando>> touch file1.txt
```

## Comandos para borrar archivos y carpetas

### comando: rm

El comando `rm` se usa para borrar archivos o carpetas<br>
En el caso de carpetas, debemos usar la bandera `-r` para tomar la accion de forma recursiva.<br> 
La bandera `-f` es para forzar la accion sin pedir confirmacion.
```bash
comando>> rm file2.txt
comando>> rm -rf animals2
comando>> ls -l
total 4
drwxrwxr-x 2 user1 user1 4096 Jan  5 06:30 animals
-rw-rw-r-- 1 user1 user1    0 Jan  5 06:37 file1.txt
```

En caso que no queremos borrar algo for accidente, podemos usar la bandera `-i` para operaciones con `rm`. <br>
La bandera `-i` indica una accion interactiva que require una respuesta `y` para "yes" o `n` para "no".
```bash
-> rm -rfi workdir/
rm: remove directory 'workdir/'? yes
```

## Comandos de fecha y calendario

Entrando el comando `date` sin argumentos muestra la fecha de hoy.
```bash
->  date
Fri Feb  2 04:22:33 UTC 2024
```

Esto muestra el mes de Enero del año `2030`.
```bash
-> cal Jan 2030
```

Cabe notar que el comando `date` es muy versatil en mostrar diferentes parametetros de tiempo que son frecuentemente usados en programacion.

Por ejemplo, esto muestra la hora, minutos y segundos.
```bash
-> date "+%H:%M:%S"
```


El comando `touch` refresca la fecha de un archivo or carpeta.
```bash
-> touch file1.txt
```

## Referencias 

### Glosario De Comandos

Los comandos siguientes son usados frecuentemente en sesiones de Linux.

mkdir
: abreviacion de `make directory`, sirve para crear carpetas

cd
: abreviacion de `change directory`, sirve para cambiar de una carpeta a otra

vi
: crear, o editar archivos

ls
: listar archivos y carpetas

cat
: mostrar el contenido de un archivo

touch
: crear archivos si no existen. Refrescar la fecha del recurso.

cp
: copiar carpetas o directorios. 

date
: mostrar la fecha de hoy o calendario deseado

find
; encontrar files y archivos

locate
; encontrar files y archivos

### Referencias Utiles

DevEsp :: el indicador
- https://docs.devesp.com/linux-conceptos/#el-indicator-the-prompt

DevEsp :: repositorio de Github
- https://github.com/devesplab/linux-devesp.git

Comandos `find`, y `locate`
- https://www.gnu.org/software/findutils/

Editor `vi`
- https://www.vim.org/docs.php