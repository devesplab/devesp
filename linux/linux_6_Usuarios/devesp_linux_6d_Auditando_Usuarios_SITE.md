---
layout: default
title: Auditando Usuarios
permalink: /auditando-usuarios/
parent: Usuarios
grand_parent: Linux
has_children: true
has_toc: false
nav_order: 1
---

# LINUX :: Usuarios :: Auditando Usuarios

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

**DESCRIPCION**

En esta leccion:
- identificar comandos para rastrear actividad de usuarios

**DEPENDENCIAS**

ninguna

**REQUERIMIENTOS**

Sistema de Linux Ubuntu. <br>
Acceso a la terminal de Linux.<br>
Alguos comandos requieren privilegios elevados.

**ADVERTENCIA**

ninguna.

## Working Environment

En esta leccion usamos el sistema operativo Ubuntu.

## Comandos Para Chequear Actividad de Login

Cada actividad en un sistema de Linux is identificada y manejado por un id de usuario.

La identificación del usuario se puede rastrear en archivos de registro.

## Lastlog

El comando `lastlog` ayuda a identificar cuando un usuario ha entrado al sistema.
En este ejemplo no hay mucha actividad porque es un sistema virtual de poco uso.
```
 -> lastlog
Username         Port     From             Latest
root                                       **Never logged in**
daemon                                     **Never logged in**
bin                                        **Never logged in**
sys                                        **Never logged in**
sync                                       **Never logged in**
games                                      **Never logged in**
man                                        **Never logged in**
lp                                         **Never logged in**
mail                                       **Never logged in**
news                                       **Never logged in**
```

Esto muestra la actividad de un usuario específico.
```
-> lastlog --user appuser
Username         Port     From             Latest
appuser                                    **Never logged in**
```

Para más información ver la ayuda en linea del comando.
```
-> lastlog --help
```

## Faillog

El comando `failog` también puede usarse para chequer actividad de usuarios.
```
root@ubuntu2204-1-devesp
hist:104 -> faillog -a
Login       Failures Maximum Latest                   On
root            0        0   01/01/70 00:00:00 +0000
daemon          0        0   01/01/70 00:00:00 +0000
bin             0        0   01/01/70 00:00:00 +0000
sys             0        0   01/01/70 00:00:00 +0000
sync            0        0   01/01/70 00:00:00 +0000
games           0        0   01/01/70 00:00:00 +0000
man             0        0   01/01/70 00:00:00 +0000
lp              0        0   01/01/70 00:00:00 +0000
mail            0        0   01/01/70 00:00:00 +0000
```
El comando `failog`  usa el archivo `/var/log/faillog` para almazenar su información.

Para más información ver la ayuda en linea del comando.
```
-> faillog --help
```

## last

En la mayoría de variaciones de Linux, el comando `last` muestra quien esta activo en el sistema.

```
-> last
reboot   system boot  6.3.13-linuxkit  Sat May 18 20:13   still running
reboot   system boot  6.3.13-linuxkit  Sat May 18 20:11   still running

wtmp begins Sat May 18 20:11:22 2024
```

## Auditd

La página manual de la utilidad `auditd` en Ubuntu dice lo siguiente:
"_La utilidad `auditd` es el componente de espacio de usuario del sistema de auditoría de Linux. es responsable de escribir registros de auditoría en el disco. La visualización de los registros se realiza con ausearch o ureport utilidades. La configuración del sistema de auditoría o la carga de reglas se realiza con auditctl utilidad_"

La utilidad puede installarse en Ubuntu y empezarla como un servicio de SystemD.
```
-> apt install auditd
-> systemctl {status|stop|start} auditd
```
Auditd mantiene los siguientes archivos:
- archivo de registro en `/var/log/audit/audit.log`
- configuración en `/etc/audit/auditd.conf`
- reglas en `/etc/audit/audit.rules` 

## Seguimiento de Actividad de Aplicaciones de Terceros

Cada applicacíon de terceros establece particularidades de acceso y seguridad. Un caso notable y muy conocido es SSH, el cuál crea una entrada en su archivo de registro. Otro ejemplos son aplicaciones de bases de datos como MySQL y PostgreSQL. Esta de menos decir que aplicaciones comerciales tienen sofisticaciones internas para rastrear el uso e implementacoin necesarias para diagnosticar problemas que ayudan an mejoras sus productos.

Generalmente los archivos de registros van en `/var/log`, pero en la majoria de los casos, las aplicaciones permiten cambiar a otro lugar.

## Conclusion

Es importante estar informado de la actividad de usuarios en un sistem. Esto puede prevenir que un usuario malicioso entre sin autorización y se propague a otros sistems donde pueda causar daños irreparables.

## Referencias 

### Glosario De Comandos

Los comandos siguientes son usados frecuentemente en sesiones de Linux.

lastlog
: informa el inicio de sesión más reciente de todos los usuarios o de un usuario determinado

faillog
: mostrar registros de fallas o establecer límites de fallas de registro de acceso

### Referencias Utiles

DevEsp :: Linux
- https://docs.devesp.com/linux-en-espa%C3%B1ol/

Paginas Manuales
- [faillog](https://manpages.ubuntu.com/manpages/focal/en/man8/faillog.8.html)
- [lastlog](https://manpages.ubuntu.com/manpages/focal/en/man8/lastlog.8.html)
- [last](https://manpages.ubuntu.com/manpages/focal/en/man1/last.1.html)
- [auditd](https://manpages.ubuntu.com/manpages/focal/en/man8/auditd.8.html)
