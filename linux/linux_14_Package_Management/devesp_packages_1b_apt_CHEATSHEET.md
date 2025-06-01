---
layout: default
title: Trucos Tecnicos de APT
permalink: /apt_cheatsheet/
parent: Manejando Paquetes
grand_parent: Linux
has_children: false
has_toc: false
nav_order: 11
---

## APT Cheatcheet
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

## Actualizar Paquete Específico Con APT

En Ubuntu usamos el comando APT para actualizar paquetes.

En este ejemplo APT indica que algunos paquetes pueden ser actualizados.<br>
Tambien indica que podemos correr el comando `apt list --upgradable` para ver cuales son esos paquetes.

```sh
-> sudo apt update -y
Hit:1 https://download.docker.com/linux/ubuntu jammy InRelease
Get:2 http://security.ubuntu.com/ubuntu jammy-security InRelease [129 kB]
Hit:3 http://archive.ubuntu.com/ubuntu jammy InRelease
Get:4 http://archive.ubuntu.com/ubuntu jammy-updates InRelease [128 kB]
Hit:5 https://ppa.launchpadcontent.net/ubuntu-toolchain-r/test/ubuntu jammy InRelease
Get:6 http://archive.ubuntu.com/ubuntu jammy-backports InRelease [127 kB]
Get:7 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 Packages [3264 kB]
Get:8 http://archive.ubuntu.com/ubuntu jammy-updates/universe amd64 Packages [1553 kB]
Fetched 5201 kB in 3s (1518 kB/s)
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
3 packages can be upgraded. Run 'apt list --upgradable' to see them.
```

Al correr el comando sugerido vemos los paquetes candidatos a actualizar.

```
-> apt list --upgradable
Listing... Done
apt-utils/jammy-updates 2.4.14 amd64 [upgradable from: 2.4.9]
apt/jammy-updates 2.4.14 amd64 [upgradable from: 2.4.9]
libapt-pkg6.0/jammy-updates 2.4.14 amd64 [upgradable from: 2.4.9]
```

To upgrade just the apt package specifically, you can use the following command:

Para solo actualizar el paquete `apt` a la version mas reciente podemos hacerlo asi:

```
sudo apt-get install --only-upgrade apt
```

[Return to main page]({{site.baseurl}}/).