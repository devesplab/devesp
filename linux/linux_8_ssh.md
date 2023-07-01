---
layout: default
title: SSH
permalink: /ssh-linux/
parent: Linux
has_children: true
has_toc: false
nav_order: 8
---

# Introducción a SSH
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
## Que es SSH?

SSH (Secure SHell) es un programa que sirve para entrar a sistemas remotos. Tambien se usa para ejecutar comandos en forma remota.

SSH es un protocolo seguro que ofrece comunicaciones cifradas y reemplaza a otros protocolos menos seguros tales como RSH o TELNET.

Esta utilidad usa el protocolo TCP, y por defecto usa el puerto 22. 

## Cliente Y Servidor

El paquete en RHEL y CentOS se llama OpenSSH y ofrecen el cliente y servidor.

Generalmente, el paquete de servidor solo se instal en sistemas de computadoras a las que se espera connectar remotament via usuario o aplicacion.

El paquete de cliente se instala en computadoras de usuarios que desean connectarse a servidores a los que tienen accesso.

[Return to main page]({{site.baseurl}}/).