---
layout: default
title: Provisionar Linux
permalink: /linux-provision/
parent: Linux
has_toc: false
nav_order: 2
---

## Provisonar Ambiente de Linux

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

Primero que nada debemos escoger el sabor de linux que queremos implementar. Esto depende de el área de trabajo y la complejidad de tareas que se harán.

{: .note }
Se debe tener en mente el ciclo de actualización y desmantelamiento de versiones.

En la discusión de los [conceptos](./linux_1_Conceptos.md) de Linux mencionamos que podemos escoger una versión apta para servidores u otra para usuarios regulares.

La sección de [referencias](#referencias) de esta página muestra una lista the varios sitios para descargar distribuciones completas de Linux.

La manera típica de obtener una distribución es ir al sitio del distribuidor y descargar la imagen en formato de ISO, la cual puede descargarse en mas de una manera.

La imagen **Ubuntu** puede obtenerse aquí:

```bash
https://ubuntu.com/download?utm_source=chatgpt.com
```
Ver la documentatción para [Instalar Ubuntu](https://documentation.ubuntu.com/desktop/en/latest/tutorial/install-ubuntu-desktop/?utm_source=chatgpt.com).

La imagen ISO de **RHEL9** puede encontrarse en esta URL.

```bash
https://developers.redhat.com/products/rhel/download
```

Las imágenes de **CentoS 7.9** se obtiene aqui:

```bash
https://vault.centos.org/7.9.2009/
https://vault.centos.org/7.9.2009/isos/x86_64/
```

{: .warning }
> CentOS ha sideo descontinuado. Se recomienda usar RedHat.<br>
> La fecha de final de vida de CentOS 7 fue Junio 30, 2024.<br>
> Ver https://endoflife.software/operating-systems/linux/centos

Es mas recomendable descargar [RedHat](https://developers.redhat.com/products/rhel/download?utm_source=chatgpt.com#downloadsbyrelease).

Los ISO están generalmente disponibles en varias localidades sincronizadas conocidos como "espejos" porque reflejan el mismo contenido. Podemos escoger un espejo que este cerca geográficamente a nuestra localidad para que el descargue sea más rápido; esto es importante en lugares donde la velocidad de internet no es óptima.

### Recursos de Computación Requeridos

Hoy día la vasta mayoría de hardware es the arquitectura AMD64, Intel 64, and 64-bit ARM.

La página de [CentOS](https://docs.centos.org/en-US/8-docs/standard-install/assembly_system-requirements-reference/) indica que los siguiente es necesario para instalar el sistema operativo:

* 10Gib de disco duro
* 768MiB de RAM usando USB/DVD/NFS
* 1.5GiB usando HTTP/HTTPS/FTP

Los requerimientos para [Ubuntu](https://help.ubuntu.com/community/Installation/SystemRequirements) son mas exigentes:

* 2 GHz dual core processor
* 4 GiB RAM 
* 25 GB (8.6 GB por lo menos) 
* VGA capaz de 1024x768 resolución de pantalla

Cuando se usa HTTP/HTTPS/FTP como método de instalación se require mas memoria porque el tráfico del internet que va y viene a través del cable es muy intenso.

El requerimiento de disco duro aumenta dependiendo de los paquetes escogidos para provisionar el sistema. Por ejemplo, instalar el sistema gráfico de GNOME consume mas disco duro, y también mas memoria física para correr normalmente.

### Entender Actualización y Seguridad

Diferentes sistemas operativos tienen distintos ciclos de actualización. Por ejemplo, el [ciclo de Fedora](https://docs.fedoraproject.org/en-US/releases/lifecycle/) es agresivo, ya que publica una nueva versión cada seis meses, lo que hace más difícil mantener los sistemas al día. Por otro lado, el [ciclo de Ubuntu](https://ubuntu.com/about/release-cycle) es más largo; la versión más estable se conoce como la versión de soporte a largo plazo (LTS, o Long Term Support).


{: .note }
Es importante entender el ciclo de vida de los diferentes sistems operativos.

## Decidir Método de Instalación

Todas las distribuciones de Linux tienen un sistema y opciones similares de instalación.

ISO
: usando le imagen ISO descargada de la internet

DVD
: usando un disco óptico en una unidad de DVD 

USB
: usando una tarjeta de memoria adjunta a un puerto de USB

Kickstart/NFS
: método usado por RedHat/CentOS/Fedora a través del cable de network

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