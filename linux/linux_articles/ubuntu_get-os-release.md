---
layout: default
title: Obtener Version De Ubuntu OS
permalink: /get-ubuntu-version/
parent: Artículos De Linux
has_children: false
has_toc: false
nav_order: 2
---

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

# Identificar la versión de Sistema Operativo de Ubuntu

**DESCRIPCION**

En esta lección: 
- aprender a identificar la versión de Sistema Operativo de Ubuntu.

**DEPENDENCIAS**

ninguna

**REQUERIMIENTOS**

Ubuntu Linux system. <br>

**ADVERTENCIA**

ninguna.

## Ambiente de trabajo

En esta leccion usamos el sistema operativo Ubuntu.
```
-> lsb_release -a
Distributor ID:	Ubuntu
Description:	Ubuntu 24.04.4 LTS
Release:	24.04
Codename:	noble
```

## Pasos en resumen para Obtener la versión de Sistema Operativo de Ubuntu

```
-> cat /etc/lsb-release

-> hostnamectl

-> lsb_release -a

-> ls -l /etc/os-release /usr/lib/os-release

-> cat /etc/os-release
```

## Obtener la versión de Ubuntu OS

La versión de Ubuntu OS se puede obtener fácilmente mostrando el contenido del archivo `/etc/os-release`:
```
devuser@devesp
~
hist:188 -> cat /etc/lsb-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=24.04
DISTRIB_CODENAME=noble
DISTRIB_DESCRIPTION="Ubuntu 24.04.4 LTS"
```

o con el comando `hostnamectl`:
```
-> hostnamectl
 Static hostname: client1
       Icon name: computer-container
         Chassis: container ☐
      Machine ID: 033f5cb1bf2c4e3fa43822fc3fb6c41a
         Boot ID: 6944a0570b704951b237822e1dc85040
  Virtualization: docker
Operating System: Ubuntu 24.04.4 LTS
          Kernel: Linux 6.3.13-linuxkit
    Architecture: x86-64
```

también podemos usar el comando `lsb_release`:
```
-> lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 24.04.4 LTS
Release:	24.04
Codename:	noble
```

En la salida del comando arriba, vemos que dice: `No LSB modules are available`<br>
La siguiente sección explica ese mensaje.


El comando `lsb_release` utiliza la información almacenada en el archivo `/etc/os-release`, que es un enlace a `/usr/lib/os-release`.
```
-> ls -l /etc/os-release /usr/lib/os-release
lrwxrwxrwx 1 root root  21 Feb  6 07:23 /etc/os-release -> ../usr/lib/os-release
-rw-r--r-- 1 root root 400 Feb  6 07:23 /usr/lib/os-release
```
Aquí está el contenido de ese archivo.
```
-> cat /etc/os-release
PRETTY_NAME="Ubuntu 24.04.4 LTS"
NAME="Ubuntu"
VERSION_ID="24.04"
VERSION="24.04.4 LTS (Noble Numbat)"
VERSION_CODENAME=noble
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=noble
LOGO=ubuntu-logo
```

## Ubuntu dice "No hay módulos LSB disponibles"

Cuando `lsb_release -a` informa que los módulos opcionales de compatibilidad LSB (Linux Standard Base) no están instalados, no significa que haya un problema. 

En los sistemas Ubuntu modernos, especialmente en versiones más recientes, esta frase es normal:
```
"No LSB modules are available" --> "No hay módulos LSB disponibles"
```

Si quieres, puedes verificar el paquete que provee los modulos en questión.
```
dpkg -l | grep lsb
```
Probablemente verás algo como:
```
ii  lsb-release
```
Pero no los paquetes antiguos como `lsb-core` o `lsb-base` porque ahora están mayormente obsoletos.

## Conclusion

Conocer la versión del sistema operativo es importante porque te ayudará a identificar aspectos importantes como versiones de paquetes, opciones de configuración de Kernel, optimizacion a nivel de aplicación, etc.

## Referencias 

### Glosario De Comands

Los siguientes comandos se usan frecuentemente en sesiones de Linux..

lsb_release
: imprimir información específica de la distribución de Ubuntu (implementación mínima).

hostnamectl
: Controla el nombre de host del sistema

### Useful References

`/usr/lib/os-release`
: Archivo proporcionado por distribución con datos de identificación del sistema operativo.

`/etc/os-release`
: Archivo específico de la máquina con datos de identificación del sistema operativo. Si está presente, /etc/os-release se lee en lugar de /usr/lib/os-release.

Paginas Manuales
- [lsb_release](https://manpages.ubuntu.com/manpages/stonking/man1/lsb_release.1.html)
- [hostnamectl](https://manpages.ubuntu.com/manpages/stonking/man1/hostnamectl.1.html)
