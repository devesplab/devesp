---
layout: default
title: Manejando Procesos
permalink: /manejar_procesos/
parent: Procesos
grand_parent: Linux
has_children: true
has_toc: false
nav_order: 2
---

# Manejando Procesos

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

Todo tipo de sistema requiere de la existencia de un método para manejarlo. En el caso de Linux, tenemos disponibles varios comandos y utilidades con diferentes características para manejar procesos. Debemos tener en mente que hay diferentes categorías de procesos: usuario, aplicación y sistema.

## Manejar Procesos De Usuario Manualmente

Supongamos que hemos empezado un programa en segundo plano que nos muestra el PID correspondiente.

```bash
~/linux-devesp/linux-scripts/forever-loop.sh &
[1] 16952

```

Mas tarde, podemos usar el comando `ps` en combinación con `grep` para localizar y ver el estado corriente del proceso.

```bash
-> ps -auxw
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
devuser    16952  0.0  0.0   4496  3328 pts/0    S    18:03   0:00 bash ./forever-loop.sh
```

Podemos terminar ese programa con el comando `kill` pasando como argumento el `PID` del proceso.
```bash
kill -9 16952
```

## Manejar Procesos De Aplicaciones

Una aplicacion se entiende como un proceso que no pertenece directamente al sistema operativo o a un usuario regular. Por ejemplo NGINX.

Veamos un ejemplo rápido en Ubuntu.
- instalar nginx (paquete nginx)
- empezar la aplicación (proceso nginx)
- terminar la aplicación (proceso nginx)

```bash
devuser@ubuntu2204-2-devesp
hist:58 -> sudo apt install nginx

devuser@ubuntu2204-2-devesp
hist:60 -> sudo systemctl start nginx

devuser@ubuntu2204-2-devesp
hist:61 -> sudo systemctl status nginx
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
     Active: active (running) since Sat 2024-08-03 06:08:36 UTC; 4s ago
       Docs: man:nginx(8)
    Process: 1336 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
    Process: 1337 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
   Main PID: 1338 (nginx)
      Tasks: 7 (limit: 19648)
     Memory: 7.4M
        CPU: 31ms
     CGroup: /system.slice/nginx.service
             ├─1338 "nginx: master process /usr/sbin/nginx -g daemon on; master_process on;"
             ├─1339 "nginx: worker process" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ""
             ├─1340 "nginx: worker process" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ""
             ├─1341 "nginx: worker process" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ""
             ├─1342 "nginx: worker process" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ""
             ├─1343 "nginx: worker process" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ""
             └─1344 "nginx: worker process" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ""

Aug 03 06:08:36 ubuntu2204-2-devesp systemd[1]: Starting A high performance web server and a reverse proxy server...
Aug 03 06:08:36 ubuntu2204-2-devesp systemd[1]: Started A high performance web server and a reverse proxy server.

devuser@ubuntu2204-2-devesp
hist:61 -> sudo systemctl stop nginx

devuser@ubuntu2204-2-devesp
hist:62 -> sudo systemctl status nginx
○ nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
     Active: inactive (dead) since Sat 2024-08-03 06:08:45 UTC; 1min 58s ago
       Docs: man:nginx(8)
    Process: 1336 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
    Process: 1337 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
    Process: 1361 ExecStop=/sbin/start-stop-daemon --quiet --stop --retry QUIT/5 --pidfile /run/nginx.pid (code=exited, status=0/SUCCESS)
   Main PID: 1338 (code=exited, status=0/SUCCESS)
        CPU: 47ms

Aug 03 06:08:36 ubuntu2204-2-devesp systemd[1]: Starting A high performance web server and a reverse proxy server...
Aug 03 06:08:36 ubuntu2204-2-devesp systemd[1]: Started A high performance web server and a reverse proxy server.
Aug 03 06:08:45 ubuntu2204-2-devesp systemd[1]: Stopping A high performance web server and a reverse proxy server...
Aug 03 06:08:45 ubuntu2204-2-devesp systemd[1]: nginx.service: Deactivated successfully.
Aug 03 06:08:45 ubuntu2204-2-devesp systemd[1]: Stopped A high performance web server and a reverse proxy server.
```

En RedHat instalamos NGINX asi:

```
-> sudo yum install nginx
```

Luego usamos los mismos comando como en Ubuntu para empezar y terminar el proceso.
```
-> sudo systemctl start nginx
-> sudo systemctl stop nginx
-> sudo systemctl status nginx

```

## Manejar Procesos De Sistema

Típicamente los procesos de sistemas son manejados for `SYSTEMD` con el comando `systemctl`

{: .warning }
Es necesario tener privilegios de super usuario para manejar procesos de sistema.

Los usos mas comúnes son ver el estatus, terminar o empezar el proceso.<br>
Un ejemplo de proceso de sistem es SSHD, que es usado for el comando `ssh` para entrar remotamente an un sistema.

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

Ademos de eso podemos ver los logs del proceso. Usando el ejemplo de SSHD, podemos localizar `/var/log/auth.log` que muestra la actividad en tiempo real del proceso.

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

systemctl
: utilidad de línea de comandos para administrar servicios sin SystemD

journalctl
: Consultar el diario systemd


### Referencias Utiles

Paginas Manuales
- [systemctl](https://manpages.ubuntu.com/manpages/focal/en/man1/systemctl.1.html)
- [journalctl](https://manpages.ubuntu.com/manpages/focal/en/man1/journalctl.1.html)

[Return to main page]({{site.baseurl}}/).