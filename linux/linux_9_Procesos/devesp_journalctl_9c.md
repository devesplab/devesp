---
layout: default
permalink: /journalctl_maintenance/
title: Manteniendo Journalctl
parent: Procesos
grand_parent: Linux
has_children: true
has_toc: false
nav_order: 4
---

# Mantenimiento del Journalctl en Linux

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

En Ubuntu (que usa el diario de systemd para el registro), se puede recuperar espacio en disco de forma segura limpiando los registros del diario almacenados en `/var/log/journal`. Sin embargo, borrar los archivos no debe ser la mejor solución. En su lugar, se recomienda usar las herramientas integradas de `systemd` para administrar y truncar los registros del diario correctamente.

Podemos ver la cantidad de espacio que el journal esta usando de esta manera.

```
-> du -sh  /var/log/journal
850M	/var/log/journal
```

Hoy dia la mayoría de sistemas no tienen problemas de espacio. Pero si estamos usando contenedores de Docker, esto podría ser un problema. En este caso puede ser necesario manejar el tamaño del journal, lo cual puede incluir disminuir el tamaño.


## Limpiando el Journal

Para eliminar archivos del diario archivados con una antigüedad superior a cierta cantidad de tiempo(p. ej., 2 semanas) hacemos lo siguiente:

```
sudo journalctl --vacuum-time=2weeks
```

O bien, para limitar el espacio total en disco utilizado por los registros del diario (p. ej., reducirlo a 50 MB):

```bash
-> sudo journalctl --vacuum-size=50M
Vacuuming done, freed 0B of archived journals from /var/log/journal.
Vacuuming done, freed 0B of archived journals from /var/log/journal/68646fed809a4c54ab836048028d6896.
Vacuuming done, freed 0B of archived journals from /run/log/journal
```

Usando la opción `--vacuum-size=` effectivamente reduce el tamaño del journal a la cantidad de megabytes especificada.

## Persistir el Tamaño del Journal

Si desea limitar el tamaño máximo de los registros del diario de forma permanente, puede editar el archivo de configuración:

```
sudo vi /etc/systemd/journald.conf
```

Configure `SystemMaxUse` con el tamaño que prefiera, por ejemplo:

```
-> grep SystemMaxUse /etc/systemd/journald.conf
SystemMaxUse=50M
```

Lo anterior indica que el tamaño máximo del journal seral 50 megabytes.


Guarde el archivo y reinicie el servicio del diario:

```
sudo systemctl restart systemd-journald
```

## Eliminar manualmente los archivos del diario (menos recomendable)

{: .warning }
(!) No se recomienda borrar el archivo de journalctl.<br>
Tenga cuidado y prefiera usar los comandos journalctl vacuum.

Si necesita eliminar manualmente los archivos del diario, puede eliminarlos de `/var/log/journal/`, pero generalmente no se recomienda, ya que puede dejar el sistema del diario en un estado inconsistente. Si elige esta opción:

```
sudo rm -rf /var/log/journal/*
```

## Referencias

Página manual de [journalctl](https://www.man7.org/linux/man-pages/man1/journalctl.1.html)

[Return to main page]({{site.baseurl}}/).