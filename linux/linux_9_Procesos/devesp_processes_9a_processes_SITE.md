---
layout: default
title: Proceso Basicos
permalink: /proceso_basicos/
parent: Procesos
grand_parent: Linux
has_children: true
has_toc: false
nav_order: 1
---

# LINUX :: cli intro :: processes

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
- como afecta un proceso el ambiente de trabaja?
- cuando y porque se empieza un proceso? 
- que sucede cuando un proceso se para?

**DEPENDENCIAS**

ninguna

**REQUERIMIENTOS**

Sistema de linux Ubuntu. <br>
Requiere acceso a la Linea de Comandos en una terminal de Linux.

**ADVERTENCIA**

ninguna.

## Working Environment

En esta leccion usamos el sistema operativo Ubuntu.

## Que es un Procesos, PID, PPID?

Cada actividad en un sistema de Linux is identificada y manejado por un proceso. 

### Proceso

Un proceso es el mecanismo que Linux usa a travez del Kernel para segmentar recursos internos separados de otras actividades de sistema o usuario. 

El Linux Kernel juega el siguiete rol:
- alocar y manejar los recursos internos necesarios para la ejecucion de un proceso
- majera el horario cuando empezar o terminar un proceso
- interactuar con el IPC (Inter Process Communication) para que diferentes procesos se comuniquen entre si a traves de pipas, señales y sockets.

Un proceso pertenece al usuario especifico que lo empezo. El usuario tiene cierto control sobre el proceso tales como monitorear la ejecucion, terminarlo, o cambiar la prioridad.

Cada proceso es identificado por un nombre predefinido, un id numerico llamado `process id` y un id numerico padre llamado `parent process id`.

En general, un proceso de Linux es una unidad independiente de ejecucion que interactua con el Kernel y con otros proceses para ejecutar tareas y compartir recursos de una manera coordinada.

### PID

El Process ID (PID) es un valor numérico asignado por el Kernel para identificar y manejar el proceso. El PID es muy útil para diagnosticar problemas que occurren durante la ejecucion de tareas que requieren arreglos u optimización. 

### PPID

El Procesos Padre, o `Parent Process ID` (PID) valor numerico del proceso padre que empezo el proceso corrient.

Este comando muestra el proceso de usuario cuando entra al sistema.
```bash
devuser@ubuntu2204-1-devesp
~
hist:208 -> ps
    PID TTY          TIME CMD
    678 pts/0    00:00:01 bash
  16606 pts/0    00:00:00 ps
```

Aqui vemos una lista parcial de los procesos corriendo en el sistema.<br>
Notese el PPID de `0` o `1` en varios procesos.
```bash
-> ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 May06 ?        00:00:18 /lib/systemd/systemd sleep infinity --system --deserialize 31
root          54       1  0 May06 ?        00:00:00 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
postfix      628     626  0 May06 ?        00:00:00 qmgr -l -t unix -u
root         635       0  0 May06 pts/0    00:00:00 bash
devuser      678     670  0 May06 pts/0    00:00:01 -bash
root        1156       1  0 May07 ?        00:00:00 /usr/libexec/packagekitd
root       16527       1  0 11:40 ?        00:00:00 /lib/systemd/systemd-logind
root       16532       1  0 13:41 ?        00:00:00 /lib/systemd/systemd-journald
systemd+   16533       1  0 13:41 ?        00:00:00 /lib/systemd/systemd-resolved
postfix    16605     626  0 17:01 ?        00:00:00 pickup -l -t unix -u -c
```

Cuando chequeamos un proceso numérico specifico, vemos la columna `STAT`. Por ejemplo, veamos el proceso numeric `54` de la lista anterior.
```bash
UID          PID    PPID  C STIME TTY      STAT   TIME CMD
root          54       1  0 May06 ?        Ss     0:00 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
```
Lo anterior muesta un proceso de sistema popular llamdo SSHD.

{: .note }
Cuando el Proceso Padre (PPID) es `1`, signifca que le proceso padre de ese proceso en particular es el proceso `init`. El proceso INIT es el primer proceso que el KERNEL empiesa cuando el sistema arranca. El proceso INIT es responsable de empezar y manejar todos los procesos que siguen.

En los ejemplos anteriores, el encabezado del comando `ps` muestra el nombre de las columnas.

UID
: nombre del dueño del proceso

PID
: valor numerico que identifica el proceso

PPID
: valor numérico que identifica el proceso padre

C
: porcentaje de CPU usado por el proceso

STIME
: Start Time, o tiempo que ha transcurrido desde que el proceso empezó

TTY
: La terminal que esta controlando el proceso

STAT
: muestra el estado del proceso: corrindo, durmiendo.

TIME
: tiempo total que el proceso ha usado CPU

CMD
: el nombre y localiad del comando o programa que empezó el proceso

