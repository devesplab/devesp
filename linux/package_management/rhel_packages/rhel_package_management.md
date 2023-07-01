---
layout: default
title: Paquetes en RedHat
permalink: /rhel-package-management/
parent: Manejando Paquetes
grand_parent: Linux
has_children: false
has_toc: false
nav_order: 98
---

# Manejo de Paquetes en RedHat (RHEL)
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


# YUM en RHEL/CentOS

RedHat y sus variantes tales como CentOS y Fedora usan el comando `yum` (tambien `dnf` ) para manejar paquetes de software.

La sintaxis general the YUM es asi:
```bash
yum [options] COMMAND
```
## Operaciones Generales con YUM

En seguida veremos varios comandos que nos permiten hacer lo siguiente:
- listar repositorios
- activar y desactivar repositorios
- buscar paquetes
- listar paquetes
- instalar y desinstalar paquetes
- mostrar informacion
- usar el shell de yum

### Listar Repositorios

Cada repositorio en CentOS/RHEL tiene un fichero correspondiente en `/etc/yum.repos.d/`.
```bash
root@client1 [ CentOS8 ]
~
hist:305 -> ls -l /etc/yum.repos.d/
total 64
-rw-r--r-- 1 root root  721 Jun 18 18:28 CentOS-Linux-AppStream.repo
-rw-r--r-- 1 root root  706 Jun 18 18:28 CentOS-Linux-BaseOS.repo
-rw-r--r-- 1 root root 1132 Jun 18 18:28 CentOS-Linux-ContinuousRelease.repo
-rw-r--r-- 1 root root  318 Sep 14  2021 CentOS-Linux-Debuginfo.repo
-rw-r--r-- 1 root root  734 Jun 18 18:28 CentOS-Linux-Devel.repo
-rw-r--r-- 1 root root  706 Jun 18 18:28 CentOS-Linux-Extras.repo
-rw-r--r-- 1 root root  721 Jun 18 18:28 CentOS-Linux-FastTrack.repo
-rw-r--r-- 1 root root  742 Jun 18 18:28 CentOS-Linux-HighAvailability.repo
-rw-r--r-- 1 root root  693 Sep 14  2021 CentOS-Linux-Media.repo
-rw-r--r-- 1 root root  708 Jun 18 18:28 CentOS-Linux-Plus.repo
-rw-r--r-- 1 root root  726 Jun 18 19:15 CentOS-Linux-PowerTools.repo
-rw-r--r-- 1 root root 1124 Sep 14  2021 CentOS-Linux-Sources.repo
-rw-r--r-- 1 root root 1680 Apr 17 06:22 epel-modular.repo
-rw-r--r-- 1 root root 1779 Apr 17 06:22 epel-testing-modular.repo
-rw-r--r-- 1 root root 1431 Apr 17 06:22 epel-testing.repo
-rw-r--r-- 1 root root 1332 Apr 17 06:22 epel.repo
```

Un repositorio esta activo cuando tiene `enabled=1`.<br>
Si tiene `enabled=0`, indica que esta desabilitado.<br>
En este ejemplo, el repositorio **powertools** esta activo porque tiene `enabled` igual a `1`.
```bash
root@client1 [ CentOS8 ]
~
hist:307 -> cat /etc/yum.repos.d/CentOS-Linux-PowerTools.repo
# CentOS-Linux-PowerTools.repo
[powertools]
name=CentOS Linux $releasever - PowerTools
baseurl=http://vault.centos.org/$contentdir/$releasever/PowerTools/$basearch/os/
gpgcheck=1
enabled=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-centosofficial
```

El siguiente comando muestra como listar todos los repositorios disponibles en el sistema.
```bash
root@client1 [ CentOS8 ]
~
hist:254 -> yum repolist
repo id                                repo name
appstream                              CentOS Linux 8 - AppStream
baseos                                 CentOS Linux 8 - BaseOS
epel                                   Extra Packages for Enterprise Linux 8 - x86_64
extras                                 CentOS Linux 8 - Extras
powertools                             CentOS Linux 8 - PowerTools
```

### Cual paquete nos da una utilidad?

