---
layout: default
title: Configurar El Bash Shell
permalink: /configurar-bash/
parent: El Shell
grand_parent: Linux
has_children: true
has_toc: false
nav_order: 2
---

# LINUX :: Bash :: uso, configuracíon, personalización

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
- Primicias del BASH Shell

{: .highlight }
Este documente es en el contexto del Sistema Operativo UBUNTU a menos que se aclare de otra manera.

**DEPENDENCIAS**

Usando `/usr/sbin/nologin` como opción de shell para un usuario causa que ese usuario no pueda entrar al sistema. Por lo tanto, no lo uses a menos que entiendas su effecto.

**REQUERIMIENTOS**

Sistema de linux Ubuntu. <br>

**ADVERTENCIA**

ninguna.

## Working Environment

En esta leccion usamos el sistema operativo Ubuntu.

## Características del BASH Shell

En su forma nativa y sin requerir ningun cambio, Bash ofrece lo necesario para empezar a trabajar immediatamente cuando entramos al sistems.

Entre otras características Bash ofrece lo siguiente:
- la abilidad de entrar comandos y ver el resultado
- functiones para agrupar comandos para tareas complejas
- combinar comandos directamete en la linea de comandos para alterar el resultado
- parámetros para guardar valores que pueden ser usados en el entorno o programas
- carácters especiales para expandir el rango de accíon de los comandos

## Primicias del BASH Shell

Para usar BASH solo entramos comandos y vemos el resultado. <br>
En esta ejemplo, usamos el comando `ping` para probar el paso de red a nuestro sitio externo de internet `docs.devesp.com`.
```
devuser@ubuntu2204-1-devesp  
~/linux-devesp
hist:208 -> ping -c 1 docs.devesp.com
PING devesplab.github.io (185.199.108.153) 56(84) bytes of data.
64 bytes from cdn-185-199-108-153.github.com (185.199.108.153): icmp_seq=1 ttl=62 time=31.0 ms
--- devesplab.github.io ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 31.012/31.012/31.012/0.000 ms
```

## Usando Aliases en BASH

Podemos crear aliases que combinan banderas para obtener mejores resultados en la salida de comandos. <br>
Por ejemplo, aqui creamos el alias `ll='ls --color -l'` que nos premite usar `ll` (equivalente a `ls -l`) y obtener una lista larga de archivos.
```
devuser@ubuntu2204-1-devesp
~/linux-devesp
hist:210 -> cat ~/.aliasrc
alias ls='ls --color -C1F'
alias ll='ls --color -l'
alias la='ls --color -la'
alias ld='ls --color -1F'

devuser@ubuntu2204-1-devesp
~/linux-devesp
hist:211 -> ll
total 56
-rw-rw-r-- 1 devuser devuser 35149 May 19 02:22 LICENSE
-rw-rw-r-- 1 devuser devuser   693 May 19 02:22 README.md
drwxrwxr-x 3 devuser devuser  4096 May 19 02:22 linux-docs/
drwxrwxr-x 6 devuser devuser  4096 May 19 02:22 linux-files/
drwxrwxr-x 3 devuser devuser  4096 May 19 02:22 linux-labs/
drwxrwxr-x 2 devuser devuser  4096 May 25 22:45 linux-scripts/
```

### Usando Programas en BASH

El BASH shell permite crear, ver y ejecutar programas. <br>
En este ejemplo hacemos lo siguiente:
- usamos el comando `cat` para mostrar el contenido del programa `greeting.sh`
- usamos el comand `ls` que muestra el permiso de ejecutar del programa
- corremos el programa que crea un archivo de registro
- usamos el comando `cat` para mostrar el contenido del archivo de registro

```bash
Fri 2024Jun21 03:29:23 UTC
devuser@ubuntu2204-1-devesp 
~/linux-devesp/linux-scripts
hist:209 -> cat greeting.sh
#!/bin/bash
#
# Di hola y muestra la fecha
#
DATE=`date +"%m/%d/%y"`
LOG=greeting.log

[ -f ${LOG} ] || echo "INFO: creando ${LOG} porque no existe" && touch ${LOG}

function greeting(){
  echo "Hola, Mundo! Hoy es ${DATE}" > ${LOG}
}

greeting
```

Corramos el programa

```
devuser@ubuntu2204-1-devesp 
~/linux-devesp/linux-scripts
hist:210 -> ls -l greeting.sh
-rwxrwxr-x 1 devuser devuser 246 May 19 02:22 greeting.sh*

devuser@ubuntu2204-1-devesp 
~/linux-devesp/linux-scripts
hist:211 -> ./greeting.sh
INFO: creando greeting.log porque no existe

devuser@ubuntu2204-1-devesp 
~/linux-devesp/linux-scripts
hist:212 -> ./greeting.sh

devuser@ubuntu2204-1-devesp 
~/linux-devesp/linux-scripts
hist:213 -> cat greeting.log
Hola, Mundo! Hoy es 06/21/24
```

