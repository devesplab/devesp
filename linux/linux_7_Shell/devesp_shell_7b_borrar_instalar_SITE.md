---
layout: default
title: Instalar Un Shell
permalink: /instalar-shell/
parent: El Shell
grand_parent: Linux
has_children: false
has_toc: false
nav_order: 1
---

# Instalar Un Shell

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

## Install BASH en Ubuntu

Para instaler Bash (Bourne Again SHell)  en Ubuntu, has lo siguiente:

Para instalar Bash en Ubuntu, podemos usar APT [^1]. Abramos la terminal y corramos los comando siguientes:

[^1]:[Ubuntu APT](../linux_14_Package_Management/devesp_packages_14a_ubuntu_package_management.md )

Actualizemos la lista de paquetes.

```bash     
sudo apt update
```

Instalemos Bash con este comando.

```bash
sudo apt install bash
```

Es todo! Has instalado Bash en tu sistema.

Una vez que la instalación termine, puedes empezar a usar Bash entrando comandos en la terminal.


## Borrar BASH en Ubuntu

Para borrar Bash en Ubuntu, tenemos que instalar un Shell alternativo tal como ZSH o FISH, y luego specificarly como shell predeterminado. En seguida veamos el flujo de trabajo:

Instalar el shell alternativo (e.g., Zsh):

``` 
sudo apt-get update
sudo apt-get install zsh
```

Ajustar el shell predeterminado  ZSH.

```bash
chsh -s $(which zsh)
```

Para aplicar el cambio, salgamos del sistema y entremos de nuevo.

Después de completar los pasos anteriores, Bash no sera mas el shell predeterminado en Ubuntu y estaremos usado el shell que instalamos.

## Instalar BASH en RedHat

Para instalar Ban en RHEL (Red Hat Enterprise Linux), podemos usar YUM [^2]. Simplemente abramos la terminal y corramos el comando siguiente:

[^2]:[RedHat YUM](../linux_14_Package_Management/devesp_packages_14b_rhel_package_management.md)

```bash 
sudo yum install bash
```

Cuando la instalatción termine, podemos empezar a usar el Bash shell entrando comandos en el indicador.

## Borrar BASH en RedHat

Para poder borrar Bash en RedHat, tendriamos que reponerlo con otro shell tal come ZSH o FISN. Sin embargo, no se recomienda borrar completamente Bash porque es el shell predeterminado para mucho programas y funciones en Redhat.

Pero, si todavia insistimos con borrar Bash, podemos ejecutar los pasos que siguen.

Cambiemos a otro shell, por ejemplo ZHH.
```bash 
chsh -s /bin/zsh
```

Repongamos `/bin/bash` con el paso del shell al que queremos cambiar with the path to the shell you want to switch to.
Primero aseguremonos que el nuevo shell esta instalado.

Corramos ese comando para instalar ZSH en RedHat.
```bash 
sudo yum install zsh
```

Borremos Bash del sistema:
```bash 
sudo yum remove bash
```

{: .warning }
Tenga en cuenta que esta acción puede dañar su sistema, ya que muchos scripts y funciones del sistema dependen de bash. Proceda con precaución y asegúrese de tener un plan de respaldo en caso de que algo salga mal.

## Conclusion

Para la gran mayoría de tareas en Linux, el Bash shell es suficiente. Cambiar a otro shell es cosa de preferencia o alguna funcion especializada que requiere característcas disponible en un shell específico. Debe observarse mucho cuidado si la intencion es cambiar el shell predeterminado.

## Referencias 

### Glosario De Comandos

Los comandos siguientes son usados frecuentemente en sesiones de Linux.

apt-get, apt
: utilidad para manejar paquetes en Ubuntu

yum
: utilidad para manejar paquetes en RedHat

chsh
: comando para cambiar de un shell a otro

### Referencias Utiles

Paginas Manuales

- [chsh](https://manpages.ubuntu.com/manpages/focal/en/man1/chsh.1.html)