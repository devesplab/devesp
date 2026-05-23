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


Aquí tienes los pasos prácticos y concisos para solucionar y recuperar un sistema de archivos lleno en Ubuntu.

## Medidas inmediatas (haz esto primero)

Cambia a root o usa sudo para todos los comandos:
```
-> sudo -i
```
Detén servicios no esenciales para evitar crecimiento de logs (ajusta según el sistema):
```
-> sudo systemctl stop apache2 mysql postfix docker  
```   
Abre una consola raíz en un tty separado si el interfaz gráfico no responde.  

## Identificar qué está lleno

Mostrar uso por sistema de archivos:
```
-> df -hT
```   

Identificar puntos de montaje con uso al 100% o cercano:
```
-> df -hT | awk '$6 ~ /^\// && $5+0 >= 90 {print}'
```

## Encontrar archivos y directorios grandes

Resumen rápido del uso en la raíz (rápido):
```
-> sudo du -shx /* 2>/dev/null | sort -h
```   
Profundizar en un directorio (ejemplo /var):
```
-> sudo du -shx /var/* 2>/dev/null | sort -h
```   
Buscar archivos individuales mayores a 100 MB:
```
-> sudo find / -xdev -type f -size +100M -exec ls -lh {} \; 2>/dev/null
```   
Mostrar los 20 archivos más grandes:
```
-> sudo find / -xdev -type f -printf '%s %p\n' 2>/dev/null | sort -nr | head -20 | awk '{printf "%.1f MB\t%s\n",$1/1024/1024,$2}'
```   

## Limpiar culpables comunes

Limpiar la caché de apt:
```
-> sudo apt-get clean
```   
Eliminar kernels antiguos (método seguro):
```
-> sudo apt-get autoremove --purge
```
Truncar logs grandes (no eliminar logs en uso; truncar con seguridad):
```
-> sudo truncate -s 0 /var/log/syslog /var/log/kern.log /var/log/*.log
```   
Rotar y comprimir logs inmediatamente:
```
-> sudo systemctl restart rsyslog
-> sudo logrotate -f /etc/logrotate.conf
```   
Limpiar logs del journal:
```
-> sudo journalctl --vacuum-size=200M
```   
o por tiempo: 
```
-> sudo journalctl --vacuum-time=7d
```

Limpiar Docker (si está instalado):
```
-> sudo docker system prune -a --volumes
```   

Eliminar paquetes huérfanos y cachés:
```
-> sudo apt-get autoremove
-> sudo apt-get autoclean
```

## Recuperación rápida cuando el espacio es crítico

Localiza y borra un archivo grande no esencial (alivio temporal). Ejemplo:
```
-> sudo rm /paso/a/archivo-grande
```   

Si `rm` falla por archivos borrados pero abiertos, identifícalos y libera su espacio:
```
-> sudo lsof -nP | grep '(deleted)'
```

Crea espacio temporal moviendo archivos grandes a otra partición:
```
-> sudo mv /paso/a/archivo-grande /mnt/temporal/
```   

{: .warning }
NO HAGAS ESTO: `sudo rm -rf /paso/archivo-grande/*`<br>
Esa acción en un sistema activo puede tener resultados inesperados.

## Limpiar Archivo Grande

Supongamos que queremos limpiar `/tmp`. <br>
Primero que nada, no debemos borrar el contenido entero the un sistema que esta corriendo.<br>
No hagas esto:
```
rm -rf /tmp/*
```

Para limpiar un archivo grande, sigue los pasos a seguir.

Por ejemplo, operemos en archivos de 7 dias os más de viejos.

Listar archivos viejos:
```
find /tmp -type f -mtime +7
```
Borrar archivos 7 días o más de viejos.
```
sudo find /tmp -type f -mtime +7 -delete
```
Borrar carpetas 7 días o más de viejos..
```
sudo find /tmp -mindepth 1 -mtime +7 -exec rm -rf {} +
```
En sistemas que utilizan systemd, el enfoque preferido suele ser:
```
systemd-tmpfiles --clean
```
porque respeta las políticas de limpieza configuradas. 

