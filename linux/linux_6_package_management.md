---
layout: default
title: Manejando Paquetes
permalink: /manejar-paquetes-linux/
parent: Linux
has_children: true
has_toc: false
nav_order: 6
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

Check whether a package is installed.
```
[root@rhel9-1 /]# rpm -qa | grep pam
pam-1.5.1-14.el9.x86_64
systemd-pam-252-14.el9_2.1.x86_64
```

Show complete list of files for an rpm package
```
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

Show config files for the app
```
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

Shows libraries files.
```
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

info for an rpm
```
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