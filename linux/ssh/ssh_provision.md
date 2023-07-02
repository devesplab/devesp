---
layout: default
title: Instalar y Configurar SSH
permalink: /ssh-provison/
parent: SSH
grand_parent: Linux
has_children: false
has_toc: false
nav_order: 1
---

# Instalar y Configurar SSH
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

## Instalar OpenSSH on RHEL9

En Linux SSH es mas conocido como OpenSSH, de hecho todos los paquetes de esta utilidad usan el nombre `openssh`.

Dependienddo del caso, debemos installer el cliente o servidor de OpenSSH antes que podamos usarlo.

Ent RHEL9 y CentOS, entrar este comando para instalar los ficheros principales, el client y servidor.
```bash
Sun 2023Jun25 23:33:33 UTC
root@client3 [ RHEL9 ]
hist:22 -> yum install openssh.x86_64 openssh-clients.x86_64 openssh-server.x86_64
```
openssh.x86_64
: Este paquete incluye los ficheros principales necesarios para el cliente y servidor de OpenSSH.

openssh-clients
: Este paquete incluye los clientes necesarios para hacer connecciones cifrada a servidors de SSH.

openssh-server
: Este paquete contiene el Secure Shell Damon (sshd). El damon permite que clientes de SSH se connecte en forma segura al servidor de SSH.

Verifiquemos los paquetes instalados
```bash
-> rpm -qa | grep openssh
openssh-8.7p1-29.el9_2.x86_64
openssh-clients-8.7p1-29.el9_2.x86_64
openssh-server-8.7p1-29.el9_2.x86_64
```

Este comando nos muestra le fichero de configuracion `/etc/ssh/sshd_config`.
```bash
-> rpm -qc openssh-server-8.7p1-29.el9_2.x86_64
/etc/pam.d/sshd
/etc/ssh/sshd_config
/etc/ssh/sshd_config.d/50-redhat.conf
/etc/sysconfig/sshd
```

En RHEL9, el directorio `/etc/ssh` contiene todos los fichers de configuracion.<br>
El fichero de configuracion es `/etc/ssh/sshd_config`.
```
-> ls -l /etc/ssh
total 584
-rw-r--r-- 1 root root 578094 Apr 13 13:01 moduli
-rw-r--r-- 1 root root   1921 Apr 13 13:01 ssh_config
drwxr-xr-x 2 root root   4096 Jun 25 23:43 ssh_config.d/
-rw------- 1 root root   3667 Apr 13 13:01 sshd_config
drwx------ 2 root root   4096 Jun 25 23:43 sshd_config.d/
```

## Instalar OpenSSH on UBUNTU 22.04

En Ubuntu 22.04, entrar este comando para instalar el client y servidor.
```bash
-> apt install openssh-client openssh-server
```
Las definiciones de cada paquete en CentOS tambien se aplican a Ubuntu.

Este comando nos muestra la lista parcial de ficheros que incluyen el fichero de configuracion.
```bash
->  dpkg-query -L openssh-server
/.
/etc
/etc/init.d/ssh
/etc/pam.d
/etc/pam.d/sshd
/etc/ssh
/etc/ssh/moduli
/etc/ssh/sshd_config.d
/usr/sbin/sshd
```
En Ubuntu, el directorio `/etc/ssh` contiene todos los fichers de configuracion.<br>
El fichero de configuracion es `/etc/ssh/sshd_config`.
```bash
-> ls -l /etc/ssh
total 540
-rw-r--r-- 1 root root 505426 Nov 23  2022 moduli
-rw-r--r-- 1 root root   1650 Nov 23  2022 ssh_config
drwxr-xr-x 2 root root   4096 Nov 23  2022 ssh_config.d/
-rw------- 1 root root    505 Jun 25 23:44 ssh_host_ecdsa_key
-rw-r--r-- 1 root root    174 Jun 25 23:44 ssh_host_ecdsa_key.pub
-rw------- 1 root root    399 Jun 25 23:44 ssh_host_ed25519_key
-rw-r--r-- 1 root root     94 Jun 25 23:44 ssh_host_ed25519_key.pub
-rw------- 1 root root   2602 Jun 25 23:44 ssh_host_rsa_key
-rw-r--r-- 1 root root    566 Jun 25 23:44 ssh_host_rsa_key.pub
-rw-r--r-- 1 root root    342 Dec  7  2020 ssh_import_id
-rw-r--r-- 1 root root   3254 Nov 23  2022 sshd_config
drwxr-xr-x 2 root root   4096 Nov 23  2022 sshd_config.d/
```

