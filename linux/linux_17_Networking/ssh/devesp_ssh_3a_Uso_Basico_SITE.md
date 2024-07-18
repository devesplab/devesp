---
layout: default
title: Uso Basico de SSH
permalink: /basico-ssh/
parent: Red y Conectividad
grand_parent: Linux
has_children: false
has_toc: false
nav_order: 3
---

# LINUX :: SSH :: Uso Básico

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

En su forma más básica SSH sirve dos propósitos:
- establecer conexiones bidireccionales de un sistem remoto
- move datos de forma bidireccional de un sistema remote

En esta leccion:
- Conexión con SSH
- Mover datos con SSH

**DEPENDENCIAS**

ninguna

**REQUERIMIENTOS**

Sistema de linux Ubuntu. <br>
Alguos comandos requieren privilegios elevados.

**ADVERTENCIA**

ninguna.

## Working Environment

En esta leccion usamos el sistema operativo Ubuntu.


## Conexión con SSH

Una vez que hemos creado y copiado una llave a un sistem remoto, podemos usar el comando ssh con esta sintaxis:

```bash
ssh usuario@<sistema-remoto>
```
O podemos excluir el nombre del usurio si el id es el mismo en los dos lados

```bash
ssh <sistema-remoto>
```
Claro que hay que sustituir `<sistema-remoto>` con el nombre actual del servidor o la ip address.

He aquí un ejemplo: nos conectamos de un sistema Ubuntu an un sistema RedHat.

```bash
devuser@ubuntu2204-1-devesp
~
hist:189 -> ssh rhel9-1-devesp
Last failed login: Sat Jul 13 22:57:50 UTC 2024 from 172.44.0.3 on ssh:notty
There was 1 failed login attempt since the last successful login.
Last login: Sat Jul 13 22:38:25 2024 from 172.46.0.4

devuser@rhel9-1-devesp
~
hist:47 -> hostnamectl
 Static hostname: rhel9-1-devesp
       Icon name: computer-container
         Chassis: container ☐
      Machine ID: 5d4a094e262746bf84e9665dd3f8a430
         Boot ID: a74ed0f59ce34aa18d96d871df8eccec
  Virtualization: container-other
Operating System: Red Hat Enterprise Linux 9.2 (Plow)
     CPE OS Name: cpe:/o:redhat:enterprise_linux:9::baseos
          Kernel: Linux 6.3.13-linuxkit
    Architecture: x86-64
```

{: .highlight }
En realidad no importa el sistema operativo Ubuntu o RedHat. SSH trabaja igual.

El ejemplo anterior demuestra que simplemente no importa si el origen o destinación son un tipo de Sistema Operativo u otro; SSH no distingue esto puesto que funciona de la misma manera en derivativas de Debian o RedHat.

## Mover datos con SSH

Una de las tareas esenciales en un entorno de computación consiste an la abilidad de mover información de manera segura entre sistemas. El comando `scp` hace justamente eso.

La sintaxis es asi:
- el comando `scp`
- `datos-a-copiar` son archivos o directorios a trasnmitir
- `usuario` es el id del usuario 
- símbolo `@` para indicar donde copiar
- `sistema-remoto` es el sistema remoto a donde mover la información
- símbolo `:` demarca la instrucción de identifcación del usuario y el destino donde mover la 

```bash
scp <datos-a-copiar> <usuario>@<sistema-remoto>:<destination>
```

La tarea de copiar puede occurir entre dos sistemas de manera bidireccional.
- de nuestro sistema local a un sistema remoto
- de un sistema remoto a nuestro sistema local

Aun más, el tipo de información altera ligeramente la sintaxis:
- copiar un solo archivo, o varios archivos
- copiar una carpeta, o grupo de carpetas
- copiar una combinación de archivos y carpetas

Enseguida vemos ejemplos de lo anteriors.

### Copiar De Local A Remoto

Copiar un archivo de nuestro sistema local a un sistema remoto con destinación del directorio hogar del usuario.<br>
> El doble punto `:` en el comando indica que la destinación es el directorio hogar del usuario.

```bash
scp archivo1 usuario@remoto:
```

Copiar un archivo a un sistema remoto especificando la destinación `/directorio/datos`.

```bash
scp archivo1 usuario@remoto:/directorio/datos
```

Copiar una carpeta a un sistema remoto especificando la destinación `/directorio/datos`.
Debemos pasar la bandera `-r` porque estamos copiando una carpeta.

```bash
scp r carpetaDatos usuario@remoto:/directorio/datos
```

{: .note }
> Podemos omitir el nombre del usuario si el nombre del usuario es el mismo en local y remoto
> ```
> scp archivo1 remoto:/directorio/datos
> ```

Copiar varios archivos y carpetas de local a remoto.

```bash
scp -r archivo* carpetaDatos carpetaInfo remoto:/directorio/datos
```

### Copiar De Remoto a Local

Para copiar de un remoto a local, simplemente revertimos el orden del ejemplo anterior.

Copiar un archivo de nuestro sistema local a un sistema remoto con destinación `$HOME`.<br>
> El doble punto `:` en el comando indica que la destinación es el directorio hogar del usuario.
```
scp usuario@remoto:archivo1 $HOME
```

Copiar el archivo desde un sistema remoto especificando la destinación `$HOME` en el sistema local.<br>
Debemos pasar la bandera `-r` porque estamos copiando una carpeta.

```bash
scp -r usuario@remoto:/carpetaDatos $HOME
```

Copiar varios archivos y carpetas de remoto a local.

```bash
scp -r remoto:/home/devuser/{carpetaDatos,carpetaInfo,archivo*} .
```
En el comando anterior usamos `{}` para agrupar una combinación de varias carpetas y archivos y copiarlos en una sola vez.

## Conclusion

Al trabajar con dos o más servidores en nuestro entorno, eventualmente no veremos con la necesidad de establecer conexiones remotas o mover datos en una dirección u otra. Por esta razón SSH provee la forma de hacer tareas a travez de la red de manera segura y eficiente. Asi que entender como usar SSH sirve un propósito escensial en ambientes complejos de computación.

## Referencias 

### Glosario De Comandos

Los comandos siguientes son usados frecuentemente en sesiones de Linux.

ssh
: utilidad que facilita conexiones remotas seguras en un entorno de computación

scp
: utilidad de SSH para copiar datos de forma segura y bidireccional 

hostnamectl
: comando que provee identificación del sistema y su sistema operativo

### Referencias Utiles

DevEsp :: Linux
- https://docs.devesp.com/linux-en-espa%C3%B1ol/

Paginas Manuales
- [ssh](https://manpages.ubuntu.com/manpages/focal/en/man1/ssh.1.html)
- [scp](https://manpages.ubuntu.com/manpages/focal/en/man1/scp.1.html)
