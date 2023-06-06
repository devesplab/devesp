---
layout: default
title: Provision
permalink: /linux-provision/
parent: Linux
has_toc: false
nav_order: 2
---

# Provisonar Ambiente de Linux
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
## Escoger y Obtener una Distribución

Primero que nada debemos escoger el sabor de linux que queremos implementar.

{: .warning }
Se debe tener en mente el ciclo de actualización y desmantelamiento de versiones.

En la discussión de los [conceptos](../linux_conceptos) de Linux mencionamos que podemos escoger una version apta para servidores u otra para usuarios regulares. 

La sección de [referencias](#referencias) muestra una lista the varios sitios para descargar distribuciones completas de Linux.

La manera típica de obtener una distribución es ir al sitio del distribuidor y descargar 
la imagen en formato de ISO.

Podemos descargar el ISO en mas de una manera.
Por ejemplo, la imagen de _CentOS 8 Stream_.
* directamente del sitio de internet usando el URL en el navegador web.
```
https://mirrors.edge.kernel.org/centos/8-stream/isos/x86_64/CentOS-Stream-8-20230509.0-x86_64-dvd1.iso
```
* directamente en la linea de comandos en un terminal usando el el comando `wget`
```
wget https://mirrors.edge.kernel.org/centos/8-stream/isos/x86_64/CentOS-Stream-8-20230509.0-x86_64-dvd1.iso
```

Los ISO estan generalmente disponibles en varias localidades sincronizadas conocidos como "espejos" porque reflejan el mismo contenido. Podemos escoger un espejo que este cerca geograficamente a nuestra localidad para que el descargue sea más rápido; esto es importante en lugares donde la velocidad de internet no es óptima.

### Recursos de Computación Requeridos

Hoy día la vasta mayoría de hardware es the arquictectura AMD64, Intel 64, and 64-bit ARM.

La página de [CentOS](https://docs.centos.org/en-US/8-docs/standard-install/assembly_system-requirements-reference/) indica que los siguiente es necesario para instalar el sistema operativo:

* 10Gib de disco duro
* 768MiB de RAM usando USB/DVD/NFS
* 1.5GiB usando HTTP/HTTPS/FTP

Los requerimientos para [Ubuntu](https://help.ubuntu.com/community/Installation/SystemRequirements) son mas exigentes:

* 2 GHz dual core processor
* 4 GiB RAM 
* 25 GB (8.6 GB por lo menos) 
* VGA capaz de 1024x768 resolución de pantalla

Cuando se usa HTTP/HTTPS/FTP como método de instalación se require mas memoria porque el tráfico del internet lledo y vieniendo a travéz del cable es muy intenso.

El requerimiento de disco duro aumenta dependiendo de los paquetes escogidos para provisionar el sistema. Por ejemplo, instalar el sistema gráfico de GNOME consume mas disco duro, y tambiem mas memoria fisica para correr normalmente.

### Entender Actualización y Seguridad

Fedora tiene un [ciclo](https://docs.fedoraproject.org/en-US/releases/lifecycle/) se actualizacion agresivo de cada seis meses lo que hace muy difícil mantener los sistemas al dia. De otro lado, Ubuntu tiene un [ciclo](https://ubuntu.com/about/release-cycle) mas largo; la version  mas estable se conoce come la version de lanzamiento a largo plazo (LTS, o Long Term Support).

{: .note }
Es importante entender el ciclo de vida de los diferentes sistems operativos.

## Decidir Método de Instalación

Todas las distribuciones de Linux tienen un sistema y opciones similares de instalación.

ISO
: usando le imagen ISO descargarda de la internet

DVD
: usando un disco óptico en una unidad de DVD 

USB
: usando una targeta de memoria adjunta a un puerto de USB

Kickstart/NFS
: método usado por RedHat/CentOS/Fedora a travéz del cable de network

Clonar
: copiar un sistema a otro con la misma configuración de hardware

## Donde Obtener Linux

Linux puede descargarse de cualquier espejo disponible.
Varios sabores de linux estan disponibles en [kernel.org](https://mirrors.edge.kernel.org/)
Algunos sabores de Linux de fuente abierta tales come CentOS [^2], Fedora [^3], Ubuntu [^4], Debian [^5], Gentoo [^6], están disponibles gratis en varias versiones.
Otras versiones tales como RedHat [^7], requieren un licensia de usuario.


## [](referencias)Referencias

[^1]: [Lista de distrubuciones de Linux](https://en.wikipedia.org/wiki/List_of_Linux_distributions)

[^2]: [Descargar CentOS](https://mirrors.edge.kernel.org/centos/)

[^3]: [Descargar Fedora](https://fedoraproject.org/)

[^4]: [Descargar Ubuntu](https://launchpad.net/ubuntu/+cdmirrors)

[^5]: [Descargar Debian](https://www.debian.org/CD/http-ftp/)

[^6]: [Descargar Gentoo](https://www.gentoo.org/downloads/)

[^7]: [Descargar RedHat](https://access.redhat.com/downloads/)

[Return to main page]({{site.baseurl}}/).