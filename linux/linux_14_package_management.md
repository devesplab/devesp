---
layout: default
title: Manejando Paquetes
permalink: /manejar-paquetes-linux/
parent: Linux
has_children: true
has_toc: false
nav_order: 14
---

# Introducción a Manejo De Paquetes
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

<style>.note-custom {
    background-color: #a4dded;
    color: #23297a;
    border: 2px solid black;
    margin-left: 2%;
    margin: 40px;
    padding: 10px;
    text-align: left;
}
</style>

Es importante entender como mantener el sistema operativo en Linux desde el punto de vista de eficiencia y seguridad. 

La razónes a considerar son a seguir

1. **seguridad:** un paquete viejo puede tener vulnerabilidades que permita entar ilegalmente al sistema; el lanzamiento de una nueva version puede contener parches para arreglar las vulnerabilidades
2. **actualizar funcionalidad:** nuevas versiones de paquetes tiene caraterísticas actualizadas que mejoran la experiencia del usuario
3. **compatibilidad:** nuevas versiones de paquetes tienen arreglos que pueded apoyar nuevo hardware, por ejemplo tarjetas de videos
4. **cumplimiento normativo:** el departemento de seguridad de una empresa on agencia guvernamental puede exigir el cumplimiento de actualizaciones estrictas para reducir la posibilidad de exponerse a casos acceso ilegal.

Para lograr lo anterior, cada sistema operativo provee una utilidad para manejar paquetes.

## Utilidades Para Manejar Paquetes en Linux

Cada sistema operativo tiene su utilidad para manejar paquetes. Sin importar el sistema operativo, todas las utilidades tienen los siguientes usos generales:
- **instalar:** consiste de provisionar o instalar paquetes que no estan disponibles en el sistema
- **borrar:** consiste de borrar o desinstalar paquetes que ya estan instalados en el sistema
- **actualizar:** consiste actualizar paquetes que ya estan instalados en el sistema 

<div class="note-custom">
    <h3> Utilidad por sistema operativo</h3>
    <ul> APT    : usada por Debian, Ubuntu</ul>
    <ul> DNF    : usada por RedHat, Fedora, CentOS (reemplaza a YUM)</ul>
    <ul> YUM    : usada por RedHat 7 y CentOS 7 </ul>
    <ul> BREW   : usada por MacOS</ul>
    <ul> WINGET : usada por Microsoft</ul>
</div>

## Operaciones Generales de Paquetes en Linux

Las utilidades para manejar paquetes proveen varios comandos que nos permiten hacer lo siguiente:
- listar repositorios
- activar y desactivar repositorios
- buscar paquetes
- listar paquetes
- instalar y desinstalar o reinstalar paquetes
- mostrar informacion acerca de un paquetes
- usar in interfaz si esta disponible

## Formato de Paquetes

Cada sabor de sistema operativo tiene sus propias convenciones y formatos para manejar paquetes..

.deb
: Es el formato de usado for el sistema operativo Debian y sus derivativas tales como Ubuntu

.rpm
: Es el formato de usado for el sistema operativo RedHat (RHEL) y sus derivativas tales como Fedora y CentOS.

## Repositorios de Paquetes

Tanto RedHat como Debian mantienen repositorios en la red que sirven comon localidad curada para ofrecer los paquetes que soportan el lanzamiento del sistem operativo. Generalmente existe los repositorios pricipales mantenidos for la distribución oficial, pero tambien hay repositorios de terceros mantenidos por la comunidad.

## Dependencias de Paquetes

En un sistem de Linux podemos instalar paquetes de dos maneras.

1. usar las utilidades para manejar paquetes discutidas en esta página
2. compilar el paquete usando código fuente

La manera óptima de instalar paquetes es usando el método (1) puesto que las utilidades como APT o DNF se encargan de resolver las dependencias requeridas, por ejemplo librerias (.so), u otros paquetes que siguen en cadena.

También es posible construir manualmente un paquete usando el método (2), pero eso requiere conocimiente detallado de las dependencias requeridas. Usualmente esta opción se reserva para situaciones en las que la versión de un paquete presente en el repositorio de distribución no cumple con los requerimientos deseado. Esta opción debe usarse infrecuentemente.

## Versiones y Seguridad

En la medida que sea posible debemos esforzarnos para mantener el sistema actualizado con las versiones más recientes disponibles de paquetes. 

Las versiones viejas de paquetes son vulnerables y puede ser el vehiculo que un mal actor use para entrar ilegalmente a nuestro sistema. Un ejemplo es OpenSSL que apoya el protocolo de HTTPS para acceso seguro de sitios de red. Otro ejemplo es SSH que se usa para acceso remoto de sistemas.

Cada sistema operativo tiene sus convenciones para identificar sus paquetes con nombre y número.

**:: RedHat**

```
nombre-version-lanzamiento.arquitectura
```

nombre
: el nombre del paquete

version
: la version del paquete

lanzamiento
: El número de lanzamiento del paquete. Se incrementa cuando hay cambios o actualizaciones.

arquitectura
: arquitectura a cambiar (e.g., x86_64, noarch, etc.)

**:: Debian (Ubuntu)**

```
version_arriba-debian_revision
```

version_arriba
: la version del paquete que viene del recurso de lanzamiento

revision-debian
: La revisión específica a Debian. Se incrementa cuando hay cambios o actualizaciones.

## Referencias 

- [Guia de Empaquetar de RPM](https://rpm-packaging-guide.github.io/)

### Referencias Utiles

- [Base Estándard de Linux (LSB)](https://github.com/LinuxStandardBase/lsb)

[Return to main page]({{site.baseurl}}/).