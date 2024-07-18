---
layout: default
title: Editores
permalink: /editores/
parent: Linux
has_children: true
has_toc: false
nav_order: 8
---

# LINUX :: Editores :: Conceptos

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
- Entender que es un Editor en Linux
- Tipos de Editores
- Disponibilidad de Editores

**DEPENDENCIAS**

ninguna

**REQUERIMIENTOS**

ninguno.

**ADVERTENCIA**

ninguna.

## Working Environment

En esta leccion usamos el sistema operativo Ubuntu.

## Entender que es un Editor en Linux 

Un editor en Linux es una aplicaciíon escrita con el propósito de trabajar con archivos de texto y otros formatos.

Invariablemente, al trabajar en cualquier sistema operativo nos veremos en la necesidad de editar archivos. Para ello, hay varias aplicaciones de terceros o comerciales. Las aplicaciones de terceros son generalmente de fuente abierta, mientras que las comerciales son pagadas. 

La mayoría de editoroes ofrecen un binario que puede ser instalado en varios sistemas operativos.

## Tipos de Editores

Esta tabla enumera los editores mas comúnes y el tipo de interfaz que se requiere para usarlo.

| Editor     | Interfaz | Requerimiento       | Platforma             | Licencia |
|:-----------|:-------- |:--------------------|:----------------------|:---------|
| vim        | texto    | terminal y CLI      | Linux                 | Gratis   |
| nano       | texto    | terminal y CLI      | Linux                 | Gratis   |
| emacs      | texto    | terminal y CLI      | Linux                 | Gratis   |
| vscode     | gráfico  | escritorio de Linux | Linux, MacOS, Windows | Gratis   |
| sublime    | gráfico  | escritorio de Linux | Linux, MacOS, Windows | Pagado   |
| notepad++  | gráfico  | escritorio de Linux | Windows               | Gratis   |

Talvéz el editor mas comunmente usado es VIM, el cual es muy fácil de usar en cualquier terminal de texto. Le sigue NANO por su simplicidad. Luego EMACS que es un poco más complejo, pero tiene muchas más characterísticas que los otros.

## Disponibilidad de Editores

Los editores tales como VIM, NANO, y EMASCS pueden ser instalados en la linea de comandos con el utilidad para manjera paquetes que corresponde al sistema operativo.

En Ubuntu podemos hacer esto para instalar los editores:
```
-> sudo apt install vim
-> sudo apt install nano
-> sudo apt install emacs
```

En RedHat podemos hacer esto para instalar los editores:
```
-> sudo yum install vim
-> sudo yum install nano
-> sudo yum install emacs
```

En el caso de VSCODE, SUBLIME y NOTEPAD++, se debe bajar del sitio de red correspondiente y luego usar el paquete bajado para instalar el editor. La sección de referencias en esta página contiene la lista de los sitios de read para cada aplicación.


Luego para usar el editor deseado usamos el comando que lleva el nombre de la aplicacíon que es lo mismo en Ubuntu y RedHat.
```
-> vim
-> nano
-> emacs
```

## Conclusion

Es importanto hacerse familiar con uno o mas tipos de editores. Ha veces estaremos en un entorno donde no hay escritorios gráficos y es necesario usar una terminal de puro texto. En otras ocasiones, tales como en nuestro escritorio local, podremos usar un editor con interfaz gráfico que permiten el trabajo de desarrollo de código; en usos más complejos, estos editorios gráficos ofrecen la característica de poder conectarse remotamente a un sistema a travéz de SSH y trabajar en desarollo de código remotatemte, es decir con archivos y directorios que no estan presentes en nuestro sistema local.

## Referencias 

### Glosario De Comandos

Los comandos siguientes son usados frecuentemente en sesiones de Linux.

sudo
: da la abilidad de ejecutar comandos con permisos de super usuario

apt
: utilidad para instalar paquetes en Ubuntu

yum
: utilidad para instalar paquetes en RedHat

vim, nano, emacs
: empezar el editor que corresponde con el nombre del comando

### Referencias Utiles

DevEsp :: Linux
- https://docs.devesp.com/linux-en-espa%C3%B1ol/

Paginas Manuales
- [vim](https://manpages.ubuntu.com/manpages/focal/en/man1/vim.1.html)
- [nano](https://manpages.ubuntu.com/manpages/focal/en/man1/nano.1.html)

Haz click en el enlace para ir al sitio red del editor.
- [Vim](https://www.vim.org/)
- [Nano](https://www.nano-editor.org/)
- [Emacs](https://www.gnu.org/software/emacs//)
- [Notepad++](https://notepad-plus-plus.org/)
- [Sublime](https://www.sublimetext.com/)


[Return to main page]({{site.baseurl}}/).