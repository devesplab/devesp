---
layout: default
title: Etiquetas En Docker
permalink: /docker_tabs/
parent: Docker
has_children: false
has_toc: false
nav_order: 5
---

# Etiquetas de Docker
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
## Concepto de Etiquetas de Docker

Las etiquetas Docker son etiquetas que se utilizan para identificar versiones o variantes específicas de una imagen Docker. Permiten administrar e implementar fácilmente diferentes versiones de las imágenes. Al extraer o enviar imágenes a un registro Docker (como Docker Hub), las etiquetas ayudan a especificar la versión de la imagen con la que se desea trabajar.
Puntos clave sobre las etiquetas Docker:

## Etiqueta predeterminada

Si no se especifica una etiqueta, Docker utiliza la última versión por defecto. Sin embargo, confiar únicamente en la última versión puede ser arriesgado, ya que no siempre apunta a la versión más reciente o estable que se pretende utilizar.

Formato de etiqueta:

Las etiquetas se añaden al nombre de la imagen con dos puntos, de la siguiente manera:
```
<repositorio>:<etiqueta>
```

Ejemplo:
```
ubuntu:22.04
```

{: .important }
Si no se especifica una etiqueta, Docker utiliza la última versión por defecto

Para obtener la imagen con la última versión usamos la etiqueta `latest`.
```
ubuntu:latest
```

## Creación de etiquetas:

Al crear una imagen, se puede especificar una etiqueta con docker build usando la opción -t:
```
-> docker build -t devespApp:1.0 .
```

## Listado de etiquetas:

Para ver las etiquetas disponibles para una imagen en Docker Hub, visite la página del repositorio de la imagen o utilice la interfaz web de [Docker Hub](https://hub.docker.com/). Algunas herramientas CLI pueden ayudar a listar imágenes localmente.

Este comando usa la API de Docker para obtener todas las etiquetas conocidas en Docker Hub

Syntaxis:
```
  curl -s "https://hub.docker.com/v2/repositories/<imagen>/tags?page_size=100" \
    | grep -o '"name":"[^"]*"' | awk -F: '{print $2}' | tr -d '"'
```
Ejemplo para obtener las etiquetas de Ubuntu. <br>
Usamos `library/ubuntu` porque esa es la localidad desde donde se sirve la imagen.
```
  curl -s "https://hub.docker.com/v2/repositories/library/ubuntu/tags?page_size=100" \
   | grep -o '"name":"[^"]*"' | awk -F: '{print $2}' | tr -d '"'
```

{: .highlight }
En DevEsp escribimos el escrito [dockerTags](https://github.com/devesplab/docker-devesp/blob/main/dockerTags.sh) en BASH para obtener la lista de imagenes.

En DevEsp escribimos el escrito `dockerTags` en BASH. Para usarlo, clona el repository, da permiso ejecutable al escrito y ejecutalo -- ver las instrucciones en la descripción del escrito.
```
-> git clone https://github.com/devesplab/docker-devesp.git
-> cd docker-devesp
-> chmod +x dockerTags.sh
-> ./dockerTags.sh ubuntu
```

Para obtener una versión específica de una imagen:
- pasar el nomber de la imagen
- pasar la etiqueta de la imagen
```
-> docker pull devespApp:1.0
```

Puede agregar etiquetas a imágenes existentes mediante docker tag:
```
-> docker tag image_id myrepo/myapp:2.0
```

## Recomendaciones y Mejores Prácticas

Generalmente debieramos atenernos a hábitos conducibles a prácticas optimizadas. A continuacion listamos algunos punto generales que nos ayuden a organizar nuestro trabajo con Docker.

- Utilice etiquetas significativas y versionadas para la gestión de versiones.
- Evite usar la última versión en entornos de producción para evitar ambigüedades, ejemplo: `ubuntu:latest`.
- Mantenga una estrategia de etiquetado consistente para facilitar la gestión de imágenes.
- Actualizar imagenes con cierta regularidad para evitar que la imagen se vuelva obsoleta.
- Solo crear etiquetas cuando sea necesario; debemos evitar crear un numero excesivo de etiquetas.

## Referencias

Ver la página de [Trucos Técnicos de Docker](docker_4a_Cheatsheet.md)

Documento official de [etiqueta de imagen de Docker](https://docs.docker.com/reference/cli/docker/image/tag/)

[Return to main page]({{site.baseurl}}/).