## Proteger OpenSSH

#### Es crucial aplicar los permisos apropiados al fichero de configuración de OpenSSH

{: .warning }
> Hagamos copias de los ficheros antes de hacer cambios.

Copiemos le fichero `/etc/ssh/sshd_config` para protegerlo de que sea cambiado al quitarle el permiso de escribir.
```bash
-> cp /etc/ssh/sshd_config /etc/ssh/sshd_config_ORIG
-> chmod a-w /etc/ssh/sshd_config_ORIG
```

Poner los permision apropiados del fichero de configuración.
```bash
-> chown root:root /etc/ssh/sshd_config 
-> chmod og-rwx /etc/ssh/sshd_config
```

Verifiquemos.
```bash
-> stat /etc/ssh/sshd_config
  File: /etc/ssh/sshd_config
  Size: 3667      	Blocks: 8          IO Block: 4096   regular file
Device: 100034h/1048628d	Inode: 3834901     Links: 1
Access: (0600/-rw-------)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2023-06-26 00:00:23.352573142 +0000
Modify: 2023-04-13 13:01:49.000000000 +0000
Change: 2023-06-26 00:08:52.401172205 +0000
 Birth: 2023-06-25 23:43:46.435161012 +0000
```

### Poner el nivel de log apropiado
Editar /etc/ssh/sshd_config y poner el parametro mostrado.
```bash
LogLevel INFO
```

### Abilitar PAM

Cuando UsePAM se usa, PAM observa los modulos de account y session. Eso es importante si queremos restringir acceso a servicios basados en el ip address, tiempo u otros aspectos de la cuenta de usuario.

Editar /etc/ssh/sshd_config y poner el parametro mostrado.
```bash
UsePAM yes
```

### Deabilitar logins de root

Esto bloquea logins para el usuario raiz (root). Requiere que usuarios entren con su propia cuenta y de acuerdo a privilegios pueden acceder la raiz una vez que han entrado al sistema.
```bash
PermitRootLogin no
```

### Desabilitar RHOSTS

Con esto no se permite el uso del fichero `.rhosts` o `/etc/hosts.equiv` si esta presente.
```bash
IgnoreRhosts yes
```

### Contraseñas vacias no se permiten

Esto bloquea el uso de contraseñas vacias en el servidor the SSH.
```
PermitEmptyPasswords no
```

### Desabilitar Variables de Ambiente

Esto no permite que usuarios pongan variables de ambiente a travez del deamon de SSH, lo cual podria potencialmente sobrepasar los protocolos de seguridad.
```
PermitUserEnvironment no
```

### Desabilitar X11 Forwarding

Desabilitar el uso de aplicaciones X11 directamente al conectarse al servidor de OpenSSH.
```
X11Forwarding no
```

### Desabilitar TCP Forwarding

SSH port forwarding es un mecanismo para crear tuneles desde el cliente al servidor o vicerversa. Un usuario malicioso podria crear una puerta trasera para entrar a la red interna desde su computadora personal. Esto es inherentemente un riesgo muy alto que no debe permitirse.
```
AllowTcpForwarding yes
```

### Crear un banner

Poner esta linea y crear el fichero `/etc/issue` con un mensaje indicando directivas de privacidad u otras guías necesarias que indiquen la politica interna de uso y seguridad.
```
Banner /etc/issue
```

### Numero Máximo de Atentados

Con esto limitamos el número de atentados que el usuario tiene antes que sea bloqueado del sistema. De esta manera reducimos la posibilidad de ataques de fuerza bruta para infiltrar el sistema ilegalmente.
```
MaxAuthTries 6
```