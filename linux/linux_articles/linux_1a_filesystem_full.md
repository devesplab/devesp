---
layout: default
title: Filesystem Full
permalink: /linux-filesystem-full/
parent: Artículos De Linux
has_children: false
has_toc: false
nav_order: 1
---

# Filesystem Full

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

Ocassionalmente, el sistema de archivos puede llenarse, lo que puede causar problemas en el sistema. Aquí hay algunos pasos para solucionar este problema:
1. **Verificar el uso del disco**: Utiliza el comando `df -h` para ver el uso del disco y determinar qué partición está llena.
2. **Identificar archivos grandes**: Utiliza el comando `du -sh /*` para identificar los directorios que están utilizando más espacio en disco.
3. **Limpiar archivos temporales**: Puedes limpiar archivos temporales con el comando `sudo apt-get clean` o `sudo yum clean all`, dependiendo de tu distribución.
4. **Eliminar archivos innecesarios**: Revisa los directorios y elimina archivos que ya no necesites, especialmente en `/var/log`, `/tmp`, y otros directorios temporales.
5. **Revisar archivos de registro**: Asegúrate de que los archivos de registro no estén creciendo demasiado. Puedes rotarlos o eliminarlos si es necesario.
6. **Revisar contenedores y volúmenes**: Si estás utilizando Docker, revisa los contenedores y volúmenes que pueden estar ocupando espacio. Usa `docker system df` para ver el uso del disco por parte de Docker.   
