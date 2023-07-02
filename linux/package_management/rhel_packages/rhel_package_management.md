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

dnf [options] COMMAND
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
devuser@server1 [ RHEL9 ]
~
hist:117 -> ls -l /etc/yum.repos.d/
total 96
-rw-r--r--  1 root root  1453 Apr 14 14:22 epel.repo
-rw-r--r--  1 root root  1552 Apr 14 14:22 epel-testing.repo
-rw-r--r--. 1 root root 88244 Jun 20 18:40 redhat.repo
```

Un repositorio esta activo cuando tiene `enabled=1`.<br>
Si tiene `enabled=0`, indica que esta desabilitado.<br>
En este ejemplo, un sistema de RHEL9, tiene el repositorio **redhat.repo** el cual esta abilitado porque tiene `enabled` igual a `1`.
```bash
devuser@server1 [ RHEL9 ]
hist:116 -> cat //etc/yum.repos.d/redhat.repo
[rhel-9-for-x86_64-baseos-rpms]
name = Red Hat Enterprise Linux 9 for x86_64 - BaseOS (RPMs)
baseurl = https://cdn.redhat.com/content/dist/rhel9/$releasever/x86_64/baseos/os
enabled = 1
gpgcheck = 1
gpgkey = file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
sslverify = 1
sslcacert = /etc/rhsm/ca/redhat-uep.pem
sslclientkey = /etc/pki/entitlement/1800058266028775816-key.pem
sslclientcert = /etc/pki/entitlement/1800058266028775816.pem
sslverifystatus = 1
metadata_expire = 86400
enabled_metadata = 1
```

El siguiente comando muestra como listar todos los repositorios disponibles en el sistema.
```bash
-> yum repolist
repo id                                repo name
epel                                   Extra Packages for Enterprise Linux 9 - x86_64
rhel-9-for-x86_64-appstream-rpms       Red Hat Enterprise Linux 9 for x86_64 - AppStream (RPMs)
rhel-9-for-x86_64-baseos-rpms          Red Hat Enterprise Linux 9 for x86_64 - BaseOS (RPMs)
```

### Cual paquete nos da una utilidad?

Este ejemplo nos dice que el paquete `coreutils-8.32-31.el9.x86_64` provee el comando `echo`
```bash
devuser@server1 [ RHEL9 ]
~
hist:117 -> yum provides echo
Extra Packages for Enterprise Linux 9 - x86_64                    882 kB/s |  18 MB     00:20
Red Hat Enterprise Linux 9 for x86_64 - AppStream (RPMs)          6.0 MB/s |  22 MB     00:03
Red Hat Enterprise Linux 9 for x86_64 - BaseOS (RPMs)             4.7 MB/s |  13 MB     00:02
Last metadata expiration check: 0:00:02 ago on Sun 02 Jul 2023 02:19:39 PM PDT.
coreutils-8.32-31.el9.x86_64 : A set of basic GNU tools commonly used in shell scripts
Repo        : rhel-9-for-x86_64-baseos-rpms
Matched from:
Provide    : /bin/echo
Filename    : /usr/bin/echo
```

### Listar paquetes

Podemos listar un solo paquete.<br>
En este ejemplo listamos el paquete `coreutils`.
```bash
-> yum list coreutils
Not root, Subscription Management repositories not updated
Last metadata expiration check: 0:08:08 ago on Sun Jul  2 14:13:26 2023.
Available Packages
coreutils.x86_64       8.32-34.el9         ubi-9-baseos-rpms    3.1.20-11.el8      baseos
```

Esto muestra la lista completa de todos los paquetes disponibles de todos los repositorios presentes 
en el sistema.
```bash
-> yum list --available
```
La lista puede ser bastante larga, asi que podemos restringir la busqueda a lo que nos interesa.<br>
Por ejemplo, aqui buscamos todos los paquetes referentes a `python`.
```bash
-> yum list --available | grep python
``` 

### Instalar un Paquete

Podemos usar el parametro `install` y pasar el nombre del paquete.<br>
La operacion encontrara las dependencias disponibles si existen.<br>
En este ejemplo instalamos el paquete `at`.
```bash
root@rhel9-1
~
hist:73 -> yum install at
Updating Subscription Management repositories.
Unable to read consumer identity
Subscription Manager is operating in container mode.