## Utilidades Para Monitorear Procesos

Hay varias utilidades disponibles en la linea de comandos para visualizar los procesos y sub-procesos activos en el sistema.

> El uso de estas utilidades asiste en la identificación de problemas de uso eficiente de CPU y memoria. 

### top
El comando `top` muestra la actividad del sistem.<br>
Aqui usamos la bandera `-n` con el valor `1` para mostrar un vista estática de los procesos que estan corriendo.
```bash
-> top -n 1
top - 17:32:41 up 5 days, 18:43,  0 users,  load average: 0.00, 0.00, 0.00
Tasks:  20 total,   1 running,  19 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :  16486.6 total,  10270.5 free,   1032.5 used,   5183.6 buff/cache
MiB Swap:   1024.0 total,   1024.0 free,      0.0 used.  14788.4 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
      1 root      20   0  182812  11648   8192 S   0.0   0.1   0:18.14 systemd
     47 message+  20   0    8800   4480   3840 S   0.0   0.0   0:03.32 dbus-daemon
     49 root      20   0   30124  18560   9984 S   0.0   0.1   0:00.06 networkd-dispat
     53 root      20   0    3192   2048   1920 S   0.0   0.0   0:00.00 agetty
     54 root      20   0   15420   9088   7552 S   0.0   0.1   0:00.00 sshd
     58 root      20   0  107500  21356  13184 S   0.0   0.1   0:00.12 unattended-upgr
    626 root      20   0   41496   4496   4096 S   0.0   0.0   0:01.48 master
    628 postfix   20   0   41880   7680   7040 S   0.0   0.0   0:00.17 qmgr
    635 root      20   0    4756   3712   3072 S   0.0   0.0   0:00.02 bash
    670 root      20   0    7204   4224   3712 S   0.0   0.0   0:00.00 su
    672 devuser   20   0   16672   9216   7936 S   0.0   0.1   0:00.50 systemd
    673 devuser   20   0  168364   4496   1664 S   0.0   0.0   0:00.00 (sd-pam)
    678 devuser   20   0    4860   3712   3072 S   0.0   0.0   0:01.08 bash
   1156 root      20   0  293248  20352  17536 S   0.0   0.1   0:00.94 packagekitd
   1160 root      20   0  234492   7168   6528 S   0.0   0.0   0:00.15 polkitd
  16527 root      20   0   15352   7424   6528 S   0.0   0.0   0:00.14 systemd-logind
  16532 root      19  -1   47716  13952  13184 S   0.0   0.1   0:00.35 systemd-journal
  16533 systemd+  20   0   25540  13552   9344 S   0.0   0.1   0:00.13 systemd-resolve
  16605 postfix   20   0   41840   7808   7168 S   0.0   0.0   0:00.00 pickup
  16675 devuser   20   0    7328   3200   2688 R   0.0   0.0   0:00.00 top
```

### htop

El comando `htop` muestra información adicional tal como el uso de cada CPU y la cantidad de memoria usada.<br>
Primero hay que instalar la utilidad en Ubuntu.
```bash
sudo apt-get install htop -y
```
Luego podemos usar la utiliad asi:
```bash
-> htop
```

### pstree

El comando `pstree` muestra los procesos en forma de árbol que es útil para ver la relacion entre procesos principales y sub-procesos.

Primero, instalemos el paquete `psmisc` en Ubuntu que provee el comando `pstree`.
```bash
-> sudo apt-get install psmisc -y
```
En RHEL instala el paquete psmisc disponible el repositorio baseos.
```bash
-> yum whatprovides psmisc -y
```
Y podemos usarlo asi usando la bandera `-p` para mostrar el PID correspondiente:
```bash
-> pstree -p
systemd(1)-+-agetty(53)
           |-dbus-daemon(47)
           |-master(626)-+-pickup(16605)
           |             `-qmgr(628)
           |-networkd-dispat(49)
           |-packagekitd(1156)-+-{packagekitd}(1157)
           |                   `-{packagekitd}(1158)
           |-polkitd(1160)-+-{polkitd}(1161)
           |               `-{polkitd}(1163)
           |-sshd(54)
           |-systemd(672)---(sd-pam)(673)
           |-systemd-journal(16532)
           |-systemd-logind(16527)
           |-systemd-resolve(16533)
           `-unattended-upgr(58)---{unattended-upgr}(79)
```

## Manejar Procesos De Usuario Manualmente

Supongamos que hemos empezado un programa en segundo plano que nos muestra el PID correspondiente.
```bash
~/linux-devesp/linux-scripts/forever-loop.sh &
[1] 16952
```
Mas tarde, podemos usar el comando `ps` en combinación con `grep` para localizar el proceso por su nombre
```bash
-> ps -auxw
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
devuser    16952  0.0  0.0   4496  3328 pts/0    S    18:03   0:00 bash ./forever-loop.sh
```
Podemos terminar ese programa con el comando `kill` pasando como argumento el PID del proceso.
```bash
kill -9 16952
```

