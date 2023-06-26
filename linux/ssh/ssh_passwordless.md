---
layout: default
title: SSH sin Contraseña
permalink: /ssh-sin-contrasena/
parent: SSH
grand_parent: Linux
has_children: false
has_toc: false
nav_order: 3
---

# SSH sin Contraseña
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

## Porque necesitamos esto?

En un ambiente de trabajo es casi una certedad que como ingenerio de devops tendremos que conectarnos a servidores en localidades remotas a las que no tenenmos acceso físico. Esto requiere de algun mecanimo para acceder tales servidores de una manera segura. 

En este articulo veremos una discusión breve del uso de llaves de OpenSSH que sirven como método de autenticación entre servidores remotos.

Supongamos que el usuario `devuser` se quiere conectar de client2 and client3
```bash
client1 ---> client3
```
En client1 vamos a crear llaves RSA en OpenSSH y copiar la llave a client3.

Veamos pues los pasos para permitir ssh sin contraseña entre servidores remotos.

## Crear el directorio .ssh

Entremos a cada sistema con el usuario `devuser`
```bash
Sun 2023Jun25 17:52:02 PDT
root@client1 [ CentOS8 ]
~
hist:330 -> su - devuser
Last login: Sun Jun 25 17:47:06 PDT 2023 on pts/1
```
Notese que estamos en el directorio de inicio denotado per el simbolo `~` en el prompt.

Crear el directorio `.ssh` y dar el permiso mostrado.
```bash
-> mkdir ~/.ssh
-> chmod 700 ~/.ssh
```
Verifiquemos
```bash
-> ls -ld .ssh
drwx------ 2 devuser devuser 4096 Jun 18 18:31 .ssh/
```

Crear el fichero `authorized_keys` con el permiso mostrado
```bash
-> touch ~/.ssh/authorized_keys
-> chmod 600  ~/.ssh/authorized_keys
```
Verifiquemos.
```bash
-> ls -l .ssh/authorized_keys
-rw------- 1 devuser devuser 569 Jun 18 18:31 .ssh/authorized_keys
```

## Generar la llave RSA

Generar la llave aceptando todos los valores per defecto.<br>
Usamos el comando `ssh-keygen` con la opción `-t` para specificar que deseamos una llave de tipo `RSA`.
```bash
Sun 2023Jun25 18:07:42 PDT
devuser@client1 [ CentOS8 ]
~
hist:127 -> ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/devuser/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/devuser/.ssh/id_rsa.
Your public key has been saved in /home/devuser/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:8jllUzSbYoOa+LghFo8JJH9UpJ+WuXkSQDpzJIP6xa4 devuser@client1
The key's randomart image is:
+---[RSA 3072]----+
| .o o.o     o    |
|.  * o   . . +   |
|o.+.=   . + +    |
|+. =o+ * . +     |
|..ooo @ S +      |
| ..*.+ * + .     |
|  =.+ = =        |
| .E. o o .       |
|    .            |
+----[SHA256]-----+
```

{: .highlight }
> El "radomart" es abreviación para "random art", o "arte aleatorio" que es para
> faciliatar la comparacion de images para validar la llave de un sistema a otro.

La operación anterior ha creado dos ficheros en el directorio`~/.ssh`:
- la llave **privada**: /home/devuser/.ssh/id_rsa
- la llave **pública**: /home/devuser/.ssh/id_rsa.pub

```bash
devuser@client1 [ CentOS8 ]
~
hist:128 -> ls -l ~/.ssh/
total 12
-rw------- 1 devuser devuser    0 Jun 25 18:16 authorized_keys
-rw------- 1 devuser devuser 2602 Jun 25 18:07 id_rsa
-rw-r--r-- 1 devuser devuser  569 Jun 25 18:07 id_rsa.pub
-rw-r--r-- 1 devuser devuser    0 Jun 18 18:31 known_hosts
```

El fichero `~/.ssh/id_rsa.pub` contiene la llave pública. <br>
Podemos distribuir esta llave pública a cuantos servidores queramos acceder con esta llave.<br>
El comentario al final de la linea indica el servidor a la que esta llave pertenece.<br>
EL contenido entero de la llave es en una sola linea.
```bash
devuser@client1 [ CentOS8 ]
~
hist:130 -> cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC9HrNdiFpR2dewlPVkanf0WmD0zbkdpkJlzwzbKw1Pq5fOgZPWBnHNGtcon+GRMCIju0Cy3dlUjz8jIvkxd+ow7DHO/5o28S/sEKYM7384PG2P2al8WWhrIrJC82EYKe6uVuJXYW/E3bEfskqcVP82lkzgLyQ0Dd5GTcSi7qA5KEcicofM+gxl8QgyCir6SGBAXUAC4JWxsYMjn+MUBxGIc/J8dzobZmhPQMXP897/S49NvC+glP8r8jeX6AZ6JNbM+AGJ5hvntP+OrTQvJSSkOdC3OrqMO6lcZ3dbzgObqWSjDeplTP/17/UdYu/ruyo1Ys7TMqMIeWXUDoYIUsuEVP59nEwhRKVFd+h2SsUNtvxUfdrpHZG8+aWVAiGFVwrv5TUfmMpLcrcZxVObz/K+El4SsUj+p3mPd7gTJg+Umj3Mvn52KD9kK7cwNhrpgBVawrdEMTBmUnHDjEHhMLoqrV9Rvo07uRsu3pKZQpL6UD/3nO2jHNwGrg0S5nTZ4ck= devuser@client1
```

Copiemos la llave publica a un sistema usando el comando `ssh-copy-id` y pasando como argumento el servidor de destino.
```
-> ssh-copy-id client3
```

En seguida podemos usar el comando `ssh` para entrar al sistema remoto.
```
-> ssh client3
```

## Comandos remotos de ssh

Igualmente podemos hacer otras operaciones que pueden ser ejecutadas directamente en el servidor remoto.

Ejecutar un comando o script remotamente en el sistem.
```
-> ssh client3 "Hola, soy el usuario devuse!"

-> ssh client3 uname -a

-> ssh client3 /usr/local/bin/myscript.sh
```

Copiar algo al sistema remote
```
-> scp directory1 fichero1 client3:/tmp
```