## Exportando Variables en BASH

El Bash Shell provee varias variables en el ambiente del usuario. Estas pueden verse con el comando `env`.
```
devuser@ubuntu2204-1-devesp
~
hist:213 -> env
SHELL=/bin/bash
GITTOKEN=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
PWD=/home/devuser
LOGNAME=devuser
XDG_SESSION_TYPE=tty
HOME=/home/devuser
GITTOKENDEVESP=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
XDG_SESSION_CLASS=user
TERM=xterm
USER=devuser
SHLVL=1
XDG_SESSION_ID=c1
XDG_RUNTIME_DIR=/run/user/2045
PATH=/home/devuser/bin:/home/devuser/Library/Python/3.9/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
MYSITE=DevEsp
MAIL=/var/mail/devuser
OLDPWD=/home/devuser/linux-devesp/linux-scripts
_=/usr/bin/env
```

Podemos crear y usar variables de ambiente que demuestra los aspectos dinámicos y versátiles del Shell. <br>
En este ejemplo exportamos la variable `MYSITE` asignandole un valor, y luego la usamos en un comando. 
```
devuser@ubuntu2204-1-devesp
~/linux-devesp
hist:208 -> export MYSITE=DevEsp

devuser@ubuntu2204-1-devesp
~/linux-devesp
hist:209 -> echo ${MYSITE} is for learning
DevEsp is for learning
```

## Archivos Especiales de BASH

Generalmente, en un sistema de Ubuntu o RedHat se ven los archivos escondidos de la lista que sigue.
```
devuser@ubuntu2204-1-devesp
~
hist:210 ->  la
total 88
drwxr-xr-x 1 devuser devuser  4096 Jun  9 00:01 ./
drwxr-xr-x 1 root    root     4096 May 18 22:55 ../
-rw-r--r-- 1 devuser devuser  2935 Jul 15  2023 .aliasrc
-rw------- 1 devuser devuser  2922 May 28 00:56 .bash_history
-rw-r--r-- 1 devuser devuser   149 Jul 15  2023 .bash_profile
-rw-r--r-- 1 devuser devuser   647 May 26 00:23 .bashrc
-rw------- 1 devuser devuser    20 Jun  9 00:01 .lesshst
-rw-r--r-- 1 devuser devuser    35 Jul 15  2023 .profile
```
Bash usa esos archivos para diferentes actividades.

.aliasrc
: archivo que el usuario crea para crear combinanciones de comandos

.bash_history
: archivo preexistence que BASH usa para registrar las actividades del usuario en la linea de comandos

.bash_profile
: archivo de personalizacion inicial cuando el usuario entra al sistema

.bashrc
: archivo estándard de inicialización del usuario 

.lesshst
: archivo usado para registar el uso del comando `less` por el usuario

.profile
: originalmente usado por SH que esta presente solo por razones históricas pero también puede ser usado por BASH

### Archivos de Inicialización de BASH

Hablemos de dos archivos en particular que el usuario puede cambiar para personalizar su ambiente: `.bashrc` y `~/.bash_profile`.

Ejemplo de `.bashrc` mostrando la inicialización estandard a nivel de usuario.<br>
En este archivo el usuario puede poner ajustes especificos a su gusto. Un ejemplo puede ser:
- agregar aliass del archivo `.aliasrc`
- declarar personalizaciones para manejo de historia de comandos
- modificar la variable de ambiete `PS1` para el indicador en la terminal
- usar el comando `export` para agregar pasos a carpetas que continenen untilidades
```
devuser@ubuntu2204-1-devesp
~
hist:211 -> cat ~/.bashrc
source ~/.aliasrc
HISTCONTROL='ignoreboth:erasedups'
PS1='-->'
export PATH=$HOME/bin:$HOME/Library/Python/3.9/bin:$HOME/mycode:$PATH
```

Ejemplo de `.bash_profile` mostrando la inicialización estandard a nivel de sistema. Nótese que incluye una instrucción para cargar el contenido de ` ~/.bashrc` si existe.
```
devuser@ubuntu2204-1-devesp
~
hist:210 -> cat ~/.bash_profile
# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
    . ~/.bashrc
    fi

# User specific environment and startup programs
```

## Expansión en BASH

En bash existen ciertas facilidades que permiten el manejo inteligente de ciertas tareas.

Los tipos de expaciones so a seguir:
- expansión de corsé
- expansión de tilde
- expansión de parámetros y variables
- sustitución de comando
- expansión aritmética
- división de palabras
- expansión de nombre de archivo

Discutiremos expansión en BASH on otra lección. Pero veamos unos ejemplos simples y de uso frequente aquí.

### expansión de tilde

