---
layout: default
title: Manejando Paquetes
permalink: /manejar-paquetes-linux/
parent: Linux
has_children: true
has_toc: false
nav_order: 5
---

# Introducción a Manejo De Paquetes
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
## See RPM files

Show complete list of files for an rpm package
```
hist:56 -> rpm -ql pam-1.3.1-15.el8.x86_64
/etc/pam.d
/etc/pam.d/config-util
/etc/pam.d/fingerprint-auth
/etc/pam.d/other
/etc/pam.d/password-auth
/etc/pam.d/postlogin
/etc/pam.d/smartcard-auth
```

Show config files for the app
```
hist:85 -> rpm -qc pam
/etc/pam.d/config-util
/etc/pam.d/fingerprint-auth
/etc/pam.d/other
/etc/pam.d/password-auth
/etc/pam.d/postlogin
/etc/pam.d/smartcard-auth
/etc/pam.d/system-auth
/etc/security/access.conf
```

Shows libraries files.
```
 -> rpm -qR pam
/bin/sh
/sbin/ldconfig
/sbin/ldconfig
/sbin/ldconfig
audit-libs >= 1.0.8
config(pam) = 1.3.1-15.el8
coreutils
glibc >= 2.3.90-37
libaudit.so.1()(64bit)
```

info for an rpm
```
->  rpm -qi pam
Name        : pam
Version     : 1.3.1
Release     : 15.el8
Architecture: x86_64
Install Date: Sun Jun 18 18:28:52 2023
Group       : System Environment/Base
Size        : 2627190
License     : BSD and GPLv2+
Signature   : RSA/SHA256, Mon May 10 07:14:17 2021, Key ID 05b555b38483c65d
Source RPM  : pam-1.3.1-15.el8.src.rpm
Build Date  : Thu May  6 22:07:13 2021
Build Host  : x86-02.mbox.centos.org
Relocations : (not relocatable)
Packager    : CentOS Buildsys <bugs@centos.org>
Vendor      : CentOS
URL         : http://www.linux-pam.org/
Summary     : An extensible library which provides authentication for applications
Description :
PAM (Pluggable Authentication Modules) is a system security tool that
allows system administrators to set authentication policy without
having to recompile programs that handle authentication.
```

This is the first section of the document.
## Second Subtitle

Second section of the document.

## Third Subtitle

And this third section of the document.

## Fourth Subtitle
 
Some interesting examples follow.


{: .note }
Currently, the navigation structure is limited to 3 levels: grandchild pages cannot themselves have child pages.

[Return to main page]({{site.baseurl}}/).