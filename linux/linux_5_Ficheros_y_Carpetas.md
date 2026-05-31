---
layout: default
title: Ficheros y Carpetas
permalink: /ficheros_carpetas/
parent: Linux
has_children: true
has_toc: false
nav_order: 5
---

## Ficheros y Carpetas

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

## Que es un Fichero?

En el sistema operativo Linux, un archivo es una colección de datos que se almacena en un dispositivo de almacenamiento, como un disco duro o SSD. Los archivos pueden ser documentos de texto, imágenes, vídeos, programas o cualquier otro tipo de datos.

Los archivos en Linux son necesarios para almacenar y organizar datos, programas, scripts, configuraciones y otra información importante. El sistema operativo utiliza los archivos para realizar un seguimiento de las configuraciones del sistema, los datos del usuario y los programas, y hacerlos accesibles para los usuarios y las aplicaciones. Los archivos son esenciales para el funcionamiento del sistema operativo, ya que sirven como elementos básicos para almacenar y manipular datos. Sin archivos, el sistema operativo no podría realizar tareas y los usuarios no podrían almacenar ni acceder a sus datos.

## Que es una Carpeta?

En Linux, los archivos están organizados en una estructura de directorios jerárquica, y cada archivo tiene un nombre único que permite que el sistema y los usuarios accedan a él y lo manipulen. Los archivos se pueden crear, editar, copiar, mover y eliminar utilizando varios comandos y aplicaciones en Linux.

Los directorios en Linux son importantes para organizar y administrar archivos y carpetas. Proporcionan una estructura jerárquica que ayuda a los usuarios a navegar fácilmente a través de diferentes directorios y acceder a archivos específicos. Los directorios también ayudan a mantener un sistema de archivos limpio y estructurado, lo que facilita la localización y administración de archivos. Además, los directorios ayudan a configurar permisos y controles de acceso para diferentes usuarios, garantizando la seguridad y privacidad de archivos y datos.

## Listado de Ejemplo de Archivos y Carpetas

Este ejemplo muestra el listado del comando `ls -l` de una carpeta de OpenJDK.

Nótese que el carácter inicial de la linea indica el tipo de activo:

- la letra `d` indica que es una carpeta
- el símbolo de guión `-` indica que es un archivo
- la letra `l` indica que es un enlace a otro archivo (or carpeta on otros casos)

![Ejemplo de listado de archivos y carpetas con el comando ls -l que muestra tipos de archivo y permisos](../../assets/images/archivos-y-carpetas_v1.png)

Las carpetas también pueden identificarse por la barra de terminación `/`.

## Propósito De Directorios y Ficheros

Cuando ha pasado tiempo que hemos trabajado en un proyecto, acumulamos mucha información de tópicos diferentes. A un cierto punto es imperativo agrupar información relacionada para que la podamos manejar mas fácil.

Luego tenemos que:

- Un fichero contiene información de un tema en particular.
- Un directorio nos ayuda a agrupar ficheros relacionados a un tema.

Los ficheros pueden ser de varios tipos: texto, binario, pdf, zip, jpeg, etc. Este tipo de clasificación no se aplica a directorios.

Eventualmente la agrupación de datos va más allá del uso individual y puede convertirse en un esfuerzo de un grupo de desarrolladores. Cuando el volumen de información crece en complejidad hay la necesitar de crear un sistema de version y registrar los cambios pasado el tiempo.

[Return to main page]({{site.baseurl}}/).