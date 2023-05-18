---
layout: default
title: DirColors de la Terminal
permalink: /linux_commands/linux_dircolors/
parent: Comandos y Procesos
grand_parent: Linux
has_children: false
has_toc: false
nav_order: 4
---

# Colorizar Con El Comando ls

{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>


Normalmente cuando usamos el comando `ls` para listar archivos, la salida son colores de facto.
Esto se puede modifiar para ver la salida del comando con ciertos colores que ayuda visualmente para leer mejor lo que vemos en la pantalla.

El proceso para abilitar esto es simple:
* instalar el paquete requerido
* obtener el archivo de configuración y modificarlo
* crear personalizaciones deseadas

## Instalar el Paquete

En **CentOS 8 Stream**, es necesario instalar el paquete `coreutils-common` para poder usar colorizacion en el listado the archivos y carpetas.
```
root@centos8-1 [DevEsp]
hist:343 -> yum install coreutils-common-8.30-12.el8.x86_64
Last metadata expiration check: 21:25:08 ago on Tue 16 May 2023 08:04:12 PM PDT.
Dependencies resolved.
================================================================================
 Package                          Architecture   Version      Repository   Size
================================================================================
 coreutils-common                 x86_64         8.30-12.el8  baseos    
================================================================================
Install  1 Package
```

Solo dos archivos son necesarios para esta tarea:

* **/etc/DIR_COLORS** :: Archivo de configuración para el sistema entero 
* **~/.dir_colors** :: Archivo de configuración para el usuario individual

La instalacion crea el archivo `/etc/DIR_COLORS`
```
root@centos8-1 [DevEsp]
hist:344 -> ls -l /etc/DIR_COLORS
-rw-r--r-- 1 root root 4536 Jul 13  2021 /etc/DIR_COLORS
```

Por defacto, este archivo va en el directorio `/etc`, y debe ser legible por el mundo.
```
-> ls -l /etc/DIR_COLORS
-rw-r--r-- 1 root root 4536 Jul 13  2021 /etc/DIR_COLORS
```

Podemos copiar este archivo a `.dir_colors` en nuestro directorio de inicio $HOME para re-escribir los valores predeterminados del sistema
```
-> sudo cp /etc/DIR_COLORS ~/.dir_colors
```

## Configurar El Ambiente

Una vez que hemos copiado el archivo a nuestro directorio de inicio, agregemos esta linea en `~/.bash_profile` para abilitar esta característica automáticamente cada vez que entremos al sistema.
```
eval $(dircolors ~/.dir_colors)    
```
Esta forma es mejor porque verifica que el archivo existe y si no, entoces lee la base de datos de facto disponible internamente en el sistema.
```
[ -e ~/.dircolors ] && eval $(dircolors -b ~/.dircolors) || eval $(dircolors -b)
```

{: .note }
> El comando `dircolors` usa la variable the ambient LS_COLORS para determinar los colores con los cuales mostrará la salida del comando `ls`.


## Usando Color Con Salida de Comandos

Ahora podemos usar el parámetro `--color` con el comando `ls` para ver la salida del comando en color.
```
-> ls -l --color /usr/bin
```
Ahora, usando cualquiera de esos aliases resultará en salida de comando colorizado, por ejemplo este
alias produce un listado de archivos formateado en tres columnas.
```
-> ls
```
Si no deseamos color podemos hacer esto para nulificar el effecto.
```
alias  ls="ls --color=never"
```

Para comprobar la configuración podemos listar algunos directorios conocidos. 
Podemos usar un alias o el comando original.
```
-> ll /etc
-> ls --color -l /sbin
```

## Referencias

Ver las páginas manuales.
```
-> man dir_colors
-> man dircolors
```

Ver el archivo de configuración.
```
-> cat /etc/DIR_COLORS
```

Ver la página manual de [dir_colors](https://linux.die.net/man/5/dir_colors) en linea.