This system is not registered with an entitlement server. You can use subscription-manager to register.

Last metadata expiration check: 1 day, 0:33:33 ago on Sat Jul  1 13:54:55 2023.
Dependencies resolved.
==================================================================================
 Package            Architecture     Version           Repository            Size
==================================================================================
Installing:
 at                 x86_64           3.1.23-11.el9     ubi-9-baseos-rpms     69 k

Transaction Summary
==================================================================================
Install  1 Package

Total download size: 69 k
Installed size: 124 k
Is this ok [y/N]: y
Downloading Packages:
at-3.1.23-11.el9.x86_64.rpm                         176 kB/s |  69 kB     00:00
--------------------------------------------------------------------------------
Total                                               174 kB/s |  69 kB     00:00
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                        1/1
  Installing       : at-3.1.23-11.el9.x86_64                                1/1
  Running scriptlet: at-3.1.23-11.el9.x86_64                                1/1
Created symlink /etc/systemd/system/multi-user.target.wants/atd.service → /usr/lib/systemd/system/atd.service.

  Verifying        : at-3.1.23-11.el9.x86_64                                1/1
Installed products updated.

Installed:
  at-3.1.23-11.el9.x86_64

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
root@rhel9-1
~
hist:74 -> yum remove at
Updating Subscription Management repositories.
Unable to read consumer identity
Subscription Manager is operating in container mode.

This system is not registered with an entitlement server. You can use subscription-manager to register.

Dependencies resolved.
=================================================================================
 Package            Architecture    Version          Repository             Size
=================================================================================
Removing:
 at                 x86_64          3.1.23-11.el9    @ubi-9-baseos-rpms    124 k

Transaction Summary
=================================================================================
Remove  1 Package

Freed space: 124 k
Is this ok [y/N]: y
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                        1/1
  Running scriptlet: at-3.1.23-11.el9.x86_64                                1/1
Removed "/etc/systemd/system/multi-user.target.wants/atd.service".

  Erasing          : at-3.1.23-11.el9.x86_64                                1/1
  Running scriptlet: at-3.1.23-11.el9.x86_64                                1/1
  Verifying        : at-3.1.23-11.el9.x86_64                                1/1
Installed products updated.

Removed:
  at-3.1.23-11.el9.x86_64

Complete!
```

### Mostrar información acerca de un paquete

Para mostrar los detalles pertinentes a un paquete solo tenemos que usar el parametro `info` y pasar el nombre del paquete.<br>
En este ejemplo obtenemos la información del paquete `at`.
```bash
root@rhel9-1
~
hist:75 -> yum info at
Updating Subscription Management repositories.
Unable to read consumer identity
Subscription Manager is operating in container mode.

This system is not registered with an entitlement server. You can use subscription-manager to register.

Last metadata expiration check: 1 day, 0:40:27 ago on Sat Jul  1 13:54:55 2023.
Installed Packages
Name         : at
Version      : 3.1.23
Release      : 11.el9
Architecture : x86_64
Size         : 124 k
Source       : at-3.1.23-11.el9.src.rpm
Repository   : @System
From repo    : ubi-9-baseos-rpms
Summary      : Job spooling tools
URL          : http://ftp.debian.org/debian/pool/main/a/at
License      : GPLv3+ and GPLv2+ and ISC and MIT and Public Domain
Description  : At and batch read commands from standard input or from a specified
             : file. At allows you to specify that a command will be run at a
             : particular time. Batch will execute commands when the system load
             : levels drop to a particular level. Both commands use user's shell.
             :
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
root@rhel9-1
~
hist:76 ->  yum search httpd
Last metadata expiration check: 1 day, 0:41:06 ago on Sat Jul  1 13:54:55 2023.
================ Name Exactly Matched: httpd ================
httpd.x86_64 : Apache HTTP Server
================ Name & Summary Matched: httpd ================
httpd-core.x86_64 : httpd minimal core
lighttpd-fastcgi.x86_64 : FastCGI module and spawning helper for lighttpd and PHP configuration
lighttpd-filesystem.noarch : The basic directory layout for lighttpd
lighttpd-mod_authn_gssapi.x86_64 : Authentication module for lighttpd that uses GSSAPI
(...snip...)
```

## Otras Opciones Usadas Con YUM

Has otras actividade menos comunes que podemos hacer con YUM.

### Historia de Actividad

Podemos mostrar la historia de actividades recientes que YUM ha ejecutado.
```bash
root@rhel9-1
~
hist:76 -> yum history
ID     | Command line             | Date and time    | Action(s)      | Altered
-------------------------------------------------------------------------------
    15 | install at -y            | 2023-07-02 14:35 | Install        |    1 EE
    14 | remove at                | 2023-07-02 14:32 | Removed        |    1 EE
    13 | install at               | 2023-07-02 14:28 | Install        |    1 EE