Por ejemplo, esto nos dice que el paquete at-3.1.20-11.el8.x86_64 provee la utilidad `at`
```bash
root@client1 [ CentOS8 ]
~
hist:270 -> yum provides at
Last metadata expiration check: 0:10:46 ago on Sun Jun 25 13:41:21 2023.
at-3.1.20-11.el8.x86_64 : Job spooling tools
Repo        : baseos
Matched from:
Provide    : at = 3.1.20-11.el8
```

### Listar paquetes

Podemos listar un solo paquete.<br>
En este ejemplo listamos el paquete `at`.
```bash
root@client1 [ CentOS8 ]
~
hist:270 ->  yum list at
Last metadata expiration check: 0:10:04 ago on Sun Jun 25 13:41:21 2023.
Available Packages
at.x86_64                                                     3.1.20-11.el8                                                     baseos
```

Esto muestra la lista completa de todos los paquetes disponibles de todos los repositorios presentes 
en el sistema.
```bash
-> yum list --available
```
La lista puede ser bastante larga, asi que podemos restringir la busqueda a lo que nos interesa.<br>
Por ejemplo, aqui buscamos todos los paquetes referentes a `mysql`.
```bash
-> yum list --available | grep mysql
``` 

### Instalar un Paquete

Podemos usar el parametro `install` y pasar el nombre del paquete.<br>
La operacion encontrara las dependencias disponibles si existen.<br>
En este ejemplo instalamos el paquete `at`.
```bash
root@client1 [ CentOS8 ]
~
hist:268 -> yum install at
Last metadata expiration check: 0:08:46 ago on Sun Jun 25 13:41:21 2023.
Dependencies resolved.
=====================================================================================
 Package                    Architecture    Version           Repository        Size
=====================================================================================
Installing:
 at                         x86_64          3.1.20-11.el8     baseos            81 k

Transaction Summary
=====================================================================================
Install  1 Package

Total download size: 81 k
Installed size: 129 k
Is this ok [y/N]: y
Downloading Packages:
at-3.1.20-11.el8.x86_64.rpm                               196 kB/s |  81 kB     00:00
-------------------------------------------------------------------------------------
Total                                                     195 kB/s |  81 kB     00:00
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                             1/1
  Installing       : at-3.1.20-11.el8.x86_64                                     1/1
warning: Unable to get systemd shutdown inhibition lock: Unit systemd-logind.service is masked.

  Running scriptlet: at-3.1.20-11.el8.x86_64                                     1/1
  Verifying        : at-3.1.20-11.el8.x86_64                                     1/1

Installed:
  at-3.1.20-11.el8.x86_64

Complete!
```

Para evitar tener que teclear la respuesta a la pregunta `Is this ok [y/N]:`, podemos pasar la opción `-y`.
```bash
-> yum -y install at
```

### Desinstalar un Paquete

Para desinstalar in paquete solo tenemos que usar el parametro `remove` y pasar el nombre del paquete.<br>
En este ejemplo borramos el paquete `at`.
```bash
root@client1 [ CentOS8 ]
~
hist:269 -> yum remove at
Dependencies resolved.
==========================================================================================
 Package                   Architecture    Version             Repository            Size
==========================================================================================
Removing:
 at                        x86_64          3.1.20-11.el8       @baseos              129 k

Transaction Summary
==========================================================================================
Remove  1 Package

Freed space: 129 k
Is this ok [y/N]: y
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                 1/1
  Running scriptlet: at-3.1.20-11.el8.x86_64                                         1/1
warning: Unable to get systemd shutdown inhibition lock: Unit systemd-logind.service is masked.

  Erasing          : at-3.1.20-11.el8.x86_64                                         1/1
  Running scriptlet: at-3.1.20-11.el8.x86_64                                         1/1
  Verifying        : at-3.1.20-11.el8.x86_64                                         1/1

Removed:
  at-3.1.20-11.el8.x86_64

Complete!
```

### Mostrar información acerca de un paquete

