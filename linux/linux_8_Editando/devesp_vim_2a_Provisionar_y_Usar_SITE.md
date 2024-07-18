---
layout: default
title: Provisionar y Usar VIM
permalink: /provisionar-vim/
parent: Editores
grand_parent: Linux
has_children: true
has_toc: false
nav_order: 8
---


# LINUX :: Editores :: Provisionar y Usar VIM

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
- Instalar VIM
- Empezando VIM
- Navegación de VIM
- Personalizaciones en VIM
- Usando VIM
- Limitaciones de VIM

VIM es abbreviación de "VI Improved" o "VI Mejorado".

Muchos usuarios originalmente han usado el editor VI, pero luego el editor VIM se hizo disponible convirtiendose en un favorito de la comunidad de linux.

VIM es un editor que usa un interfaz de texto. Tiene las características suficientes que cumple con las necesidades básicas de la mayoría de usuarios.

**DEPENDENCIAS**

ninguna

**REQUERIMIENTOS**

Sistema de linux Ubuntu. <br>
Alguos comandos requieren privilegios elevados.

**ADVERTENCIA**

ninguna.

## Working Environment

En esta leccion usamos el sistema operativo Ubuntu.


## Instalar VIM

El editor VIM pueden ser instalado en la linea de comandos con la utilidad para manejar paquetes que corresponde al sistema operativo.

En Ubunu podemos hacer esto para instalar los editores:
```
-> sudo apt install vim
```

En RedHat podemos hacer esto para instalar los editores:
```
-> sudo yum install vim
```


## Empezando VIM

Primero que nada aseguremonos que VIM esta presente en nuestro sistema usando el comando `which` para localizarlo.
```
devuser@ubuntu2204-1-devesp
~
hist:243 -> which vim
/usr/bin/vim
```
Para empezar a usar VIM entramos el comando `vim` en la terminal
```
devuser@ubuntu2204-1-devesp
~
hist:243 -> vim
```

El interfaz de texto empieza y vemos lo siguiente.
```
~
~
~
~                                  VIM - Vi IMproved
~
~                                  version 8.2.2121
~                              by Bram Moolenaar et al.
~                       Modified by team+vim@tracker.debian.org
~                     Vim is open source and freely distributable
~
~                            Help poor children in Uganda!
~                   type  :help iccf<Enter>       for information
~
~                   type  :q<Enter>               to exit
~                   type  :help<Enter>  or  <F1>  for on-line help
~                   type  :help version8<Enter>   for version info
~
~
~
```

Las lineas empiezan con tilde `~` indicando que son lineas vacias y disponibles para insertar texto.

## Navegación de VIM

Una vez que hemos empezado el editor, estamos listos a interactuar con el.

{: .highlight }
No se puede usar el ratón dentro del interfaz de VIM.

Para mover el indicador dentro del interfaz de VIM, presiona las teclas h, j, k, y l como se muestra:
```
             ^
             k              Pista: la llave h esta a la izquierda y mueve a la izquierda
       < h       l >               la llave l esta a la derecha y mueve a la derecha
             j                     la llave k mueve para arriba
             v                     la llave j mueve para abajo  
```

Las teclas anteriores mueven el indicador una letra a la vez en la direccíon mostrada.

## Modos de Acción en VIM

Los usos más generales de VIM incluyen las actividades siguientes:
- Entrar una linea
- Encontrar una linea usando el numero de linea
- Mover de palabara en palabra
- Buscar un patrón
- Sustituir un patrón

VIM tiene dos modos de editar: Entrar (Input) y Comando (Command).

**Input** :: Este mode se activa al presionar la tecla `i`. En este modo podemos entrar texto para crear o actualzar un archivo .

**Comando** :: Este modo es para entrar comandos para crear acciones que resaltan la funcionalidad de VIM. Entre otras cosas, podemos guardar los cambios hechos, o movernos de una linea a otra, reponer una palabra o grupo de palabras, y finalmente salir del editor cuando hemos terminado nuestro trabajo.

La lista siguiente muestra varios de los comandos mas frequentes y útiles en el interfaz de vim.

Moverse alrededor del editor:

`i` 
: (insert) enters insert mode before the current cursor position

`0` 
: moverse al principio de la línea

`$` 
: moverse al final de la línea

`w` 
: pasaremos a la siguiente palabra

`b` 
: pasar a la palabra anterior

Añadir, borrar, sustituir, desechar:

`a`
: ingresa al modo de inserción después de la posición actual del cursor

`A` 
: ingresa al modo de inserción después del último carácter imprimible de la línea actual

`x`
: eliminar carácter en la posición actual del cursor

`s`
: sustituir el carácter en la posición actual del cursor, entra modo de editar 

`u`
: desechar el ultimo cambio

Cortar lineas:

`dd`
: corta la línea actual

`d$`
: desde la posición actual hasta el final de la línea

Abrir lineas:

`o`
: crear una nueva línea vacía después la actual e ingrese insertar

`O`
: para crear una nueva línea vacía,antes de la actual e ingrese insertar

Guardar y salir:

`:w`
: escribe el buffer actual en el disco

`:wq`
: escribir y salir

`:q!`
: salir sin escribir


## Usando VIM

Para empezar a editar, presionar la tecla `i` que indica insertar. 

La parte izquierda abajo de la terminal muestra `INSERT`(insertar), lo cual indica podemos empezar a escribir.
```
~
~
-- INSERT --
```

Para guardar el archivo en cualquier momento, 
- presiona `ESC`  
- entra la tecla `:` seguida for la letra `w`, seguida por el nombre del archivo
- presiona `Enter`
```
~
~
:w archivo1.txt
```

Al presionar `Enter`, la parte izquierda abajo de la terminal cambia con un mensaje indicando que ha registrado el contenido.
```
~
~
"archivo1.txt" [New] 1L, 14B written
```

{: .note }
Cada vez que entramos la tecla `:` ponemos VIM en modo de Comando.

Una vez que hemos terminado, 
- presiona `ESC`  
- entra la tecla `:` seguida for la letra `q`
- presiona `Enter`
```
~
~
:q
```

Con esa acción volvemos al indicador en la terminal.

## Limitaciones de VIM

Debido a su simplicidad, VIM no tiene las caracteristicas sofisticadas de otros editores que pueden usarse en tareas complejas de desarrollo de software. 
VIM se usa genralmente en entornos de texto ANSI donde no hay necesidad de un interfaz gráfico.

## Conclusion

VIM es un editor simple y versátil que vale la pena saber usar. La instalación es fácil y  requiere configuración mínima. A pesar de sus limitaciondes, es muy usado en la comunidad. Hay suficiente documentación y recursos de aprendizaje en línea.

## Referencias 

### Glosario De Comandos

Los comandos siguientes son usados frecuentemente en sesiones de Linux.

vim
: empezar el editor de VIM en la CLI

nano
: empezar el editor de NANO en la CLI

### Referencias Utiles

DevEsp :: Linux
- https://docs.devesp.com/linux-en-espa%C3%B1ol/

Paginas Manuales
- [vim](https://manpages.ubuntu.com/manpages/focal/en/man1/vim.1.html)
- [set](https://manpages.ubuntu.com/manpages/focal/en/man1/set.1posix.html)

Haz click en el enlace para ir al sitio red del editor.
- [Vim](https://www.vim.org/)
- [Libro para aprender Vim Gratis](https://riptutorial.com/ebook/vim)
- [Vim color themes](https://github.com/rafi/awesome-vim-colorschemes)