Puedes consultar la póliza aquí:
```
cat /usr/lib/tmpfiles.d/tmp.conf
```
o aqui:
```
cat /etc/tmpfiles.d/*.conf
```
Un patrón de limpieza manual más seguro en los sistemas de producción es:
```
sudo find /tmp -xdev -type f -atime +3 -delete
```
Eso evita cruzar los límites del sistema de archivos y solo elimina archivos antiguos no utilizados.

También nota: 
- muchos sistemas Linux limpian automáticamente /tmp 
- Reiniciar puede borrar /tmp dependiendo de la distribución/configuración 
- /var/tmp está pensado para archivos temporales de mayor duración y normalmente sobrevive al reinicio

## Comprobar la salud del sistema de archivos

1. Para particiones desmontadas, ejecuta `fsck` (desde recuperación o live USB):
```
-> sudo umount /dev/sdXN
-> sudo fsck -f /dev/sdXN
```   
2. Para la raíz, arranca en recuperación o usa medios en vivo para ejecutar `fsck`.

## Prevenir recurrencias (tareas de seguimiento)

1. Añade monitorización y alertas:
   - Scripts cron sencillos que envíen df -h por correo o usa monitorización (Prometheus + node_exporter + Grafana).
2. Añade herramientas de análisis de uso:
   - Instala `ncdu` para análisis interactivo: `sudo apt-get install ncdu`
   - Ejecuta: `sudo ncdu /`
3. Configura rotación de logs y limita el tamaño del journal:
   - Edita `/etc/systemd/journald.conf: SystemMaxUse=200M`
   - Asegura políticas adecuadas en `/etc/logrotate.d`.
4. Mueve directorios grandes y volátiles a particiones más grandes (ej.: mover `/var/lib/docker` o `/home` a otro disco) y usa bind-mounts.
5. Usa cuotas en sistemas multiusuario:
   - `sudo apt-get install quota`; configura `/etc/fstab` y `edquota`.

## Comandos diagnósticos rápidos (one-liners)

```
-> df -hT
-> sudo du -shx /* | sort -h
-> sudo find / -xdev -type f -size +100M -exec ls -lh {} \; | sort -k5 -h
-> sudo journalctl --disk-usage
-> sudo lsof +L1  (lista archivos borrados pero abiertos)
```

## Usando -maxdepth, -mindepth

Puede que haya un caso en el que tengamos el sistema de archivos raíz `/`, y quizá `/mnt` justo debajo, ¿cómo usar maxdepth para mirar solo en la raíz sin recorrer `/mnt`?

Ejemplos (ejecuta como root o con sudo):

Usar `-xdev` para evitar descender a otros sistemas de archivos (recomendado):
```
sudo find / -xdev -type f -size +100M -printf '%s %p\n' 2>/dev/null | sort -nr | head -20
```
Usar `-maxdepth` para buscar solo en entradas de primer nivel bajo `/` (no recorrerá `/mnt` si `/mnt` es un punto de montaje separado):
```
sudo find / -maxdepth 2 -mindepth 2 -type f -size +100M -printf '%s %p\n' 2>/dev/null | sort -nr | head -20
```
(Con `-maxdepth 2` se listan archivos en subdirectorios inmediatos de `/`; ajusta la profundidad según necesites. Usa `-maxdepth 1` para listar solo archivos directamente en `/`.)

Combina ambos para mayor seguridad (mantente en el mismo FS y limita la profundidad):
```
sudo find / -xdev -maxdepth 3 -type f -printf '%s %p\n' 2>/dev/null | sort -nr | head -20
```

Notas:
- `xdev` impide descender a otros sistemas de archivos montados (por ejemplo /mnt si es un montaje separado).
- `maxdepth` controla la profundidad de recursión pero no evita por sí solo cruzar puntos de montaje; usa ambos cuando corresponda.

## Cuándo pedir ayuda / acciones riesgosas

- No borres archivos desconocidos en `/var`, `/etc`, `/boot` sin confirmar su propósito.
- Pide ayuda si liberar espacio requiere eliminar paquetes del sistema, kernels o archivos profundos.
- Para sistemas con NFS o volúmenes en la nube, coordina con operaciones antes de redimensionar o desmontar.

