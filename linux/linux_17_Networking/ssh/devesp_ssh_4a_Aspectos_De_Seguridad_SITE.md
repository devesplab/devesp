---
layout: default
title: Aspectos de Seguridad en SSH
permalink: /seguridad-ssh/
parent: Red y Conectividad
grand_parent: Linux
has_children: false
has_toc: false
nav_order: 4
---

# LINUX :: SSH :: Aspectos De Seguridad

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
- SSH Passphrase
- Propósito del Randomart

**DEPENDENCIAS**

ninguna

**REQUERIMIENTOS**

Sistema de linux Ubuntu. <br>
Alguos comandos requieren privilegios elevados.

**ADVERTENCIA**

ninguna.

## Working Environment

En esta leccion usamos el sistema operativo Ubuntu.

## SSH Passphrase

## Propósito de la Imagen Aleatoria (randomart)

```bash
Sat 2024Jul13 23:12:37 UTC
devuser@ubuntu2204-1-devesp
~
hist:191 -> ssh  -o VisualHostKey=yes  rhel9-1-devesp
Host key fingerprint is SHA256:4JJ+Zavlqkp4mZsjCV2gN9G4a0zAoBdXHPp9eQBJlGU
+--[ED25519 256]--+
|+ .o.oo+++E      |
|o.+o... oo       |
|.o.+. .   .      |
|..= .+ o   o     |
| = +o o S o .    |
|..=+ . o o .     |
|oo= . . o        |
|oo.o . +         |
| .+o..o..        |
+----[SHA256]-----+
Last login: Sat Jul 13 23:07:06 2024 from 172.44.0.3
```

## Conclusion

Los aspectos de seguridad alrededor de SSH son extremadamente importantes. Es crucial que las llaves nunca sean comprometidas puesto que si esto sucede es como dar las llaves del reino.

## Referencias 

### Glosario De Comandos

Los comandos siguientes son usados frecuentemente en sesiones de Linux.

ssh
: client para conexiones seguras entre sistemas

### Referencias Utiles

DevEsp :: Linux
- https://docs.devesp.com/linux-en-espa%C3%B1ol/
- [The drunken bishop](http://www.dirk-loss.de/sshvis/drunken_bishop.pdf): An analysis of the OpenSSH fingerprint visualization algorithm

Always being asked for passphrase
[https://superuser.com/questions/722742/always-being-asked-for-passphrase/722751#722751]

What is randomart produced by ssh-keygen?
[https://superuser.com/questions/22535/what-is-randomart-produced-by-ssh-keygen]

What's the purpose of the randomart image for user (not host) SSH keys?
[https://unix.stackexchange.com/questions/144702/whats-the-purpose-of-the-randomart-image-for-user-not-host-ssh-keys]

Getting started with systemctl
[https://www.redhat.com/sysadmin/getting-started-systemctl]

Paginas Manuales
- [ssh](https://manpages.ubuntu.com/manpages/focal/en/man1/ssh.1.html)
