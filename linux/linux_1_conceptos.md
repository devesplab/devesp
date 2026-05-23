---
layout: default
title: Conceptos
permalink: /linux-conceptos/
parent: Linux
has_toc: false
nav_order: 1
---

# Conceptos e Introducción a Linux
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

## Estructura del Sistema De Archivos (Filesystem)

La estructura general del sistema de archivos de Linux puede representarse the esta manera.
```

                                          ┌──────────────┐
                                          │   root       │
         ┌───────────────────┬────────────┴───────┬──────┴───────────┬──────────────────┐
         │                   │                    │                  │                  │
  ┌──────▼────┐      ┌───────▼──────┐     ┌───────▼──────┐    ┌──────▼──────┐    ┌──────▼─────┐
  │    /home  │      │    /usr      │     │    /var      │    │    /opt     │    │   /mnt     │
  └───────────┘      └──────────────┘     └──────────────┘    └─────────────┘    └────────────┘
```

El diseño, organizacíon y jerarquia del sistema de archivos puede modificarse para satisfacer necesidades específicas de cada institución en particular.

El admininistrador de sistemas tiene la libertad the hacer ajustes en lo pertinente a la localizacíon absoluta del archivo asi como el tamaño correspondiente de cada sección.

En este ejemplo, el comando `df` nos muestra la organización típica de un maquina virtual de CentOS 8 Stream.
```bash
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
overlay          59G   22G   35G  39% /
tmpfs            64M     0   64M   0% /dev
shm              64M     0   64M   0% /dev/shm
tmpfs           7.9G  377M  7.5G   5% /run
```

Cada columna signifca lo siguiente:

Filesystem
: nombre del archivo

Size
: Tamaño en Gigabytes

Used
: Cantidad de Gigabytes en uso

Avail
: Cantidad de Gigabytes disponibles para uso

Use%
: Porcentaje de la Cantidad de Gigabytes que estan en uso

Mounted on
: localización del paso absoluto donde reside el archivo en la jerarquia del sistema

La estructura es similar en Ubuntu.

## La Terminal 

La terminal es el area de trabajo donde podemos escribir comandos para interactuar con el sistema.

![](../../assets/images/terminal_example.png)

## El Shell

El [Shell](./linux_7_Shell.md) es el programa que accepta los comandos que entramos y los ejecuta para realizar la acción deseada en el sistema. El Shell es la manera fundamental como interactuamos con el Sistema Operativo. 

Típicamente, Linux ofrece el BASH shell de entrada. Pero hay otros que podemos escoger tales como SH, CSH, TCSH, o ZSH.

Cuando entramos al sistema, decimos que estamos en el Shell. Podemos usar la variable de ambiente `$SHELL` para saber cual Shell tenemos. En el ejemplo que sigue, tenemos el BOURNE SHELL o SH.

```bash
$ echo $SHELL
/bin/sh
```

El Shell provee el indicador que es designado por el signo dólar `$` arriba. El indicador viene a ser el lugar donde podemos entrar comandos. Esto se conoce como la Linea De Comandos.

En la documentación general de este sitio, a menos que indiquemos de otra manera, usaremos el BASH shell en los ejemplos y ejercicions que hemos de exponer.

## Linea De Comandos (Command Line)

El [Indicador](./linux_7_Shell/devesp_shell_7d_cli_intro_SITE.md) es el área de la terminal donde entramos las instrucciones que queremos mandar al sistema operativo; esta area también es conocida come _La Linea de Comandos_ or CLI. EL shell esta encargado de interpretar los comandos y los pasa al Kernel para ejecutar la tarea especifica.

{: .note }
Nos vamos a referir a la Linea De Comandos for sus siglas en Ingles: **CLI**, lo que significa _Command Line Inteface_ o Interfaz de la Linea de Comandos.

Generalmente, la CLI se identifica for el signo de dólar `$` cuando entramos al sistema, pero podemos personalizar ese detalle en cualquier momento.

En este ejemplo, escribimos comandos que nos ayudan a identificar el usuario con que hemos entrado al sistema.
```bash
$ id
uid=0(root) gid=0(root) groups=0(root)

$ whoami
root
```

Ha veces, cuando los comandos son largos y complejos, nos vemos en la necesidad de editar para afectar el resultado deseado. Las siguientes combinaciones de teclados facilitan la manipulación de la linea de la CLI.

CTRL-a
: Mover el cursor al principio de la linea de entrada

CTRL-e
: Mover el cursor al final de la linea de entrada

CTRL-l
: borrar todo el texto de la terminal

CTRL-u
: borra la linea de entrada completamente

CTRL-_
: revertir la última acción de teclado

ENTER
: mandar el comando al shell (bash, csh, zsh, etc)

DEL
: borrar el simbolo en el que se encuentra el cursor

Tipicamente, en RedHat el indicador predeterminado se ve asi:
```bash
[user2@rhel9-1-devesp ~]$
```
Mientras que Ubuntu se ve asi:
```bash
$
```

Podeos escribir comandos en el area marcada for el signo `$`.

## El Indicator (The Prompt)

El  [Indicador](./linux_7_Shell/devesp_shell_7d_cli_intro_SITE.md) marca la parte de la terminal donde podemos entrar comandos. Generalmente se indica con el signo `$`.

{: .warning }
Si el indicador muestra el signo `#` en lugar de `$`, indica que hemos entrado con la cuenta del superusuario (root), la cual tiene control absoluto del sistema. Un solo comando equivocado y podemos causar gran daño.

El indicador es primariamente designado con la variable de ambiente `PS1`, la cual es configurable de la manera que nos plazca. Podemos designar cualquier símbolo en lugar de `$` o `#`. 

En el ejemplo que sigue, el signo de `$` es por defecto. Podemos usar el comando `export` para cambiarlo a `comando>> `. Luego usamos `echo` para verificar el ajuste.
```bash
$
$ export PS1='comando>> '
comando>> 
comando>> echo $PS1
comando>>
```

Discutiremos el uso del comando `export` en otro documento.
## Directorio De Inicio (Home Directory)

El directorio de inicio, o directorio hogar, es donde aterrizamos cuando entramos al sistema y es nuestra base de operaciones. Es aquí donde creamos y mantenemos todos lo archivos, carpetas, documentos, imagenes, programas y personalizaciones particulares nuestro entorno de usuario.

Tan pronto como entramos al sistema, el comando `pwd` nos asiste para mostrarnos la localidad del directorio de inicio.
```bash
$ pwd
/root
```

Podemos usar `echo` para mostrar el directorio de inicio en cualquier momento que estamos en el sistema.
```bash
$ echo $HOME
/root
```
La localidad estándar donde Linux crea carpetas de inicio es bajo `/home`, de manera que usualmente un usuario tendría `/home/<usuario>` como la localidad de hogar. La localidad puede cambiarse de acuerdo al diseño del administrador de sistemas que maneja las cuentas de usuariois.

## Referencias 

Para mas información ver los documentos a seguir:

- El [Shell](./linux_7_Shell.md)
- El [Indicador](./linux_7_Shell/devesp_shell_7d_cli_intro_SITE.md)

[Return to main page]({{site.baseurl}}/).