```
Las historia muestra que hemos instalado varios paquetes y la fecha. Esto puede ser muy útil para auditar y encontrar la razón de algun problema.

### Limpiar el Caché

Si asi lo requerimos, podemos limpiar el caché de la computadora
```bash
root@rhel9-1
~
hist:76 -> yum clean all
37 files removed
```

### Recrear el Caché

Podemos limpiar el caché de la computadora.
```bash
root@rhel9-1
~
hist:77 -> yum makecache
Extra Packages for Enterprise Linux 9 - x86_64             2.5 MB/s |  18 MB     00:07
Red Hat Universal Base Image 9 (RPMs) - BaseOS             735 kB/s | 580 kB     00:00
Red Hat Universal Base Image 9 (RPMs) - AppStream          2.0 MB/s | 1.9 MB     00:00
Red Hat Universal Base Image 9 (RPMs) - CodeReady Builder  286 kB/s | 195 kB     00:00
Last metadata expiration check: 0:00:01 ago on Sun Jul  2 14:39:28 2023.
Metadata cache created.

```

### Grupos de Software

Ocasionalmente deseamos intalar un grupo entero de paquetes.<br>
Podemos usar el parametro `group` para buscar los nombres de grupos de paquetes.
```bash
root@rhel9-1
~
hist:78 -> yum group
Last metadata expiration check: 0:01:11 ago on Sun Jul  2 14:39:28 2023.
Available Groups: 2

root@rhel9-1
~
hist:79 -> yum grouplist
Last metadata expiration check: 0:01:36 ago on Sun Jul  2 14:39:28 2023.
Available Environment Groups:
   KDE Plasma Workspaces
Available Groups:
   Fedora Packager
   Xfce
```

El siguiente comando instala un grupo entero de paquetes.
```bash
-> yum groupinstall "KDE Plasma Workspaces"
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
root@rhel9-1
~
hist:80 -> yum shell
> info httpd
Available Packages
Name         : httpd
Version      : 2.4.53
Release      : 11.el9_2.5
Architecture : x86_64
Size         : 53 k
Source       : httpd-2.4.53-11.el9_2.5.src.rpm
Repository   : ubi-9-appstream-rpms
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
Last metadata expiration check: 0:03:51 ago on Sun Jul  2 14:39:28 2023.
.
> repository enable epel
Last metadata expiration check: 0:03:58 ago on Sun Jul  2 14:39:28 2023.
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
Cambiemos al directorio que hemos creado.
```bash
root@rhel9-1
~
hist:80 -> mkdir Downloads && cd Downloads
```
Entrar el comando para bajar el rpm.
```bash
root@rhel9-1
~/Downloads
hist:80 -> yum download  at-3.1.20-11.el8.x86_64 --destdir Downloads
Last metadata expiration check: 0:40:55 ago on Sun Jun 25 14:18:31 2023.
at-3.1.20-11.el8.x86_64.rpm                         183 kB/s |  81 kB     00:00

root@rhel9-1
~Downloads
hist:80 -> ls 0l ~/Downloads/
-rw-r--r-- 1 root root 83344 Jun 25 14:59 at-3.1.20-11.el8.x86_64.rpm
```
Entrar el comando para instalar el rpm.
```bash
root@rhel9-1
~Downloads
hist:80 -> yum install Downloads/at-3.1.20-11.el8.x86_64.rpm
```