Para mostrar los detalles pertinentes a un paquete solo tenemos que usar el parametro `info` y pasar el nombre del paquete.<br>
En este ejemplo obtenemos la información del paquete `at`.
```bash
root@client1 [ CentOS8 ]
~
hist:266 -> yum info at
Last metadata expiration check: 0:34:16 ago on Sun Jun 25 13:41:21 2023.
Available Packages
Name         : at
Version      : 3.1.20
Release      : 11.el8
Architecture : x86_64
Size         : 81 k
Source       : at-3.1.20-11.el8.src.rpm
Repository   : baseos
Summary      : Job spooling tools
URL          : http://ftp.debian.org/debian/pool/main/a/at
License      : GPLv3+ and GPLv2+ and ISC and MIT and Public Domain
Description  : At and batch read commands from standard input or from a specified
             : file. At allows you to specify that a command will be run at a
             : particular time. Batch will execute commands when the system load
             : levels drop to a particular level. Both commands use user's shell.
             :y
             : You should install the at package if you need a utility for
             : time-oriented job control. Note: If it is a recurring job that will
             : need to be repeated at the same time every day/week, etc. you should
             : use crontab instead.
```
Algunos aspectos importates son:

Architecture
: reflejan en la que este paquete trabaja

Source
: el nombre completo de el RPM

Repository
: el repositorio de origen

### Buscar un paquete

Para mostrar todos los paquetes relacionados a una categoria podemos usar el parametro `search` y pasar el nombre del paquete.<br>
En este ejemplo obtenemos la información del paquete `httpd`.<br>
La lista mostrada aqui es parcial. 
```bash
root@client1 [ CentOS8 ]
~
hist:267 -> yum search httpd
Last metadata expiration check: 0:07:53 ago on Sun Jun 25 13:41:21 2023.
=========================== Name Exactly Matched: httpd ===================================
httpd.x86_64 : Apache HTTP Server
=========================== Name Matched: httpd ===========================================
httpd-devel.x86_64 : Development interfaces for the Apache HTTP server
httpd-filesystem.noarch : The basic directory layout for the Apache HTTP server
httpd-manual.noarch : Documentation for the Apache HTTP server
httpd-tools.x86_64 : Tools for use with the Apache HTTP Server
libmicrohttpd.i686 : Lightweight library for embedding a webserver in applications
libmicrohttpd.x86_64 : Lightweight library for embedding a webserver in applications
lighttpd.x86_64 : Lightning fast webserver with light system requirements
perl-Test-Fake-HTTPD.noarch : Fake HTTP server module for testing
sympa-httpd.x86_64 : Sympa with Apache HTTP Server
sysusage-httpd.noarch : Apache configuration for sysusage
thttpd.x86_64 : A tiny, turbo, throttleable lightweight HTTP server
========================= Summary Matched: httpd ===========================================
mod_auth_mellon.x86_64 : A SAML 2.0 authentication module for the Apache Httpd Server
mod_dav_svn.x86_64 : Apache httpd module for Subversion server
```

## Otras Opciones Usadas Con YUM

Has otras actividade menos comunes que podemos hacer con YUM.

### Historia de Actividad

Podemos mostrar la historia de actividades recientes que YUM ha ejecutado.
```bash
Sun 2023Jun25 13:46:59 PDT
root@client1 [ CentOS8 ]
~
hist:265 -> yum history
ID     | Command line                          | Date and time    | Action(s)      | Altered
--------------------------------------------------------------------------------------------
    18 | install authselect -y                 | 2023-06-24 22:38 | Install        |    2 EE
    17 | install pam-devel                     | 2023-06-24 20:57 | Install        |    1 EE
    16 | install redhat-rpm-config -y          | 2023-06-24 20:55 | Install        |   16 EE
    15 | install httpd-devel                   | 2023-06-24 20:52 | Install        |    8 EE
    14 | install httpd                         | 2023-06-24 20:51 | Install        |   11 EE
```
Las historia muestra que hemos instalado varios paquetes y la fecha. Esto puede ser muy útil para auditar y encontrar la razón de algun problema.

### Limpiar el Caché

Si asi lo requerimos, podemos limpiar el caché de la computadora
```bash
root@client1 [ CentOS8 ]
~
hist:273 -> yum clean all
45 files removed
```

### Recrear el Caché

Podemos limpiar el caché de la computadora.
```bash
root@client1 [ CentOS8 ]
~
hist:275 -> yum makecache
CentOS Linux 8 - AppStream                             6.0 MB/s | 8.4 MB     00:01
CentOS Linux 8 - BaseOS                                5.9 MB/s | 4.6 MB     00:00
CentOS Linux 8 - Extras                                 39 kB/s |  10 kB     00:00
CentOS Linux 8 - PowerTools                            4.0 MB/s | 2.3 MB     00:00
Extra Packages for Enterprise Linux 8 - x86_64         2.7 MB/s |  16 MB     00:05
Metadata cache created.
```

