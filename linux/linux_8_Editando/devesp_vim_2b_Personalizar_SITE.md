---
layout: default
title: Personarlizar VIM
permalink: /personarlizar-vim/
parent: Editores
grand_parent: Linux
has_children: true
has_toc: false
nav_order: 8
---

# LINUX :: Editores :: Personalizar VIM

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

VIM es corto para "VI Improved" o "VI Mejorado".

Muchos usuarios originalmente han usado el editor VI, pero luego el editor VIM se hizo disponible convirtiendose en un favorito de la comunidad de Linux.

VIM es un editor que usa un interfaz de texto. Tiene las características suficientes que cumple con las necesidades básicas de la mayoría de usuarios.

En esta leccion:
- Personalizaciones en VIM

**DEPENDENCIAS**

ninguna

**REQUERIMIENTOS**

Sistema de linux Ubuntu. <br>
Alguos comandos requieren privilegios elevados.

**ADVERTENCIA**

ninguna.

## Working Environment

En esta leccion usamos el sistema operativo Ubuntu.

## Personalizaciones en VIM

Los ajustes discutidos aqui resaltan ciertas características visuales del editor que el usuario encontraría muy útiles, por ejemplo:
- ver el número de las lineas
- ver código colorizado
- tabulación automática para mejor legibilidad

{: .highlight }
Los ajustes de VIM discutidos en esta sección son opcionales. No se requieren para empezar usar VIM de manera immediata.

Para resaltar el uso de VIM, creamos el archivo escondido `$HOME/.vimrc`con el contenido mostrado.
```
syntax enable
set nu
set smartindent
set expandtab
set shiftwidth=2
set softtabstop=2
set tabstop=2
```

> El uso del número `2` es solo un ejemplo que podemos cambiar a cualquier otro número que deseamos.

El uso de cada opción es a seguir:

syntax enable
: permite resaltar la sintaxis para varios lenguajes de programación y tipos de archivos 

nu
: muestra los números de línea en el lado izquierdo de la ventana del editor.

smartindent
: sangría automáticamente el texto según la sangría de las líneas anteriores

expandtab
: use espacios en lugar de tabulaciones para la sangría

shiftwidth=n
: establece el número de espacios que se utilizarán para cada nivel de sangría en `n`.

softtabstop=n
: establece el número de espacios para que se inserte una <Tab> al presionar la tecla tab en `n`.

tabstop=n
: establece el número de espacios para que un carácter <Tab> se muestre como 2 espacios


## Cambiar el Esquema de Color

- [Vim clor schemes](https://linuxhandbook.com/vim-color-schemes/) -- DO NOT PUBLISH ON MY SITE

Crea esta directorio.
```
mkdir -p ~/.vim/colors/
```

Clona este repositorio de git en el sistema local.
```
-> git clone https://github.com/rafi/awesome-vim-colorschemes.git
```

Cambia al directorio que contiene los archivos esquemas de colors 
```
-> cd ~/workdir/awesome-vim-colorschemes/colors
```

Copia los archivos `.vim` al directorio `~/.vim/colors/`.
```
cp *.vim ~/.vim/colors/
```

Al empezar VIM, puedes escribr la instrucción siguiente para obtener la lista de esquemas de colores disponibles.
```
:colorscheme [space] [CTRLD-D]
```

Veraz algo asi (esta es una vista parcial) con la lista de esquemas de colores listados en la parta baja del editor.
```bash                                                                               
  1
~
~
~
~                                      VIM - Vi IMproved
~
~                                      version 8.2.2121
~                                  by Bram Moolenaar et al.
~                           Modified by team+vim@tracker.debian.org
~                         Vim is open source and freely distributable
~
~                                Help poor children in Uganda!
~                       type  :help iccf<Enter>       for information
~
~                       type  :q<Enter>               to exit
~                       type  :help<Enter>  or  <F1>  for on-line help
~                       type  :help version8<Enter>   for version info
~
~
~
:colorscheme
256_noir           darkblue           gotham256          molokayo           parsec         
OceanicNext        deep-space         gruvbox            morning            peachpuff      
OceanicNextLight   default            happy_hacking      mountaineer        pink-moon      
blue               fogbell_light      materialbox        orange-moon        snow           
carbonized-dark    fogbell_lite       meta5              orbital            solarized8     
carbonized-light   github             minimalist         pablo              solarized8_flat
challenger_deep    gotham             molokai            paramount          solarized8_high
:colorscheme
```


Para usar un esquema de color temporalmente, usa el comando siguiente.
```
:colorscheme [scheme-name]
```

## Conclusion

Cada individuo tiene preferencias. VIM ofrece un rango limitado de características que son suficientes para resaltar la experiencia del usuario.

## Referencias 

### Glosario De Comandos

Los comandos siguientes son usados frecuentemente en sesiones de Linux.

set
: comanod usado para poner en lugar ajustes de entorno

### Referencias Utiles

DevEsp :: Linux
- https://docs.devesp.com/linux-en-espa%C3%B1ol/

Paginas Manuales
- [vim](https://manpages.ubuntu.com/manpages/focal/en/man1/vim.1.html)
= [set](https://manpages.ubuntu.com/manpages/focal/en/man1/set.1posix.html)

Haz click en el enlace para ir al sitio red del editor.
- [Vim](https://www.vim.org/)
- [Libro para aprender Vim Gratis](https://riptutorial.com/ebook/vim)
- [Vim color themes](https://github.com/rafi/awesome-vim-colorschemes)