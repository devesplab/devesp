---
layout: default
title: Auditando Usuarios
permalink: /auditando-usuarios/
parent: Usuarios
grand_parent: Linux
has_children: false
has_toc: false
nav_order: 3
---

# Auditando Usuarios En Linux

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

## Seguridad En Linux

Hay varias razones por las que debemos tener en mente el tópico de Seguridad en Linux:
- protección en contra de hackers
- privilegio de acceso a información propietaria de alta confidencialidad
- proteger todo el entorno de computación empresarial
- mantener los requerimientos y regulaciones de la industria
- minimizar el impacto de perdida en caso de exponer una parte del ambiente

La manera mas elemental de proteger los sistemas es enforzar medidas de seguridad para limitar la entrada solo a los usuarios o procesos authorizados.

También adoptamos principios de auditaje para asegurarnos que solo cuentas authorizadas interactuan en el entorno.

Linux provee utilidades nativas básicas que ayudan a identifar la frequencia y tiempo de acceso a los sistemas.

En esta lección identificamos comandos para rastrear actividad de usuarios en un sistema de Ubuntu.

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

## journalctl

El comando `journalctl` también puede usarse para chequer actividad de usuarios.
```
--> journalctl -u devuser
-- No entries --
```
En este ejemplo no hay actividad el usuario `devuser`.

## Last

En la mayoría de variaciones de Linux, el comando `last` muestra quien esta activo en el sistema.

```
-> last
reboot   system boot  6.3.13-linuxkit  Sat May 18 20:13   still running
reboot   system boot  6.3.13-linuxkit  Sat May 18 20:11   still running

wtmp begins Sat May 18 20:11:22 2024
```

## auth.log

En sistemas Linux como Ubuntu, /var/log/auth.log es un archivo de registro que registra eventos relacionados con la autenticación y la seguridad. Se utiliza principalmente por sistemas que ejecutan syslog tradicional (como rsyslog) para rastrear cualquier cosa relacionada con el inicio de sesión y la verificación de identidad.

Para habilitar esta función, haz esto:
```
-> sudo apt install rsyslog

-> sudo systemctl enable --now rsyslog
```
Luego podras ver la actividad del sistema en el archivo `/var/log/auth.log`.
```
-> sudo tail -f /var/log/auth.log
```
Las entradas típicas incluyen: 
- Intentos de inicio de sesión por SSH (exitosos y fallidos) 
- Inicios de sesión locales (TTY, sesiones de interfaz gráfica) 
- Uso de sudo (quién usaba qué como root) 
- Fallos de autenticación (contraseña incorrecta, usuario inválido) 
- Eventos PAM (Módulos de Autenticación Enchufables) 
- Bloqueos de cuenta (si están configurados)

En este ejemplo el usuario `devuser` atenta hacer login a `root` y falla.<br>
El archivo `auth.log` capturó la actividad indicando la razón: `FAILED SU`
```
devuser@devesp
~
hist:232 -> su - root
Password:
su: Authentication failure


Sat 2026May23 04:38:26 UTC
devuser@devesp
~
hist:233 -> sudo tail -f /var/log/auth.log
2026-05-23T04:38:23.839909+00:00 client1 su: pam_unix(su-l:auth): authentication failure; logname= uid=1001 euid=0 tty=/dev/pts/1 ruser=devuser rhost=  user=root
2026-05-23T04:38:25.318905+00:00 client1 su[3293]: FAILED SU (to root) devuser on pts/1
2026-05-23T04:38:32.545058+00:00 client1 sudo:  devuser : TTY=pts/1 ; PWD=/home/devuser ; USER=root ; COMMAND=/usr/bin/tail -f /var/log/auth.log
2026-05-23T04:38:32.545425+00:00 client1 sudo: pam_unix(sudo:session): session opened for user root(uid=0) by (uid=1001)
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

La sección de [referencias](#referencias) contiene enlaces a utilidades de Código Abierto tales como Fail2Ban y LogWatch. Podemos instalar esas utilidades en el sistema para observar y rastrear acceso al sistema o bloquear acceso si es necesario.

Generalmente los archivos de registros van en `/var/log`, pero en la majoria de los casos, las aplicaciones permiten cambiar a otro lugar de acuerdo a la política del entorno.

## Conclusion

Es importante estar informado de la actividad de usuarios en un sistema. Esto puede prevenir que un usuario malicioso entre sin autorización y se propague a otros sistems donde pueda causar daños irreparables. La exposición y pérdida de información puede causar daño a la reputación de individuos o empresas lo que proyecta una mala imagen de manejo de negocios.

## Referencias 

### Glosario De Comandos

Los comandos siguientes son usados frecuentemente en sesiones de Linux.

lastlog
: informa el inicio de sesión más reciente de todos los usuarios o de un usuario determinado

faillog
: mostrar registros de fallas o establecer límites de fallas de registro de acceso

### [](referencias)Herramientas de Código Abierto

- [Fail2Ban](https://github.com/fail2ban/fail2ban)
- [LogWatch](https://github.com/gjalves/logwatch)

Paginas Manuales
- [faillog](https://manpages.ubuntu.com/manpages/focal/en/man8/faillog.8.html)
- [lastlog](https://manpages.ubuntu.com/manpages/focal/en/man8/lastlog.8.html)
- [last](https://manpages.ubuntu.com/manpages/focal/en/man1/last.1.html)
- [auditd](https://manpages.ubuntu.com/manpages/focal/en/man8/auditd.8.html)

[Return to main page]({{site.baseurl}}/).
