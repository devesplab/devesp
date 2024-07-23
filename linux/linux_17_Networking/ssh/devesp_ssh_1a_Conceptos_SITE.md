---
layout: default
title: Conceptos de SSH
permalink: /conceptos-ssh/
parent: Red y Conectividad
grand_parent: Linux
has_children: false
has_toc: false
nav_order: 1
---

# LINUX :: SSH :: Conceptos

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
- Qué es SSH?
- tema dos
- tema tres

**DEPENDENCIAS**

ninguna

**REQUERIMIENTOS**

Sistema de linux Ubuntu. <br>
Alguos comandos requieren privilegios elevados.

**ADVERTENCIA**

ninguna.

## Working Environment

En esta leccion usamos el sistema operativo Ubuntu.

## Qué es SSH?

OpenSSH es la principal herramienta de conectividad para inicio de sesiones remotas con el protocolo SSH. Cifra todo el tráfico para eliminar escuchas ilegales, secuestro de conexiones y otros ataques. Además, OpenSSH proporciona un gran conjunto de capacidades de tunelización segura, varios métodos de autenticación y opciones de configuración sofisticadas.

La suite OpenSSH consta de las siguientes herramientas:

- Las operaciones remotas se realizan mediante ssh, scp y sftp.
- Gestión de claves con ssh-add, ssh-keysign, ssh-keyscan y ssh-keygen.
- El lado del servicio consta de sshd, sftp-server y ssh-agent.

> _La descripción anterior fue tomada de la página hogar del producto_ [^1]

## Carácteristicas de SSH

Ofrece modo de cifrado segura.

Permite el uso de llaves multiples.

Integracion on herramientas tales como Ansible que permite automatización.

## Usos de SSH

Entrada segura a sistemas mediante el uso de llaves. (ssh user@host)

Automatización mediante el uso de llaves para evitar entrar contraseñas. (ssh user@host)

Transporte de dato mediate copia segura. (SCP)

## Cliente de SSH

OpenSSH provee el client `ssh` para facilitar la comunicación entre sistemas.
El cliente accepta ajustes para facilitar su uso `~/.ssh/config`, `/etc/openssh/openssh.config`

## Servicio de SSH

OpenSSH provee el daemon `sshd` para manejar el proceso que acepta conexiones que vienen de sistemas remotos.
El daemon accepta ajustes para administrar su uso `/etc/openssh/opensshd.config`

El servicio `sshd` se maneja con el comando `systemctl`. 

Ver el estatus del servicio.
```bash
systemcl status ssd
```

Parar servicio.
```bash
systemcl stop ssd
```

Empezar servicio.
```bash
systemcl start ssd
```

Ver el archivo de configuración del servicio.
```bash
systemcl cat ssd
```

## Llaves de SSH

SSH logra comunicaciones cifradas mediante el uso de llaves que crean encripcion en tránsito.

En realidad, SSH requiere el uso de un par de llaves:
- llave privada: tiene que mantenerse privada y segura a todo costo
- llave pública: puede copiarse a cualquier sistema donde se requiera conexión remota

Podemos crear llaves de varios tipos tales como: `rsa`, `rsa1`, `dsa`. 

Usamos el comando `ssh-keygen` para crear llaves. El comando crea la llave pública y la llave privada.

Por defecto ssh usa el nombre `id_rsa` o `id_dsa` para la llave privada, y usa `id_rsa.pub` o `id_dsa.pub` para la llave pública. Las llaves son guardadas en la un directorio escodido `$HOME/.ssh`, pero si asi lo deseamos podemos poner las llaves en el lugar que nos plazca.

En otro documento discutimos como usar las llaves para comunicarnos sobre la red.

## Conclusion

Es absolutamente esencial saber usar SSH en un entorno complejo de computación que requiere transmisiones seguras.

## Referencias 

### Glosario De Comandos

Los comandos siguientes son usados frecuentemente en sesiones de Linux.

ssh-keygen
: crear llaves de SSH

ssh
: client para conexiones seguras entre sistemas

systemctl
: utilidad de línea de comando para administrar servicios sin SystemD

### Referencias Utiles

DevEsp :: Linux
- https://docs.devesp.com/linux-en-espa%C3%B1ol/

Paginas Manuales
- [systemctl](https://manpages.ubuntu.com/manpages/focal/en/man1/systemctl.1.html)
- [systemd](https://manpages.ubuntu.com/manpages/focal/en/man1/systemd.1.html)

[^1]: Página hogar de [OpenSSH](https://www.openssh.com/)

