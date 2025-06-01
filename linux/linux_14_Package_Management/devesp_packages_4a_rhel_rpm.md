---
layout: default
title: Usando RPM en RedHat
permalink: /rhel-rpm/
parent: Manejando Paquetes
grand_parent: Linux
has_children: false
has_toc: false
nav_order: 40
---

# Manejo de Paquetes en RedHat con RPM
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

Un paquete de RPM es un archivo de `.rpm` que contiene archivos compresos y metadata que describen el paquete y sus dependencias.

La utilidad `rpm` ofrece las características básicas para manejar paquetes, pero no tiene la abilidad de resolver dependencias entre paquetes. Esta es la razón que en RedHat es mejor usar DNF puesto que tiene la abilidad de resolver dependencias.

## Usar el Comando RPM en RedHat

Instalar paquete

```bash
rpm -i package.rpm
```

Desinstalar un paquete

```bash
rpm -e package_name
```

Actualizar un paquete

```bash
rpm -U package.rpm
```

Verificar un paquete.

```bash
rpm -V package_name
```

Encontrar el paquete al que pertenence un archivo.

```bash
-> rpm -qf /bin/ls
coreutils-single-8.32-34.el9.x86_64
```


Verificar si un paquete esta instalado.

```bash
[root@rhel9-1 /]# rpm -qa | grep pam
pam-1.5.1-14.el9.x86_64
systemd-pam-252-14.el9_2.1.x86_64
```

Mostrar la lista completa de archivos perteneciente a un paquete de rpm.

```bash
[root@rhel9-1 /]# rpm -ql pam-1.5.1-14.el9.x86_64
/etc/motd.d
/etc/pam.d
/etc/pam.d/config-util
/etc/pam.d/fingerprint-auth
/etc/pam.d/other
/etc/pam.d/password-auth
/etc/pam.d/postlogin
/etc/pam.d/smartcard-auth
/etc/pam.d/system-auth
/etc/security
/etc/security/access.conf
/etc/security/chroot.conf
(...snip...)
```

Mostrar los archivos de configuración de un paquete the rpm.

```bash
[root@rhel9-1 /]# rpm -qc pam
/etc/pam.d/config-util
/etc/pam.d/fingerprint-auth
/etc/pam.d/other
/etc/pam.d/password-auth
/etc/pam.d/postlogin
/etc/pam.d/smartcard-auth
/etc/pam.d/system-auth
/etc/security/access.conf
/etc/security/chroot.conf
/etc/security/console.handlers
/etc/security/console.perms
/etc/security/faillock.conf
/etc/security/group.conf
/etc/security/limits.conf
/etc/security/namespace.conf
/etc/security/namespace.init
/etc/security/opasswd
/etc/security/pam_env.conf
/etc/security/pwhistory.conf
/etc/security/sepermit.conf
/etc/security/time.conf
```

Mostrar las librerias pertenecientes a un paquete the rpm.

```bash
[root@rhel9-1 /]# rpm -qR pam
/usr/bin/sh
audit-libs >= 1.0.8
config(pam) = 1.5.1-14.el9
glibc >= 2.3.90-37
ld-linux-x86-64.so.2()(64bit)
ld-linux-x86-64.so.2(GLIBC_2.3)(64bit)
libaudit.so.1()(64bit)
libc.so.6()(64bit)
libc.so.6(GLIBC_2.14)(64bit)
(...snip...)
```

Mostrar toda la información de un paquete the rpm.

```bash
[root@rhel9-1 /]# ls -l /etc/DIR_COLORS
-rw-r--r-- 1 root root 4673 Jan  6 11:48 /etc/DIR_COLORS

[root@rhel9-1 /]# rpm -qi pam
Name        : pam
Version     : 1.5.1
Release     : 14.el9
Architecture: x86_64
Install Date: Thu Jun 15 01:44:19 2023
Group       : Unspecified
Size        : 1898193
License     : BSD and GPLv2+
Signature   : RSA/SHA256, Tue Nov 29 19:16:00 2022, Key ID 199e2f91fd431d51
Source RPM  : pam-1.5.1-14.el9.src.rpm
Build Date  : Tue Nov 29 12:33:58 2022
Build Host  : x86-64-01.build.eng.rdu2.redhat.com
Packager    : Red Hat, Inc. <http://bugzilla.redhat.com/bugzilla>
Vendor      : Red Hat, Inc.
URL         : http://www.linux-pam.org/
Summary     : An extensible library which provides authentication for applications
Description :
PAM (Pluggable Authentication Modules) is a system security tool that
allows system administrators to set authentication policy without
having to recompile programs that handle authentication.
```

## Referencias 

Manuales en linea
- [Manejo de Paquetes en RedHat](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/4/html/system_administration_guide/package_management)

### Referencias Utiles

Man Pages
- [rpm](https://man7.org/linux/man-pages/man8/rpm.8.html)


[Return to main page]({{site.baseurl}}/).