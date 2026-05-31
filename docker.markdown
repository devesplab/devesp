---
layout: page
title: Docker
permalink: /docker-en-español/
nav_order: 5
has_children: true
has_toc: false
---

<div class="topnav">
 <a class="active" href="../index">Home</a>
 <a href="../about">About</a>
</div>

# Docker

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

## Docker como herramienta

En esta sección hablaremos the Docker.

Docker es una plataforma de código abierto que se usa para gestionar la automatización
de aplicaciones en un ambiente de desarrollo virtual en un
 de contenedores ligeros y portátiles.

{: .highlight }
Docker no es la única herramienta para crear contenedores. Hay otras tales como Podman, LXC, containerd, etc.

Un contenedor es una implementación usada para empaquetar una unidad de código y las dependencias requeridas para que funcione sin problemas, de manera confiable y que se pueda repetir de la misma manera cada vez.

Docker usa el modelo de servidor y cliente, es decir un demonio corre en el sistema y los clientes se comunican con el para poder crear contenedores..

> El objectivo de esta sección es enternder la manera de usar esta herramienta para crear un ambiente para probar varios aspectos de Docker.

## Propósito General de uso de Docker

**Aislar** un aplicación es una de las razones principales para usar docker. Esto permite proveer un ambiente contenedorizado para correr una aplicación de forma separada con respecto al sistema en el que de el demonio de docker esta corriendo.

Otra razón puede ser proveer un **ambiente de desarrollo versátil** en el que se puede hacer las mutaciones necesarias para fabricar la aplicación. A veces es necesario empaquetar una aplicación con librerías específicas y ajustes muy particulares.

**Escalabilidad** es quizá una de las razones principales para usar docker. Frecuentemente se requiere correr varias instancias de una aplicación simultáneamente. Esto solo puede lograrse con tecnologías tales como **Docker Compose** que Docker provee.

La facilidad de implementar **actualizaciones incrementales** es otra buena razón para usar contenedores. Podemos hacer cambios menores y empujar esos cambios en incrementos pequeños que pueden probarse y asegurarnos que todo trabaja bien antes de un major lanzamiento para uso general.

## Mejores prácticas para usar Docker

Hay ciertas practicas generales que podemos tomar en consideración para usar docker de manera óptima.

- Tratemos de usar imagenes de docker oficiales o generalmente mantenidas y actualizadas regularmente
- El aspecto de **seguridad** es sumamente importante; debemos evitar la introducción de vulnerabilidades y acceso inapropiado de usuarios, servicios, demonios y otros aspectos de acceso en un ambiente de computación seguro.
- no empaquetar componentes innecesarios tales como librerías, ejecutables, binarios u otros componentes que no contribuyen a la implementación optima del contenedor
- evitar codificar variables o valores directamente en la imagen, en vez deberíamos usar variables de entorno que retornan valores dinámicamente
- Usar **volúmenes** para persistir datos en vez de usar información dentro del contenedor que puede perderse en caso de corrupción de datos o la necesidad de restablecer el ambiente de la aplicacíon.
- Siendo que los recursos de fuente abierta cambian frecuentemente, es buena practica actualizar imágenes de docker con regularidad. Debe tomarse en cuenta cuestiones de compatibilidad, y la implementación de herramientas actualizadas.

## Limitaciones del uso de Docker

Los contenedores creados con una imagen de docker son ligeros, pero a cierto punto, cuando intensificamos su uso, produce un sobrecargo de rendimiento. Debemos tener en cuenta la _cantidad de recursos_ disponibles en el ambiente en el cual la aplicación corre.

{: .important }
Docker esta limitado por la _cantidad de recursos_ disponibles en el ambiente en el cual la aplicación corre.

Es decir, tenemos que tomar en cuenta lo siguiente:

- el número de núcleos de CPU
- la cantidad y tipo de memoria
- el ancho de banda de la red
- la cantidad y tipo de disco de almacenamiento (Disco Duro, SSD, NAS, etc)

Asi que, el tipo y cantidad de recursos pone de manifiesto las limitaciones y dificultades que pueden presentarse en una implementación de un ambiente de contenedores. De hecho, esto indica el **máximo numero** de contenedores que podemos correr simultáneamente.

## En Resumen

Hay una cierta curva de aprendizaje para implementar Docker, pero es un esfuerzo bien gastado.<br>
Podemos desarrollar aplicaciones con rapidez y eficiencia.<br>
La implementación de buenas practicas garantizan que nuestras aplicaciones seran eficientes y seguras.<br>
Al conocer las limitaciones de recursos podemos desarrollar la aplicación de manera optima.<br>

[Return to main page]({{site.baseurl}}/).
