---
layout: default
title: Git
permalink: /git-en-español/
has_children: true
has_toc: false
nav_order: 2
---

[comment]: # (Adds topnav bar above the main image)
<div class="topnav">
 <a class="active" href="../index">Home</a>
 <a href="../about">About</a>
 <a href="../news">News</a>  
</div> 

# Git
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

# GIT :: Conceptos Fundamentales

**DESCRIPCION**

En esta leccion exponemos los conceptos fundamentales de Git:
- que es código fuente?
- que es control de versiones?
- El Rol de Git Con Código Fuente 

**DEPENDENCIAS**

ninguna

**REQUERIMIENTOS**

Instalar el cliente de Git en el sistema local.<br>
Usamos el cliente de Git >= 2.0

**ADVERTENCIA**

ninguna.

## Working Environment

En esta leccion usamos el sistema operativo Ubuntu.

## La Importancia de Control de Versiones Explicada

Tratemos de entender la importancia de Control de Versiones con un ejemplo práctico.

{: .note }
Control de Versiones se conoce como Version Control System (VCS) en Inglés.

Supongamos que tenemos una carpeta con archivos que mantienen información climática de cada año en forma de CVS. Juntamente tenemos código fuente para generar programas para hacer predicciones climáticas de años futuros. Luego supongamos que un equipo de seis metereólogos mantienen esa información que se comparte con la comunidad de metereologa a nivel internacional. 

Supongamos que estas personas tiene cada uno una copia en su laptop personal. O talvez para mejorar, toda esa información se mantiene en un servidor de datos en el cuarto de un edificio. 

Varias preguntas surgen que meritan respuesta:
1. Cómo coordinan los mantenedores la actualización de datos día a día?
2. Siendo que el clima es muy dinámico, cómo se agrega nuevos datos continuamente?
3. Que pasa si uno de los metereólogos pierde su laptop? Qué pasaría si un desastre, tales como un incendio, destruye el edificio donde se encuentra el servidor de datos?
4. Cómo se recuperan los datos?

Es posible que algun ingeniero tenga una copia vieja de los datos en su laptop local y algo pueda recuperarse. O talvez hay una copia en un servidor de respaldo en otro edificio. Pero hay un problema: que tan actualizada será esa copia? Qué diferencia de datos se habrá perdido desde la ultima copia que se hizo?

Aquí es donde entra el Control de Versiones. 

Para poder controlar la actualización de datos, los metereólogos pueden cada uno trabajar con una copia de los datos en su computadora portátil. Cada uno puede enviar los cambios de código por separado y luego combinar los datos de manera coherente. Esto puede lograrse porque cada metereólogo puede aplicar una versión a sus cambios y usar esa versión para rastrear los cambios que cada quien envia no importa cuanto tiempo haya pasado. Y si por alguna razón la copia en el servidor central es destruida, corrupta, o alterada, los datos pueden recuperarse usando el control de revisión establecida. 

## Entendiendo código fuente

Ahora bien, que es código fuente? Simplemente es la copia original y versionada de datos, programas, archivos, imagenes, etc, que puede considerarse como la fuente verdadera de datos que podemos considerar confiable. La entrada nueva de datos es controlada y revisada con mucho cuidado para asegurar que se adhiere a los estándares que la organización ha establecido. Aun más, cada nuevo cambio es fechado y acompañado por un comentario que indica el motivo de la actualización.

## El Rol de Git Con Código Fuente

En nuestro escenario ficticio de los metereólogos haciendo predicciones climáticas, el código fuente es la copia que se mantiene en el Control de Versiones. Ellos usarían esa copia como Código Base para actualizaciones futuras en vez de recrear datos desde cero y al hazar y sin fundamento. 

Asi que, El rol de Git con Código Fuente es proveer un método de organización de datos de manera metódica y confiable.

Git provee un cliente conocido apropiadamente como `git`. Este es un comando disponible en Linux que un desarrollador puede usar en su sistema local. Este comando se usa para confirmar y enviar cambios a un repositorio de Github.

## Ques es Clonar en Github?

Para poder enterder como podemos usar el Código Fuente de projectos en Github, es necesario entender el concepto de lo que es clonar en github.

En el mundo de desarrollo de software casi siempre se encuentran bugs, funciones que no trabajan como se esperaba y toda clase de anomalías que deben corregirse en forma de patches y arreglos de software. 

La accíon de clonar es básicamente obtener una copia idéntica de un codigo fuente con la intencíon de hacer ajustes y actualizaciones que pueden ser registradas y rastreadas en el futuro en el repositorio de github. 

Un repositorio de Github puede clonarse usando varios protocolos (git, https, ssh, file):
- git://github.com/devesplab/git-devesp.git
- https://github.com/devesplab/git-devesp.git
- ssh://user@github.com/devesplab/git-devesp.git
- file:///myProject/pets.git

Por regla general, usamos el protocolo HTTPS para obtener una copia usando el cliente de git de esta manera:
```
-> git clone https://github.com/devesplab/git-devesp.git
```

El comando anterior creará un directorio con el nombre del repositorio `git-devesp`. Podemos cambiar a ese directorio y empezar a trabajar.
```
-> cd git-devesp
```

## Conclusion

El Control de Versiones es extermadamente importante para rastrear cambios ne Código Fuente. Al tener un repositorio de datos nos aseguramos que podemos recuperar la información en caso de un desastre. 

Un group de desarrolladoers pueded trabajar en tándem en projectos complejos y combinar acutalizaciones de datos y programas.

## Referencias 

### Glosario De Comandos

Los comandos siguientes son usados frecuentemente en sesiones de Linux.

git
: cliente de github para control de revisiones

### Referencias Utiles

DevEsp :: Linux
- Código fuente de [linux-devesp](https://github.com/devesplab/linux-devesp)

Recursos de Git
- [Documentación en línea de Git](https://git-scm.com/)
- [Libro de Git en Español](https://git-scm.com/book/es/v2)

[Return to main page]({{site.baseurl}}/).