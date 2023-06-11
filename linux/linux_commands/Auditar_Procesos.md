---
layout: default
title: Auditar Procesos
permalink: /auditar-procesos/
parent: Comandos y Procesos
grand_parent: Linux
has_children: true
has_toc: false
nav_order: 3
---

# Auditar Procesos.
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

Es necesario tener idea del funcionamiento del sistema, aplicaciones y programas en general.

Debemos saber si los procesos estan consumiendo recursos de manera optima o en exceso, o si hay aplicaciones que funcionan despacio y necesitan ser atunadas un poco.

Las utilidades que discutimos en este espacio nos dan una vista interna a la salud de los procesos que estan corriendo en el sistema.

## ps

El comando `ps` es el metodo mas usado para mostrar lo procesos activos.
```
-> ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 00:16 ?        00:00:00 sleep infinity
root        7385       1  0 00:24 ?        00:00:00 [gpg-agent] <defunct>
root        8599       0  0 00:28 pts/0    00:00:00 bash
ntp         9340       1  0 01:36 ?        00:00:02 /usr/sbin/ntpd -p /var/run/ntp
root        9968    8599  0 05:39 pts/0    00:00:00 ps -ef
```
Usando las opciones `-aux` muestra CPU y MEM que cada proceso esta usando.
```
-> ps -aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0   2788  1012 ?        Ss   00:16   0:00 sleep infinity
root        7385  0.0  0.0      0     0 ?        Zs   00:24   0:00 [gpg-agent] <de
root        8599  0.0  0.0   4868  4120 pts/0    Ss   00:29   0:00 bash
ntp         9340  0.0  0.0  76104  5972 ?        Ssl  01:36   0:02 /usr/sbin/ntpd
root        9988  0.0  0.0   7368  2992 pts/0    R+   05:41   0:00 ps -aux
```

El comando `ps` muestra información estática de un cierto momento en el tiempo. Podemos usar otros comandos tales como `top` o `htop` para una vista mas dinamica.

## top

El comando top muestra una galeria de recursos corrientemente en uso.
```
top - 05:27:01 up  5:19,  0 users,  load average: 0.00, 0.00, 0.00
Tasks:   5 total,   1 running,   3 sleeping,   0 stopped,   1 zombie
%Cpu(s):  0.1 us,  0.1 sy,  0.0 ni, 99.8 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :  16011.0 total,    916.1 free,    820.7 used,  14274.2 buff/cache
MiB Swap:   1024.0 total,   1024.0 free,      0.0 used.  14499.0 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
      1 root      20   0    2788   1012    924 S   0.0   0.0   0:00.00 sleep
   7385 root      20   0       0      0      0 Z   0.0   0.0   0:00.00 gpg-agent
   8599 root      20   0    4868   4096   3248 S   0.0   0.0   0:00.34 bash
   9340 ntp       20   0   76104   5972   4856 S   0.0   0.0   0:02.11 ntpd
   9886 root      20   0    7312   3496   2928 R   0.0   0.0   0:00.00 top
```

## pstree

La utilidad `pstree` sirve para visualizar la asociación entre procesos. Esta utilidad nos muestra el proceso principal y los procesos secundarias bajo el mismo.

En Ubuntu instala el paquete `psmisc`.
```
-> apt-get install psmisc
```

En CentOS instala el paquete `pstree` disponible el repositorio `baseos`.
```
-> yum whatprovides pstree
```

Y luego pudes usar el comando `pstree` para mostrar la organización de procesos en el sistema.
```
-> pstree
systemd-+-chronyd
        |-dbus-daemon
        |-systemd-journal
        `-systemd-udevd
```