En Bash la tilde `~` denota el directorio hogar del  usuario. Podemos demostrar esto usando el comando `echo`.
```
devuser@ubuntu2204-1-devesp  
~
hist:217 -> echo ~
/home/devuser
```
Asi que donde sea que usemos `~` se traduce al directorio hogar; en nuestra ejempo es `/home/devuser`.

Usando el tilde veamos la lista de archivos en el directorio `linux-devesp/linux-scripts`.
```
devuser@ubuntu2204-1-devesp
~
hist:217 -> ls ~/linux-devesp/linux-scripts
file-update-timestamp.sh*
forever-loop.sh*
greeting.log
greeting.sh*
```
Usando `echo` vemos el paso absoluto del programa.
```
devuser@ubuntu2204-1-devesp
~
hist:218 -> echo ~/linux-devesp/linux-scripts
/home/devuser/linux-devesp/linux-scripts
```

{: .highlight }
Los archivos y programas usados aqui estan disponibles en [linux-devesp](https://github.com/devesplab/linux-devesp.git)

Podemos ejecutar el program de esta manera.
```
devuser@ubuntu2204-1-devesp
~
hist:219 -> ~/linux-devesp/linux-scripts/forever-loop.sh

Hello, DevESP... today is Sat Jun 22 18:51:52 UTC 2024
```

### Expansión de Corsé

Aqui usamos la variable de entorno `$HOME` y el corsé para agrupar varios directorios a crear simultaneamente.
```
mkdir $HOME/{dir1,dir2,dir3}
```
El resultado se ve así:
```
devuser@ubuntu2204-1-devesp
~
hist:210 -> ll
total 20
drwxrwxr-x 2 devuser devuser 4096 Jun 22 18:42 dir1/
drwxrwxr-x 2 devuser devuser 4096 Jun 22 18:42 dir2/
drwxrwxr-x 2 devuser devuser 4096 Jun 22 18:42 dir3/
```

### Expansión de nombre de archivo

El símbolo `*` es comúnmente usado para encapsular el patrón de nombre de varios archivos.

Este ejemplo muestra que capturamos cualquier símbolo con `*` seguido por la extensión `.sh`.
```
devuser@ubuntu2204-1-devesp  
~/linux-devesp/linux-scripts
hist:221 -> ls -l *.sh
-rwxrw-rw- 1 devuser devuser 3926 May 25 22:45 file-update-timestamp.sh*
-rwxrw-rw- 1 devuser devuser  178 May 25 22:45 forever-loop.sh*
-rwxrwxr-x 1 devuser devuser  246 May 19 02:22 greeting.sh*
```
Básicamente hemos listado cualquier programa de bash, esos que tienen `.sh` como [^extension de archivo].

## Conclusion

El SH y BASH shell es una de las primeras creaciones altamente adoptadas por la comunidad usuaria. Ofrecen muchas características versátiles que hacen su uso muy atractivo. El uso del SHELL varia desde lo más simple tales como entrar comandos para ver archivos, a lo más complejos tales como escriber programas para automatizar tareas complejas.

Es recomendable aprender como usar el shell de manera proficiente para tomar ventaja de muchos usos versatiles. 

## Referencias 

### Glosario De Comandos

Los comandos siguientes son usados frecuentemente en sesiones de Linux.

[^cuentas de sistema]: Tipo de cuenta que no es usuada por algun usuario para entrar al sistema. El propósito de una cuenta de sistema es generalmente para actividades administrativas o ejecución de apllicaciones de terceros que no requieren acción directa de usuarios a travéz de la linea de comandos en una terminal.

[^extension de archivo]: se refiera al grupo de letras con que finaliza un nombre de archivo y que generalmente indica el tipo de función que ofrece o el tipo de aplicacíon que se requiere para operarlo. Por ejemplo: un documento de Adobe Acrobat usa `.pdf`, un documento de Microsoft Word usa `.doc`, mientras que un programa de Bash usa `.sh`.

export
: comando usado para crear variables con NOMBRE y VALOR en el SHELL

env
: sirve para mostrar variables con NOMBRE y VALOR en el ambiente del usario.

### Referencias Utiles

DevEsp :: Linux
- https://docs.devesp.com/linux-en-espa%C3%B1ol/

Paginas Manuales
- [Bash Shell](https://manpages.ubuntu.com/manpages/focal/en/man1/bash.1.html)
- [SH Shell](https://manpages.ubuntu.com/manpages/focal/en/man1/sh.1.html.1.html)
- [nologin](https://manpages.ubuntu.com/manpages/focal/en/man5/nologin.5.html)
- [export](https://manpages.ubuntu.com/manpages/focal/en/man1/export.1posix.html)
- [env](https://manpages.ubuntu.com/manpages/focal/en/man1/env.1.html)
- [less](https://manpages.ubuntu.com/manpages/focal/en/man1/less.1.html)

Otras referencias
- [GNU Bash](https://www.gnu.org/software/bash//)
- [Referencia del Manual de Bash](https://www.gnu.org/software/bash/manual/bash.html)
