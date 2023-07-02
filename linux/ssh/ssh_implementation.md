---
layout: default
title: Implementación de SSH
permalink: /ssh-implementation/
parent: SSH
grand_parent: Linux
has_children: false
has_toc: false
nav_order: 2
---

# Acceso Remoto con SSH
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
## Acceso Remoto

El uso mas general de SSH is entrar a un sistema remotamente.

Eso se consigue al simplemente entrar el comando ssh seguido per el hostname del servidor que queremos acceder.

Supongamos que queremos entrar al servidor con hostname `remoteServer`.
```
ssh remoteServer
```
Normalmente, se asume que queremos entrar con el mismo number de  usuario que tenemos en el servidor de origen.
Pero si es necesario, podemos especicar el usuario en el servidor remote asi:
```
ssh devuser@remoteServer
```
Tambien podemos usar la dirección de internet.
```
ssh devuser@11.22.33.44
```

## Comandos remotos de ssh

La idea central de SSH es que podemos hacer operaciones que pueden ser ejecutadas directamente en un servidor remoto.

La sintaxis es asi:
```
ssh <hostname-de-sistema-remoto> <comando-o-script>
```

Ejecutar un comando o script remotamente en un sistem.
```
-> ssh client3 "Hola, soy el usuario devuse!"

-> ssh client3 uname -a

-> ssh client3 /usr/local/bin/myscript.sh
```

Copiar algo a un sistema remoto.
```
-> scp directory1 fichero1 client3:/tmp
```