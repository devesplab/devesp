---
layout: default
title: LLaves de SSH
permalink: /llaves-ssh/
parent: Red y Conectividad
grand_parent: Linux
has_children: false
has_toc: false
nav_order: 2
---

# LINUX :: SSH :: Llaves

{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

**DESCRIPCION**

En esta leccion:
- crear llaves
- contraseña de llaves
- usar llaves

**DEPENDENCIAS**

ninguna

**REQUERIMIENTOS**

Sistema de linux Ubuntu. <br>
Alguos comandos requieren privilegios elevados.

**ADVERTENCIA**

ninguna.

## Working Environment

En esta leccion usamos el sistema operativo Ubuntu.

## Propósito de Llaves SSH

Supongamos que deseamos conectarnos a un sistema usando SSH con destinación a una IP.

Al intentar iniciar una sessión, veremos lo sigiente:

```bash
 -> ssh 172.46.0.3
The authenticity of host '172.46.0.3 (172.46.0.3)' can't be established.
ED25519 key fingerprint is SHA256:4JJ+Zavlqkp4mZsjCV2gN9G4a0zAoBdXHPp9eQBJlGU.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.46.0.3' (ED25519) to the list of known hosts.
user1@172.46.0.3's password:
```

Lo siguiente ha sucedido:
- SSH a identifcado la destinación remota como un sistema desconocido.<br>
- Ha agregado la IP a la lista de "known hosts".<br>
- Requiere la contraseña del usuario.

El comando automaticamente ha creado el directorio `.ssh` y el archivo `known_hosts`
```bash
-> ls -ld ~/.ssh
drwx------ 2 user1 user1 4096 Jul 13 19:36 /home/user1/.ssh/

-> ls -l ~/.ssh/
total 4
-rw-r--r-- 1 user1 user1 142 Jul 13 19:36 known_hosts
```

La discusión en esta página es usar llaves de SSH para evitar el uso de contraseñas. Si usaamos llaves, no hay necesidad de entrar una contraseña.

## Archivos y Carpetas Relevantes Para SSH

SSH usa archivos y directorios predeterminado en el ambiente del usuario.

`$HOME/.ssh`
: carpeta para guardar las llaves publicas y privadas.

authorized_keys
: archivo para guardar llaves usadas en conexiones remotas

known_hosts
: archivo que mantiene un inventorio de systemas remotos

## Crear el Directorio `.ssh`

En Ubuntu el directorio `ssh` es creado automáticamente cuando generamos una llave por primera vez.

{: .note }
En el evento que SSH no crea el directorio `$HOME/.ssh` automáticamente, podemos usar las instrucciones a seguir para provisionarlo.

SSH usa la localidad predeterminada `$HOME/.ssh` para el lugar donde guardar las llaves.

A continuación vamos a crear el directorio `.ssh` si no existe.
Aplicamos permiso `700` que es (r)ead/(w)ite/(e)xecute para el dueño solamente.
```bash
mkdir $HOME/.ssh
chmod 700 $HOME/.ssh
```

Crear el archivo `authorized_keys`.
Aplicamos permiso `600` que es (r)ead/(w)ite para el dueño solamente.
```bash
touch $HOME/.ssh/authorized_keys
chmod 600 $HOME/.ssh/authorized_keys
```

{: .warning }
> SSH tira errores si los permisos no son de esta manera:
> - `700` para `$HOME/.ssh`
> - `600` para `$HOME/.ssh/authorized_key` 


No es necesario crear el archivo `$HOME/.ssh/known_hosts` explicitamente. Este archivo es creado cuando hacemos la primera conexión.

## Crear llaves

Usamos el comando `ssh-keygen` para crear llaves que podemos usar en conexiones remotas seguras.

Por defecto, el comando `ssh-keygen` hace lo siguiente:
- guarda la llave en el directorio hogar del usuario `$HOME/.ssh`
- crea llaves de tipo RSA.
- pregunta for una frase de contraseña (passphrase), la cual es opcional
- muestra la huella (fingerprint)
- provee una imagen de arte aleatorio (randomart)

### Crear llave SSH RSA

Sin proveer ningun argumento, el comando `ssh-keygen` crea una llave RSA.<br>
En este ejemplo aceptamos los sugerencias mostradas.<br>
Presiona la tecla `Enter`cuando pregunta por "passphrase" para indicar que sera vacia.

```bash
-> ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/user1/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/user1/.ssh/id_rsa
Your public key has been saved in /home/user1/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:crZbSYv01tumkwyGmQTQeYqrGPRdCFMmgrcbrlQYoIg user1@ubuntu2204-1-devesp
The key's randomart image is:
+---[RSA 3072]----+
|+. .o= .         |
|=.ooo + .        |
|E.o.o..+         |
| oo......        |
|..oo..o.S+.      |
|..o... =+=o+     |
|.+ .    o.*o..   |
|o .      +  +o.  |
|        .   o+.  |
+----[SHA256]-----+
```

Lo anterior ha creado un par de llaves:
- `id_dsa` es la llave privada que debe mantenerse de manera segura y nunca debe ser compartida
- `id_rsa.pub` es la llave publica cuyo propósito es ser distribuida como sea necesario

```bash
-> ls -l $HOME/.ssh
total 20
-rw------- 1 user1 user1 2610 Jul 13 19:50 id_rsa
-rw-r--r-- 1 user1 user1  579 Jul 13 19:50 id_rsa.pub
```

### Crear llave SSH DSA

Creamos llaves DSA pasando el la opción `-t dsa` al comando `ssh-keygen`.
En este ejemplo aceptamos los sugerencias mostradas.
Presiona la tecla `Enter`cuando pregunta por "passphrase" para indicar que sera vacia.

```bash
-> ssh-keygen -t dsa
Generating public/private dsa key pair.
Enter file in which to save the key (/home/user1/.ssh/id_dsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/user1/.ssh/id_dsa
Your public key has been saved in /home/user1/.ssh/id_dsa.pub
The key fingerprint is:
SHA256:lGOp8NHX15dEjC1HbOFpMedlkHmF1kgnvGlAlvLXX5Q user1@ubuntu2204-1-devesp
The key's randomart image is:
+---[DSA 1024]----+
|           .o+@#X|
|       . o.oooXE@|
|    . . B .o.o=@+|
|     o = o  ..* +|
|      o S    o  o|
|                .|
|                 |
|                 |
|                 |
+----[SHA256]-----+
```

Lo anterior ha creado un par de llaves:
- `id_dsa` es la llave privada que debe mantenerse de manera segura y nunca debe ser compartida
- `id_dsa.pub` es la llave publica cuyo propósito es ser distribuida como sea necesario

```bash
-> ls -l $HOME/.ssh
total 20
-rw------- 1 user1 user1 1393 Jul 13 19:57 id_dsa
-rw-r--r-- 1 user1 user1  615 Jul 13 19:57 id_dsa.pub
```

## Distribuir Llaves

Presumiblemente creamos llaves de SSH porque intentamos usarlas. Una vez que hemos creado una par de llaves, el proximo paso lógico es distribuir la llave pública. Esto implica que debemos enviar, or copiar la _*llave pública*_ del par que hemos creado. Hay dos maneras de hacer esto:
1. Editar el archivo `authorized_keys` en el servidor remoto para agregar la llave
2. Usar el comando `ssh-copy-id` para transmitir la llave a travez de la red 

{: .warning }
La llave privada nunca debe distribuirse. Debe mantenerse de manera segura.

### Agregar Llave SSH Editando el Archivo `authorized_keys`

En este ejemplo deseamos copiar la llave pública RSA de un sistema Ubuntu (origen) a un sistema RedHat (destino).

Al usar este método, primera mostremos la llave que queremos copiar en el sistema de origin.

En el sistema Ubunu usamos `cat` para mostrar el contenido de la llave.

```bash
devuser@ubuntu2204-1-devesp
~
hist:186 -> cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCG/xHRR59b/Ny7iL9zdd/D0sBWXD3ln13mVea5rUHp2LnQ75cdTX1DUvt7nzvifPMhzg803M4XyioDNEpDghOY9siXCXvOxodMyRHGzSJfvF/Mn5zGg2MUaXxHr+ZEMlbiK8MOu0CL50QJ2wBcY3qk+c9aH8ai7p/PYjRVcRUxgAK3zASpdU/imR/LPWLlLnbDFORpIe4p+WZUg7qQnq9wM8EBocnOExT+Y3W126/AxQGVw7fAKEUHBdQdjqfQgMo5+JsW10O4KZMS+c1Yl/qkHt45MAHh3bcPVJdVNMtcXecWHneeG82frBfJiXw6E662qE4BQJLANln9Nc09hfOLrZhtR79nF5MpmRQ0MChs+Izih75+M/TV0jk9B5kLM9V7rbzp+Hu5orGz6fMyy/G+LjG0ubF/RdYba9z7q/Wvy2utQq9hhSNkY+IfucEjARmLdkDRGoemuMOwP11PfRcWdtTgIH+/pVy9tEwOtw2HdJ+l+06pBuJ8rLJ5euomYk8= devuser@ubuntu2204test
```

En el sistema remoto de RedHat, usamos el comando `vi` para editar el archivo ` ~/.ssh/authorized_keys`.

```bash
devuser@rhel9-1-devesp
~
hist:43 -> vi ~/.ssh/authorized_keys
```

Luego inspeccionamos el contenido del archivo con el comando `cat`.

```bash
devuser@rhel9-1-devesp
~
hist:44 -> cat  ~/.ssh/authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCG/xHRR59b/Ny7iL9zdd/D0sBWXD3ln13mVea5rUHp2LnQ75cdTX1DUvt7nzvifPMhzg803M4XyioDNEpDghOY9siXCXvOxodMyRHGzSJfvF/Mn5zGg2MUaXxHr+ZEMlbiK8MOu0CL50QJ2wBcY3qk+c9aH8ai7p/PYjRVcRUxgAK3zASpdU/imR/LPWLlLnbDFORpIe4p+WZUg7qQnq9wM8EBocnOExT+Y3W126/AxQGVw7fAKEUHBdQdjqfQgMo5+JsW10O4KZMS+c1Yl/qkHt45MAHh3bcPVJdVNMtcXecWHneeG82frBfJiXw6E662qE4BQJLANln9Nc09hfOLrZhtR79nF5MpmRQ0MChs+Izih75+M/TV0jk9B5kLM9V7rbzp+Hu5orGz6fMyy/G+LjG0ubF/RdYba9z7q/Wvy2utQq9hhSNkY+IfucEjARmLdkDRGoemuMOwP11PfRcWdtTgIH+/pVy9tEwOtw2HdJ+l+06pBuJ8rLJ5euomYk8= devuser@ubuntu2204test
```
La llave que agregamos se ve al final de la salida.

### Agregar Llave SSH Usando `ssh-copy-id`

En este ejemplo deseamos copiar la llave pública RSA de un sistema Ubuntu (origen) a un sistema RedHat (destino).

Usamos el comando `ssh-copy-id` para copiar la llave pública del sistema origen al sistema de destino.<br>
El comando  `ssh-copy-id` requiere que proveamos la contraseña del usuario en el sistema remoto.

```bash
devuser@ubuntu2204-1-devesp
~
hist:189 -> ssh-copy-id rhel9-1-devesp
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/devuser/.ssh/id_rsa.pub"
The authenticity of host 'rhel9-1-devesp (172.44.4.2)' can't be established.
ED25519 key fingerprint is SHA256:4JJ+Zavlqkp4mZsjCV2gN9G4a0zAoBdXHPp9eQBJlGU.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
devuser@rhel9-1-devesp's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'rhel9-1-devesp'"
and check to make sure that only the key(s) you wanted were added.
```
La salida del comando indica que copió una llave. <br>
También sugiere la manera de conectarse al sistema remoto.<br>
Por último, recomienda verificar que la llave fué copiada.

Ahora, en el sistema remoto usamos el comando `cat` para inspeccionar el contenido del archivo `authorized_keys` para verificar que la llave se transmitió.

```bash
devuser@rhel9-1-devesp
~
hist:46 -> cat ~/.ssh/authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCG/xHRR59b/Ny7iL9zdd/D0sBWXD3ln13mVea5rUHp2LnQ75cdTX1DUvt7nzvifPMhzg803M4XyioDNEpDghOY9siXCXvOxodMyRHGzSJfvF/Mn5zGg2MUaXxHr+ZEMlbiK8MOu0CL50QJ2wBcY3qk+c9aH8ai7p/PYjRVcRUxgAK3zASpdU/imR/LPWLlLnbDFORpIe4p+WZUg7qQnq9wM8EBocnOExT+Y3W126/AxQGVw7fAKEUHBdQdjqfQgMo5+JsW10O4KZMS+c1Yl/qkHt45MAHh3bcPVJdVNMtcXecWHneeG82frBfJiXw6E662qE4BQJLANln9Nc09hfOLrZhtR79nF5MpmRQ0MChs+Izih75+M/TV0jk9B5kLM9V7rbzp+Hu5orGz6fMyy/G+LjG0ubF/RdYba9z7q/Wvy2utQq9hhSNkY+IfucEjARmLdkDRGoemuMOwP11PfRcWdtTgIH+/pVy9tEwOtw2HdJ+l+06pBuJ8rLJ5euomYk8= devuser@ubuntu2204test
```

## Conclusion

El conocimiento de uso de llaves de SSH va mas alla de facilitar entrar a un sistema de manera puramente conveniente. La idea principal del uso de llaves de SSH es evitar el uso de contraseñas. Esto presenta una verdadera ventaja durante la implementación de tareas de automatización que no requiere intervencion del usuario. 

## Referencias 

### Glosario De Comandos

Los comandos siguientes son usados frecuentemente en sesiones de Linux.

ssh-keygen
: Utilidad de clave de autenticación OpenSSH. Se usa para crear llaves de SSH.

ssh-copy-id
: utilizar claves disponibles localmente para autorizar inicios de sesión en una máquina remota

### Referencias Utiles

DevEsp :: Linux
- https://docs.devesp.com/linux-en-espa%C3%B1ol/

Paginas Manuales
- [ssh-keygen](https://manpages.ubuntu.com/manpages/focal/en/man1/ssh-keygen.1.html)
- [ssh-copy-id](https://manpages.ubuntu.com/manpages/focal/en/man1/ssh-copy-id.1.html)
