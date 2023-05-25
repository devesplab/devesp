---
layout: default
title: Implementación
permalink: /linux_implementacion/
parent: Linux
has_toc: false
nav_order: 3
---

# Personalización y Extensión

{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

Algunos sabores de sistemas operativos son mas facil de usar desde punto de vista del usuario casual, mientras que otros son para uso serio en un ambiente de production.

Una de los mejores características de Linux es que puede personalizarse de la manera que deseamos.
## Ambiente en Linux

El ambiente del usuario es un conjunto de elementos que vienen a hacer la manera en la que el usuario puede interactuar con el sistema operativo.

Para poder entra a un sistema de Linux, necesitamos una cuenta de usuario.
Una cuenta típica de usuario se ve de esta manera:
```
devuser:x:2085:2086::/home/devuser:/bin/bash
```
Esa entrada muestra el nombre del usuario `devuser`, el directorio de inicio `/home/devuser` y el shell `/bin/bash`. Cada uno de esos detalles puede ser personalizado. Discutiremos cada  parte en otro articulo.

## Directorio de Inicio

Cada usuario puede ver su Directorio de Inicio en manera differente.

El directorio puded referirse usando el símbolo de tilde `~` o la variable de ambiente `$HOME`.

Si estamos algun otro directorio y queremos ir al directorio de inicio, usemos el comando `cd` como en los ejemplos que siguen.

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

Para verificar que estamos en nuestro directorio podemos usar varias opciones.
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

Linux provee varias variables de ambiente precargadas y disponible para uso immediatate.

Podemos usar el comando `env` para ver las variables disponibles.

En CentOS 8 Stream se ve de esta manera.

```
devuser@centos8-2 [DevEsp]
~
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

En Ubuntu 22.04 se ve de esta manera.

```
devuser@ubuntu2204-1 [DevEsp]
~
hist:66 -> env
SHELL=/bin/bash
PWD=/home/devuser
LOGNAME=devuser
HOME=/home/devuser
TERM=xterm
USER=devuser
SHLVL=1
PATH=/home/devuser/bin:/home/devuser/Library/Python/3.9/bin:/home/devuser/bin:/home/devuser/Library/Python/3.9/bin:/home/devuser/bin:/home/devuser/Library/Python/3.9/bin:/home/devuser/bin:/home/devuser/Library/Python/3.9/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
MAIL=/var/mail/devuser
OLDPWD=/etc
_=/usr/bin/env

```

Cada una de las entrades an letras mayúsculas es una variable de ambiente.

SHELL
: el interfaz que permite entrar comandos que permite al usuario interactuar con el sistema operativo.

PWD
: el paso al directorio en el que nos encontramos en un momento dado

LOGNAME
: el nombre del usuario reconozido como el nombre para entra al sistema

HOME
: el directorio de inicio

TERM
: el tipo de terminal asignada de facto for el sistema operativo

USER
: el nombre de usuario, lo mismo que LOGNAME

PATH
: colecciíon de directorios con paso absolute para dar accesso a utilidades y comandos

MAIL
: paso absolute para donde el usuario recibe correo electrónico

HISTIZE
: valor configurable para la longitud del almacenamiento de comando ejecutados

## El Prompt

El prompt marca la parte de la terminal donde podemos entrar comandos. Generalmente se indica con el signo `$`.

El prompt es primariamente designado con la variable de ambiente `PS1`, la cual es configurable de la manera que nos plazca. Podemos designar cualquier simbolo. 

En el ejemplo que sigue, el signo de `$` es de facto. Podemos usar el comando `export` y cambiarlo a `comando>> `. Luego usamos `echo` para verificar el ajuste.
```
$
$ export PS1='comando>> '
comando>> 
comando>> echo $PS1
comando>>
```

Discutiremos el uso del comando `export` en otro documento.

[Return to main page]({{site.baseurl}}/).