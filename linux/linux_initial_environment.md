---
layout: default
title: Ambiente de Inicio
permalink: /linux_initial_environment/
parent: Linux
has_toc: false
nav_order: 3
---

# Ambiente de Inicio en Linux

{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

Algunos sabores de sistemas operativos son mas facil de usar desde punto de vista del usuario casual, mientras que otros son para uso serio en un ambiente de production en los que las exigencias requieren mas potencia de CPU y Memoria.

Una de los mejores características de Linux es que puede personalizarse de la manera que deseamos.

El ambiente del usuario es un conjunto de elementos que vienen a hacer la manera en la que el usuario puede interactuar con el sistema operativo.

## Cuenta de Usuario

Para poder entra a un sistema de Linux, necesitamos una cuenta de usuario.
Una cuenta típica de usuario se ve de esta manera:
```
devuser:x:2085:2086::/home/devuser:/bin/bash
```
Esa entrada muestra el nombre del usuario `devuser`, el directorio de inicio `/home/devuser` y el shell `/bin/bash`. Cada uno de esos detalles puede ser personalizado. Discutiremos cada parte en otros articulos.

## Directorio de Inicio

Cada usuario puede organizar su Directorio de Inicio en manera differente.

El directorio puede referirse usando el símbolo de tilde `~` o la variable de ambiente `$HOME`.

Si estamos en algun directorio diferente the `$HOME` y queremos ir al directorio de inicio, podemos usar el comando `cd` como en los ejemplos que siguen.

* Usar `~`
```
devuser@ubuntu2204-1 [DevEsp]
hist:60 -> cd ~
```
* Usar `$HOME`
```
devuser@ubuntu2204-1 [DevEsp]
hist:61 -> cd $HOME
```

Para verificar que estamos en nuestro `$HOME` podemos usar varias opciones.
- el comando `echo`
- el comando `pwd`
- la variable de ambiente `PWD` 

```
devuser@ubuntu2204-1 [DevEsp]
hist:62 -> echo $HOME
/home/devuser

devuser@ubuntu2204-1 [DevEsp]
hist:62 -> pwd
/home/devuser

devuser@ubuntu2204-1 [DevEsp]
hist:63 -> echo $PWD
/home/devuser
```

## Las Variables de Ambiente

Linux provee varias variables de ambiente precargadas con información útil que disponibles para uso immediato. Por ejemplo, podemos usar el comando `env` para ver las variables disponibles.

Generalmente, las variables de ambiente estan definidas en `/etc/profile`, `/etc/bashrc`, `~/.bashrc`, o `~/.bash_profile`.

En CentOS 8, el archivo del usuario `~/.bashrc` muestra un bloque indicando que lee ajustes encontrados en `/etc/bashrc`.
```
[user1@centos8-2 ~]$  cat ~/.bashrc
# .bashrc

# Source global definitions
if [ -f /etc/bashrc ]; then
	. /etc/bashrc
fi
```

Por ejemplo, en CentOS, el archivo `/etc/bashrc` tiene la definición para el SHELL por defecto.
```
SHELL=/bin/bash
```

En CentOS 8 Stream podemos usar el comando `env` para ver todas las variables de ambiente que tenemos disponibles.
```
devuser@centos8-2 [DevEsp]
hist:18 -> env
LANG=en_US.UTF-8
HISTCONTROL=ignoreboth:erasedups
HOSTNAME=centos8-2
which_declare=declare -f
USER=devuser
PWD=/home/devuser
HOME=/home/devuser
MAIL=/var/spool/mail/devuser
SHELL=/bin/bash
TERM=xterm
SHLVL=1
LOGNAME=devuser
PATH=/home/devuser/bin:/home/devuser/Library/Python/3.9/bin:/home/devuser/bin:/home/devuser/Library/Python/3.9/bin:/home/devuser/bin:/home/devuser/Library/Python/3.9/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin
HISTSIZE=1000
LESSOPEN=||/usr/bin/lesspipe.sh %s
BASH_FUNC_which%%=() {  ( alias;
 eval ${which_declare} ) | /usr/bin/which --tty-only --read-alias --read-functions --show-tilde --show-dot "$@"
}
_=/usr/bin/env
```

El comando `set` muestra información aún mas expandida organizada alfabeticamente (parcialmente extraida abajo).
```
devuser@centos8-2 [DevEsp]
hist:30 -> set
BASH=/bin/bash
BASH_VERSINFO=([0]="4" [1]="4" [2]="20" [3]="1" [4]="release" [5]="x86_64-redhat-linux-gnu")
BASH_VERSION='4.4.20(1)-release'
COLUMNS=128
DIRSTACK=()
ENVS=/vagrant/shared/DATAM2/learning/learning_env01
EUID=2074
GROUPS=()
HISTCONTROL=ignoreboth:erasedups
HISTFILE=/home/devuser/.bash_history
HISTFILESIZE=1000
HISTSIZE=1000
HOME=/home/devuser
HOSTNAME=centos8-2
```

{: .highlight }
Notese que cada una de las variables es en letras mayúsculas. 

La salida de los comandos arriba se ve de manera muy similar en Ubuntu 22.04.
El uso y propósito de estas variables es consistente a travéz de todos los sabores de Linux

El propósito de cada una es a seguir:

SHELL
: el interfaz que permite entrar comandos que permite al usuario interactuar con el sistema operativo.

PWD
: el paso al directorio en el que nos encontramos en un momento dado

LOGNAME
: el nombre del usuario reconozido como el nombre para entra al sistema

HOME
: el directorio de inicio

TERM
: el tipo de terminal asignada por defecto por el sistema operativo

USER
: el nombre de usuario, lo mismo que LOGNAME

PATH
: colecciíon de directorios con paso absolute para dar accesso a utilidades y comandos

MAIL
: paso absolute para donde el usuario recibe correo electrónico

HISTIZE
: valor configurable para la longitud del almacenamiento de comando ejecutados

## Terminal de Acceso

Una que que tenemos la cuenta de usuario y directorio de inicio, necesitamos acceder el sistema a travez de un shell usando como interfaz la terminal. 

La terminal es el area de trabajo donde podemos escribir comandos para interactuar con el sistema.


[Return to main page]({{site.baseurl}}/).