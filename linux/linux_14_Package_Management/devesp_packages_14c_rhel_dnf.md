---
layout: default
title: Usando DNF en RedHat
permalink: /rhel-dnf/
parent: Manejando Paquetes
grand_parent: Linux
has_children: false
has_toc: false
nav_order: 2
---

# Manejo de Paquetes en RedHat con DNF
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

## Configuración de DNF

La utilidad DNF (Dandified YUM) se usa para administrar paquetes para el sistema operativo de RedHat y Fedora.

El archivo de configuración de DNF es `/etc/dnf/dnf.conf`.

```bash
devuser@rhel9-1-devesp
~
hist:54 -> cat /etc/dnf/dnf.conf
[main]
gpgcheck=1
installonly_limit=3
clean_requirements_on_remove=True
best=True
skip_if_unavailable=False
```

Las ajustes predeterminados que se muestran arriba indican lo siguiente:

gpgcheck=1
: Opción para la verificación de firmas GPG en los paquetes. Requiere que todos los paquetes tengan una firma GPG válida para garantizar su autenticidad e integridad. Un valor de 1 significa que la verificación GPG está habilitada; 0 la deshabilitaría.

installonly_limit=3
: Opción que establece la cantidad de paquetes que se pueden instalar con el indicador "installonly". Asiste en preservar solo has el número de versiones anteriores de un paquete, por ejemplo el Linux Kernel.

clean_requirements_on_remove=True
: Cuando esta opción está establecida en True, DNF eliminará automáticamente todas las dependencias (requisitos) que ya no sean necesarias cuando se elimine un paquete. Esto ayuda a mantener el sistema limpio y libre de paquetes huérfanos.

best=True
: Opción que indica que DNF siempre debe intentar instalar la mejor versión disponible de un paquete. 

skip_if_unavailable=False
: Cuando esta opción está establecida en `False`, DNF fallará si intenta instalar un paquete que no está disponible. 

Las opciones no deberían cambiarse a menos que se necesario. En particular `gpgcheck=1` debe manterse asi para verificar la autenticidad de paquetes.

Los repositorios que DNF usa para manejar paquetes estan en `/etc/yum.repos.d/*.repo`

```bash
devuser@rhel9-1-devesp
~
hist:55 -> ls -l /etc/yum.repos.d/*.repo
-rw-r--r-- 1 root root 1142 Aug 17  2023 /etc/yum.repos.d/epel-cisco-openh264.repo
-rw-r--r-- 1 root root 1552 Aug 17  2023 /etc/yum.repos.d/epel-testing.repo
-rw-r--r-- 1 root root 1453 Aug 17  2023 /etc/yum.repos.d/epel.repo
-rw-r--r-- 1 root root  358 Jul  4  2023 /etc/yum.repos.d/redhat.repo
-rw-r--r-- 1 root root 2736 Jun 15  2023 /etc/yum.repos.d/ubi.repo
```

{: .note }
La utilidad YUM y RPM tambien usan los repositorios `/etc/yum.repos.d/*.repo`.

Veamos la configuración de DNF

```bash
-> dnf config-manager --dump
```

## Buscando Paquetes con DNF

Para buscar un paquete, utilice:

```bash
->  dnf search término
```

Reemplace "término" con un palabra relacionado con el paquete.
Este comando es util cuando no sabemos con exactitud el nombre del paquete, pero sabemos algo acerca de la descripción del mismo. Asi que el comando anterior busca el nombre y la descripción del paquete.

```bash
-> dnf search --all término
```

Reemplace "término" con un término que desee buscar en el nombre, resumen o descripción de un paquete.

{: .note }
El comando `dnf search --all` permite una búsqueda más completa y detallada pero es más despacio.

Para buscar un paquete y su versión o un archivo:

```bash
-> dnf repoquery <nombre-de-paquete>
```

Para buscar un paquete, binario o archivo:

```bash
-> dnf provides <nombre-de-paquete>

-> dnf provides "*/<nombre-de-paquete>"
```

Reemplace "<nombre-de-paquete>" con el nombre de un paquete, binario o archivo que desee buscar.

Mostrar información sobre todos los paquetes instalados y disponibles:

```bash
-> dnf list --all
```

Obtener una lista de todos los paquetes instalados.

```bash
-> dnf list --installed

-> dnf repoquery --installed
```

Obtener una lista de todos los paquetes en todos los repositorios habilitados que podemos instalar.

```bash
-> dnf list --available

-> dnf repoquery
```

## Listar Repositorios con DNF

Es muy útil saber los repositorios que tenemos configurados en el sistema porque asi sabremos si algun paquete que necesitamos esta disponible o no.

Listar todos los repositorios habilitados.

```bash
-> dnf repolist
```

Listar todos los repositorios deshabilitados.

```bash
-> dnf repolist --disabled
```

Listar los repositorios habilitados y los deshabilitados.

```bash
-> dnf repolist --all
```

Listar información sobre los repositorios.

```bash
-> dnf repoinfo
```

## Mostrar Información de Paquetes con DNF

Los comandos siguientes muestran información sobre los paquetes disponibles en el sistema.

Para mostrar información sobre uno o más paquetes disponibles, utilice:
```bash
-> dnf info <nombre-de-paquete>

-> dnf repoquery --info <nombre-de-paquete>
```
Para mostrar información sobre uno o más paquetes instalados en su sistema, utilice:

```bash
-> dnf repoquery --info --installed <nombre-de-paquete>
```

## Referencias 

Manuales en linea
- [Manejo de Paquetes en RedHat](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/4/html/system_administration_guide/package_management)
- [Manejando Paquetes con DNF](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_software_with_the_dnf_tool/index)

### Referencias Utiles

Man Pages
- [dnf](https://www.man7.org/linux/man-pages/man8/dnf.8.html)

[Return to main page]({{site.baseurl}}/).
