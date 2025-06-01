---
layout: default
title: Paquetes en RedHat
permalink: /rhel-package-management/
parent: Manejando Paquetes
grand_parent: Linux
has_children: false
has_toc: false
nav_order: 20
---

# Manejo de Paquetes en RedHat (RHEL)
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

## Identificando La Versión De Sistema Operativo

Primero que nada debemore saber la version del sistema operativo antes de instalar o actualizar paquetes.

RedHat ofrece el comando `lsb_release` que principalmente muestra el distribuidor y la versión de lanzamiento

Instalar el paquete si no esta presente

```bash
devuser@rhel9-1-devesp
hist:49 -> sudo yum install  lsb_release
```

Luego usamos el comando `lsb_release` para ver la revision corriente del sistema operativo.
```bash
devuser@rhel9-1-devesp
hist:50 -> lsb_release -a
LSB Version:	n/a
Distributor ID:	RedHatEnterprise
Description:	Red Hat Enterprise Linux 9.2 (Plow)
Release:	9.2
Codename:	n/a
```

Arriba vemos que estamos en `Red Hat Enterprise Linux 9.2`.

## Utilidades Para Manejar Paquetes en RedHat

RedHat ofrece tres utilidades para manejar paquetes: rpm, yum y dnf.

Yum es la utilidad que se usa en versiones viejas de CentOS y Fedora. 

Rehat reemplazó YUM con DNF.

Y nos preguntaremos, debemos usar YUM o DNF? 

{: .note }
DNF es la utilidad predeterminada para manejar paquetes en RedHat. 

Las respuesta es DNF porque tiene mejoras que le permiten resolver dependendcias mas rápidamente. DNF es muy similar a YUM, pero DNF tiene características que puden ser muy útilies al escribir programas de mantenimiento de sistema. Otra cosa es que DNF mantiene la historia de despliege que permite revisar y hasta retornar a la versión previa de un paquete en caso de problemas de compatibilidad. 

## Referencias 

### Glosario De Comandos

lsb_release
: mostrar ls información de distribución específica

### Referencias Utiles

Man Pages

- [lsb_release](https://manpages.ubuntu.com/manpages/focal/en/man1/lsb_release.1.html)

[Return to main page]({{site.baseurl}}/).