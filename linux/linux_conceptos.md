---
layout: default
title: Conceptos
permalink: /linux_conceptos/
parent: Linux
has_toc: false
nav_order: 1
---

# Conceptos de Introducción
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

## Estructura del Sistema De Archivos (Filesystem)

La estructura general del sistema de archivos de Linux puede representarse the esta manera.
```

                                          ┌──────────────┐
                                          │   root       │
         ┌───────────────────┬────────────┴───────┬──────┴───────────┬──────────────────┐
         │                   │                    │                  │                  │
  ┌──────▼────┐      ┌───────▼──────┐     ┌───────▼──────┐    ┌──────▼──────┐    ┌──────▼─────┐
  │    /home  │      │    /usr      │     │    /var      │    │    /opt     │    │   /mnt     │
  └───────────┘      └──────────────┘     └──────────────┘    └─────────────┘    └────────────┘
```

{: .highlight }
La organizacíon y jerarquia se puede modificar para satisfacer necesidades específicas.

El admininistrador de sistemas tiene la libertad the hacer ajustes en lo pertinente a la localizacíon absoluta del archivo asi como el tamaño correspondiente del mismo.

En este ejemplo, el comando `df` nos muestra la organización típica de un maquina virtual de CentOS 8 Stream. La structura es similar en Ubuntu.
```
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
overlay          59G   22G   35G  39% /
tmpfs            64M     0   64M   0% /dev
shm              64M     0   64M   0% /dev/shm
tmpfs           7.9G  377M  7.5G   5% /run
```

## Terminal 

La terminal es el area de trabajo donde podemos escribir comandos para interactuar con el sistema.

![](../../assets/images/terminal_example.png)

## Linea De Comandos (Command Line)

Generalmente, la linea de commados se identifica for el signo de dólar `$` cuando entramos al sistema. Eso puede ser personalizado en cualquier momento.

En este ejemplo, escribimos comandos que nos ayudan a identificar el usuario con que hemos entrado al sistema.
```
$ id
uid=0(root) gid=0(root) groups=0(root)

$ whoami
root
```

## Directorio De Inicio (Home Directory)

El directorio de inicio es donde aterrizamos y es nuestra base de operaciones.

Tan pronto como entramos al sistema, el comando `pwd` nos asiste para mostrarnos la localidad del directorio de inicio.
```
$ pwd
/root
```

Podemos usar `echo` para mostrar el directorio de inicio en cualquier momento después de esta en una sessión for algun tiempo.
```
$ echo $HOME
/root
```

## Páginas Manuales (Man Pages)

Las páginas manuales son documentos disponibles internamente el en sistema que nos proveen información acerca de las diferentes funcioines y comandos disponibles durante la session con el sistema.

Es de notar que las páginas manuales no están instaladas for de facto en máquinas virtuales. El usuario las puede instalar usando la localización que corresponda.

[Return to main page]({{site.baseurl}}/).