---
layout: default
title: Provisionar Cliente de Git
permalink: /provisionar-git/
parent: Git
has_children: false
has_toc: false
nav_order: 1
---

# Provisionar El Cliente De Git

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

Antes que podamos hacer operaciones de control de revision, debemos instalar el cliente de Git.

En esta leccion exploramos como instalar el cliente de git.

En esta leccion usamos el sistema operativo Ubuntu y RedHat.<br>
Usamos el cliente de Git >= 2.0

<style>.note-custom {
    background-color: #a4dded;
    color: #23297a;
    border: 2px solid black;
    margin-left: 2%;
    margin: 40px;
    padding: 10px;
    text-align: left;
}
</style>

<div class="note-custom">
  <h2>Utilidades Para Manejar Paquetes Por Sistema Operativo</h2>
    <ul> APT    : usada por Ubuntu</ul>
    <ul> YUM    : usada por RedHat</ul>
    <ul> BREW   : usada por MacOS</ul>
    <ul> WINGET : usada por Microsoft</ul>
</div>

## Instalar el Client Git en Ubuntu

Hacemos lo siguiente en `Ubuntu 22.04.2 LTS`.

Veamos si el paquete esta disponible.
```bash
hist:13 -> sudo apt show  git -a
Package: git
Version: 1:2.34.1-1ubuntu1.9
(...snip...)
```

Instalar el paquete.
```bash
-> sudo apt install  git
```

Verificar la instalacion.
```bash
-> dpkg --get-selections | grep git
git               install
git-man           install
```

Verificar el binario
```bash
-> which git
/usr/bin/git

-> git --version
git version 2.34.1
```

Si por alguna razón es necesario, podemos desinstalar git en Ubuntu.
```bash
-> sudo apt remove  git -y
```

## Instalar el cliente Git en RedHat

Hacemos lo siguiente en `Red Hat Enterprise Linux 9.2 (Plow)`.

Ver si el paquete esta disponible.

Podemos usar la bandera `provides` o `list` como se muestra a continuación.
```bash
-> yum provides git
Not root, Subscription Management repositories not updated
Last metadata expiration check: 0:01:18 ago on Mon May 27 20:03:13 2024.
git-2.43.0-1.el9.x86_64 : Fast Version Control System
Repo        : ubi-9-appstream-rpms
Matched from:
Provide    : git = 2.43.0-1.el9

-> yum list git
Not root, Subscription Management repositories not updated
Last metadata expiration check: 0:01:34 ago on Mon May 27 20:03:13 2024.
Available Packages
git.x86_64                           2.43.0-1.el9                           ubi-9-appstream-rpms
```

Instalar el paquete.
```bash
-> sudo yum install git
```

Para verificar la instalacion podemos usar el comando `yum` o `rpm` como se muestra a continuación.
```bash
-> yum list git
Not root, Subscription Management repositories not updated
Last metadata expiration check: 0:10:56 ago on Mon May 27 20:03:13 2024.
Installed Packages
git.x86_64

hist:25 ->  rpm -qa | grep git
crypto-policies-20221215-1.git9a18988.el9.noarch
crypto-policies-scripts-20221215-1.git9a18988.el9.noarch
crontabs-1.11-27.20190603git.el9_0.noarch
net-tools-2.0-0.62.20160912git.el9.x86_64
git-core-2.43.0-1.el9.x86_64
git-core-doc-2.43.0-1.el9.noarch
git-2.43.0-1.el9.x86_64
```

Verificar el binario
```bash
-> which git
/usr/bin/git

-> git --version
git version 2.43.0
```

Si por alguna razón es necesario, podemos desinstalar git en RedHat.
```bash
-> yum remove git
```

## Instalar Git en MacOS

Instalar git using `Homebrew`.
```bash
-> brew install git
```

{: .note }
Ver la sección de referencias para el enlace donde obtener `brew`.

Verificar el binario
```bash
-> which git
/usr/bin/git

-> git --version
git version 2.39.2 (Apple Git-143)
```

## Instalar Git en Windows

En Windows debemos bajar el archivo ejecutable (.exe) y correrlo en la computadora deseada. Lo unico que hay que tener en mente es el tipo de arquitectura: 32bit o 64bits.

Otra manera fácil de instalar el cliente git en Windows es usar el `Escritorio de Github`que puede bajarse en linea. 

{: .note }
Ver la sección de referencias para el enlace donde obtener el `Escritorio de Github`.

También existe la utilidad `winget` que puede usarse asi:
```bash
winget install --id Git.Git -e --source winget
```

## Conclusion

EL cliente de Github conocido como `git` es una utilidad de uso universal disponible en los Sistemas Operativos modernos. Esta utilidad es altamente desarrollada y actualizada y puede usarse con confianza sabiendo que tiene el soporte de una gran comunidad de usuarios.

## Referencias 

### Glosario De Comandos

Los comandos siguientes son usados frecuentemente en sesiones de Linux.

yum
: utilidad para instaler paquetes en RedHat

apt
: utilidad para instaler paquetes en Ubuntu

brew
: utilidad para administrar paquetes en MacOS

winget
: utilidad para administrar paquetes en computadoras basadas en el Sistema Operativo Microsoft 

### Referencias Utiles

DevEsp 
- [Ubuntu Manejar Paquetes](../linux/linux_14_Package_Management/devesp_packages_14a_ubuntu_package_management.md)
- [RedHat Manejar Paquetes](../linux/linux_14_Package_Management/devesp_packages_14b_rhel_package_management.md)


Obtener el cliente de Git
- [Git Linux Cliente](https://git-scm.com/download/linux)
- [Git MacOS Cliente](https://git-scm.com/download/mac/)
- [Git Windows Cliente](https://git-scm.com/download/win)
- [Escritorio de Github](https://desktop.github.com/)

Utilidades Para Adminstrar Paquetes

- [Apt](https://help.ubuntu.com/community/AptGet/Howto) para Ubuntu
- [Yum](https://access.redhat.com/solutions/9934) para RedHat
- [Homebrew](https://brew.sh/) para MacOS
- [Winget](https://learn.microsoft.com/en-us/windows/package-manager/winget/) para Windows

[Return to main page]({{site.baseurl}}/).