### Grupos de Software

Ocasionalmente deseamos intalar un grupo entero de paquetes.<br>
Podemos usar el parametro `group` para buscar los nombres de grupos de paquetes.
```bash
root@client1 [ CentOS8 ]
~
hist:276 -> yum group
Last metadata expiration check: 0:00:45 ago on Sun Jun 25 14:18:31 2023.
Available Groups: 14

Sun 2023Jun25 14:19:17 PDT
root@client1 [ CentOS8 ]
~
hist:277 -> yum grouplist
Last metadata expiration check: 0:00:50 ago on Sun Jun 25 14:18:31 2023.
Available Environment Groups:
   Server with GUI
   Server
   Minimal Install
   Workstation
   KDE Plasma Workspaces
   Virtualization Host
   Custom Operating System
Available Groups:
   Container Management
   .NET Core Development
   RPM Development Tools
   Development Tools
   Graphical Administration Tools
   Headless Management
   Legacy UNIX Compatibility
   Network Servers
   Scientific Support
   Security Tools
   Smart Card Support
   System Tools
   Fedora Packager
   Xfce
```

El siguiente comando instala un grupo entero de paquetes.
```bash
-> yum groupinstall "Development Tools"
```

### Usar el SHELL de YUM

Shell specific arguments:

config                   
: set config options

help                     
: print help

repository (or repo)     
: enable, disable or list repositories

resolvedep               
: resolve the transaction set

transaction (or ts)      
: list, reset or run the transaction set

run                     
: resolve and run the transaction set

exit (or quit)           
: exit the shell

Iniciamos la session interactiva con el command `yum shell`.
```bash
root@client1 [ CentOS8 ]
~
hist:280 -> yum shell
Last metadata expiration check: 0:02:21 ago on Sun Jun 25 14:18:31 2023.
> info httpd
Installed Packages
Name         : httpd
Version      : 2.4.37
Release      : 43.module_el8.5.0+1022+b541f3b1
Architecture : x86_64
Size         : 4.3 M
Source       : httpd-2.4.37-43.module_el8.5.0+1022+b541f3b1.src.rpm
Repository   : @System
From repo    : appstream
Summary      : Apache HTTP Server
URL          : https://httpd.apache.org/
License      : ASL 2.0
Description  : The Apache HTTP Server is a powerful, efficient, and extensible
             : web server.
```

Una vez en el YUM SHELL hacemos lo siguiente:

Instalar un paquete.
```bash
> install httpd
Package httpd-2.4.37-43.module_el8.5.0+1022+b541f3b1.x86_64 is already installed.
```

Abilitar o desabilitar in repositorio.
```bash
> repository disable epel
Last metadata expiration check: 0:06:27 ago on Sun Jun 25 14:18:26 2023.
.
> repository enable epel
Last metadata expiration check: 0:06:27 ago on Sun Jun 25 14:18:31 2023.
>
```

Podemos dejar el yum shell cuando hemos completado las tareas.
```bash
> quit
Leaving Shell
```

## Bajar e Instalar RPM Manualmente

Podemos estar en una situación en la que tenemos que instalar un rpm manualmente.<br>

Sigamos los pasos siguientes.

Crear directorio donde bajaremos el rpm.
```bash
root@client1 [ CentOS8 ]
hist:295 -> mkdir Downloads
```
Entrar el comando para bajar el rpm.
```bash
root@client1 [ CentOS8 ]
hist:302 -> yum download  at-3.1.20-11.el8.x86_64 --destdir Downloads
Last metadata expiration check: 0:40:55 ago on Sun Jun 25 14:18:31 2023.
at-3.1.20-11.el8.x86_64.rpm                         183 kB/s |  81 kB     00:00

root@client1 [ CentOS8 ]
hist:303 -> ll Downloads/
-rw-r--r-- 1 root root 83344 Jun 25 14:59 at-3.1.20-11.el8.x86_64.rpm
```
Entrar el comando para instalar el rpm.
```bash
root@client1 [ CentOS8 ]
hist:304 -> yum install Downloads/at-3.1.20-11.el8.x86_64.rpm
```