## Manejar Procesos De Sistema

Típicamente los sistemas de procesos son manejados for SYSTEMD con el comando `systemctl`

{: .warning }
Es necesario tener privilegios de super usuario para manejar procesos de sistema.

Los usos mas comúnes son ver el estatus, terminar o empezar el proceso.<br>
Este ejemplo es con el proceso de SSHD, que es usado for el comando `ssh` para entrar remotamente an un sistema.
```bash
sudo systemctl status sshd
sudo systemctl stop sshd
sudo systemctl start sshd
```
Basicamente, cuando decimos `stop`, el proceso termina. En el ejemplo de SSHD, indica que hemos perdido la abiliad the entrar al sistem usando el comando `ssh`. Por lo tando debe ternerse sumo cuidado de planear adecuadamete cuado tal tarea tenga lugar.

## Monitorear Procesos De Sistema

Ademas de usar comandos tales como `ps` y otras utilidade, podemos hacer uso de las facilidades que provee SYSTEMD.<br> 
En este ejemplo, el comando `systemctl` muestra que SSHD esta cargado y corriendo (loaded/running).
```bash
-> sudo systemctl status sshd
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
     Active: active (running) since Sat 2024-05-11 18:55:36 UTC; 3min 9s ago
       Docs: man:sshd(8)
             man:sshd_config(5)
    Process: 17935 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
   Main PID: 17936 (sshd)
      Tasks: 1 (limit: 19648)
     Memory: 1.7M
        CPU: 22ms
     CGroup: /system.slice/ssh.service
             └─17936 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

May 11 18:55:36 ubuntu2204-1-devesp systemd[1]: Starting OpenBSD Secure Shell server...
May 11 18:55:36 ubuntu2204-1-devesp sshd[17936]: Server listening on 0.0.0.0 port 22.
May 11 18:55:36 ubuntu2204-1-devesp sshd[17936]: Server listening on :: port 22.
May 11 18:55:36 ubuntu2204-1-devesp systemd[1]: Started OpenBSD Secure Shell server.
```
Ademos de eso podemos ver los logs del proceso. Usando el ejemplo de SSHD, podemos localizar `/var/log/auth.log` que muestra la actividad en tiempo real del proceso..
```bash
-> sudo tail -f /var/log/auth.log
May 11 18:47:33 ubuntu2204-1-devesp sshd[17833]: Server listening on 0.0.0.0 port 22.
May 11 18:47:33 ubuntu2204-1-devesp sshd[17833]: Server listening on :: port 22.
May 11 18:51:00 ubuntu2204-1-devesp sshd[17833]: Received signal 15; terminating.
May 11 18:51:00 ubuntu2204-1-devesp sshd[17833]: Received signal 15; terminating.
May 11 18:55:36 ubuntu2204-1-devesp sshd[17936]: Server listening on 0.0.0.0 port 22.
May 11 18:55:36 ubuntu2204-1-devesp sshd[17936]: Server listening on :: port 22.
May 11 18:55:36 ubuntu2204-1-devesp sshd[17936]: Server listening on 0.0.0.0 port 22.
May 11 18:55:36 ubuntu2204-1-devesp sshd[17936]: Server listening on :: port 22.
```
Tambien podemos usar el comando `journalctl` para ver lo que ha pasado durante un cierto tiempo.
```bash
-> journalctl -u sshd --since "30 minutes ago"
-- No entries --
```

## Conclusion

Es importante conocer el estado de diferentes procesos en el sistema. Este conocimiento es crucial para determinar el estado general y salud de los procesos en general. Pasado el tiempo, podemos determinar si el sistema tiene la capacidad de sostener el nivel de presion o si necesitamos actualizar la configuracion para alcanzar funcionalidad optima. Es por esta razón que existen conceptos de computación tales como elasticidad, y optimización de plataformas.

## Referencias 

### Glosario De Comandos

Los comandos siguientes son usados frecuentemente en sesiones de Linux.

ps
: comando para ver procesos

systemctl
: manejar procesos de sistema (requiers poder de super usuario)

journalctl
: utilidad specializada para diagnosticar problemas de funcionamiento de aplicaciones y procesos


### Referencias Utiles

DevEsp :: Linux
- https://docs.devesp.com/linux-en-espa%C3%B1ol/

Paginas Manuales
- [systemctl](https://manpages.ubuntu.com/manpages/focal/en/man1/systemctl.1.html)
- [journalctl](https://manpages.ubuntu.com/manpages/focal/en/man1/journalctl.1.html)

Utilidad [HTOP](https://htop.dev/)

Utilidad [PSTREE](https://manpages.ubuntu.com/manpages/bionic/man1/pstree.1.html)
