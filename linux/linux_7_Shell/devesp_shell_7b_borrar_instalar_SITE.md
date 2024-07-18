---
layout: default
title: Instalar Un Shell
permalink: /manejando-usuarios/
parent: El Shell
grand_parent: Linux
has_children: true
has_toc: false
nav_order: 1
---

# LINUX :: SHELL :: Instalar Un Shell

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

**DESCRIPCION**

En esta leccion:
- borrar o installer un shell

**DEPENDENCIAS**

ninguna

**REQUERIMIENTOS**

Sistema de linux Ubuntu. <br>
Alguos comandos requieren privilegios elevados.

**ADVERTENCIA**

ninguna.

## Working Environment

En esta leccion usamos el sistema operativo Ubuntu.

## Install BASH from Ubuntu

To install Bash (Bourne Again SHell) in Ubuntu, follow these steps:

Open a terminal window by pressing `Ctrl + Alt + T`.
Update the package list by running the command:

```     
sudo apt update
```

Install Bash by running the following command:

```
sudo apt install bash
```

Once the installation is complete, you can start using Bash by typing bash in the terminal.

That's it! You have successfully installed Bash on your Ubuntu system.

## Remove BASH from Ubuntu

To remove Bash from Ubuntu, you would need to install an alternative shell such as Zsh or Fish, and then set it as the default shell. Here is a general outline of the steps involved:

Install the alternative shell (e.g., Zsh):

``` 
sudo apt-get update
sudo apt-get install zsh
```

Set the alternative shell as the default:

```
chsh -s $(which zsh)
```
Log out and log back in to apply the changes.

After completing these steps, Bash will no longer be the default shell in Ubuntu, and you will be using the alternative shell that you installed (e.g., Zsh).

## Install BASH from RedHat

To install bash on Red Hat Enterprise Linux, you can use the yum package manager. Simply open a terminal and run the following command:

``` 
sudo yum install bash
```

Once the installation is complete, you can start using the bash shell by typing bash in the terminal.

## Remove BASH from RedHat

To remove bash from a Redhat system, you would need to replace it with another shell such as zsh or fish. However, it is not recommended to completely remove bash as it is the default shell for many scripts and system functions on Redhat.

If you still want to proceed with removing bash, you can do so by using the following steps:

    Switch to another shell such as zsh or fish. You can do this by running the following command:

 
chsh -s /bin/zsh

Replace /bin/zsh with the path to the shell you want to switch to.

    Make sure the new shell is installed on your system. You can install zsh or fish using the package manager, for example:

 
sudo yum install zsh

    Remove bash from your system by running the following command:

 
sudo yum remove bash

Please note that this action may break your system as many scripts and system functions rely on bash. Proceed with caution and make sure you have a backup plan in case something goes wrong.

aaa

## Tipos de Usuarios

| Tipo De Usuario      | Esfera De Acción |
| ---------------------| ---------------- |
| Usuario Regular      | Reducido al directorio Hogar       |
| Root (Super Usuario) | Acceso complete al sistem        | 
| Cuenta de Sistema    | Acceso a un proceo o aplicación        | 

## Conclusion

aaa

## Referencias 

### Glosario De Comandos

Los comandos siguientes son usados frecuentemente en sesiones de Linux.

comando1
: definicion

comando1
: definicion

### Referencias Utiles

DevEsp :: Linux
- https://docs.devesp.com/linux-en-espa%C3%B1ol/

Paginas Manuales

