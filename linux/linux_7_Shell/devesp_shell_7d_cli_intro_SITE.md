---
layout: default
title: El Indicador
permalink: /indicador/
parent: El Shell
grand_parent: Linux
has_children: true
has_toc: false
nav_order: 3
---

# LINUX :: cli intro :: el indicador 

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
- que es la linea de comandos
- que es un comando?
- demonstracion

Exploremos respuestas as las preguntas siguientes: <br>
- Que puedo hacer en la linea de comandos?
- Como saber si una aplicacion esta disponible para mi uso?
- Como moverse en el sistema? Donde puedo ir? 

**DEPENDENCIAS**

ninguna

**REQUERIMIENTOS**

Sistema de linux Ubuntu. <br>
Requiere acceso a la Linea de Comandos en una terminal de Linux.

**ADVERTENCIA**

ninguna.

## Working Environment

En esta leccion usamos Ubuntu 22.04.

```bash
devuser@ubuntu2204-1-devesp
~
hist:6 -> cat /etc/lsb-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=22.04
DISTRIB_CODENAME=jammy
DISTRIB_DESCRIPTION="Ubuntu 22.04.2 LTS"
```
 
Usaremos el BASH shell.
```
-> echo $SHELL
/bin/bash
```


## El Indicador

Primero establescamos que el indicador es el area en la terminal donde entramos comandos.

Para empezar, cambiamos a un usuario base que no tiene nada configurado en su ambiente.
[comment]: # (login as user1)
```
user1@ubuntu2204-1-devesp:~$
```
Este ejemplo muestra el indicador per defecto on sistemas de ubuntu.
- La primera parte `user1` es el nombre del usuario.
- Luego `ubuntu2204-1-devesp` es el nombre del sistema.
- El simbolo `$` al final de la linea indica un usuario regular.
- EL simbolo `~` indica que estamos en el directorio hogar base.

Linux usa la variable de ambient `PS1` para configurar el indicador.

En el ejemplo anterior la variable de ambiente `PS1` tiene ajustes predefinidos por el sistema operativo.
```bash
user1@ubuntu2204-1-devesp:~$ echo $PS1
${debian_chroot:+($debian_chroot)}\u@\h:\w\$
```

Los usuarios pueden definir su propio indicador.<br>
El indicador puede cambiarse usando el comando `export` y la variable de ambiente `PS1`.
```bash
export PS1='$ '
export PS1='comando>> '
```
El ajuste hecho directamente en el indicador es transitorio, es decir, desaparace tan pronto como cerramos la terminal.<br>
Para hacer el cambio permanente podemos editar `$HOME/.basrhc` y agregar el ajuste.
```bash
-> vi $HOME/.basrhc
export PS1='comando>> '
```
Luego tenemos que implementar el cambio
```bash
-> source $HOME/.basrhc
```

## Localidad Inicial

A continuaction, establescamos donde no encontramos cuando entramos al sistema.

Usemos el comando `echo` y el simbolo `~` para ver la localidad inicial cuando hacemos login.
```bash
user1@ubuntu2204-1-devesp:~$ echo ~
/home/user1

user1@ubuntu2204-1-devesp:~$ echo $HOME
/home/user1
```

En cualquier momento podemos usar el comando `pwd` para saber donde estamos.
```bash
user1@ubuntu2204-1-devesp:~$ pwd
/home/user1
```

No importa donde nos encontremos, podemos usar el comando `cd` para volver al directorio hogar `$HOME`.
```bash
user1@ubuntu2204-1-devesp:~$ cd
```
{: .note }
El comando cd siempre nos regresa a `$HOME` sin importar donde estamos en el sistema de archivos

## Interactuando con el Sistema

En Linux entramos comandos en la terminal, en el indicador para interactuar con el sistema.

Este comando muestra el texto en doble comillas
```bash
comando>>  echo "Hola, DevEsp"
Hola, DevEsp
```

Este comando muestra quien soy
```bash
comando>> whoami
devuser
```

## Conclusion

Esto concluye la introduction a la Linea de Comandos y el indicador en Linux.

Vimos lo siguiente:
- el indicador se puede personalizar al gusto del usuario
- tenemos abilidad de usar varios comandos para manejar archivos y carpetas
- debe tenerse en cuenta los permisos del usuario de acuerdo donde se encuentra

Los animo a practicar lo aprendido.
Esta base es un comienzo para entrar y navegar el sistema.

## Referencias 

### Glosario De Comandos

Los comandos siguientes son usados frecuentemente en sesiones de Linux.

echo
: printer algo en la terminal o archivo

export
: exportar una variable de ambiente

source
: activar cambios en los archivos de ambiente tales como `.bashrc`

pwd
: mostrar el paso al directorio corriente donde nos encontramos

whoami
: identifca el nombre del usuario

### Referencias Utiles

DevEsp :: el indicador
- https://docs.devesp.com/linux-conceptos/#el-indicator-the-prompt

The Linux Documentation Project (TLDP)
- https://tldp.org/

Where to practice linux
- https://duckduckgo.com/?q=linux+simulator+online+free&t=ffab&ia=web
- https://webminal.org/
- https://cocalc.com/features/linux
- https://linuxsurvival.com/
- [10] https://www.terminaltemple.com/
- [10] https://bellard.org/jslinux/
