---
layout: default
title: SDKMAN - Java
permalink: /sdkman_java/
parent: Articulos
has_children: false
has_toc: false
nav_order: 1
---

## Que es SDKMAN?

{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

**DESCRIPCIÓN**

SDKMAN! es una herramienta de línea de comandos para gestionar versiones de software de desarrollo en sistemas Unix. Permite instalar, actualizar y cambiar entre diferentes versiones de herramientas como Java, Groovy, Scala, Kotlin, entre otros.

En este artículo, veremos cómo instalar **SDKMAN!** y cómo usarlo para gestionar versiones de Java.

**DEPENDENCIAS**

ninguna.

**REQUERIMIENTOS**

Se requiere acceso a internet.

**ADVERTENCIA**

ninguna.

## Ambiente de trabajo

En esta lección usamos el sistema operativo Ubuntu.

```bash
-> lsb_release -a
Distributor ID: Ubuntu
Description: Ubuntu 24.04.4 LTS
Release: 24.04
Codename: noble
```

## Cuando Usar SDKMAN!

Usa SDKMAN! para instalar y gestionar múltiples herramientas JVM (Java, Groovy, Kotlin, Scala, Maven, Gradle, etc.) en sistemas tipo Unix

Usos comunes:

- Instala una distribución/versión específica de Java para un proyecto
- Cambiar entre versiones de Java globalmente o por shell
- Instalar/gestionar herramientas de línea de comandos (Maven, Gradle, Sbt)
- Actualizar o listar SDKs y candidatos disponibles

## Guía Rápida

Guía rápida para probar SDKMAN!

```bash
# instalar sdkman
sudo curl -s "https://get.sdkman.io" | bash

# activar la configuración
source "/home/devuser/.sdkman/bin/sdkman-init.sh"

# listar versiones disponibles de java
sdk list java

# instalar una version
sdk install java 8.0.462-zulu

# verificar la version
sdk current java

# establecer la version predeterminada
sdk use java 8.0.462-zulu

# desinstalar una version de java
sdk uninstall java 11.0.28-zulu

# listar el directorio conteniendo todas las instalaciones en el sistema
ls -l /home/devuser/.sdkman/candidates/java
```

## Instalar SDKMAN

{: .note }
Corre el instalador con el usuario que usas para desarrollar, en tu directorio hogar.

Mientras estas en tu directorio hogar, corre el comando `curl` mostrado aqui:

```bash
Sat 2025Jul26 22:40:06 UTC
devuser@devops-u1
~
hist:91 -> sudo curl -s "https://get.sdkman.io" | bash
…
Set version to 5.19.0 ...
Set native version to 0.7.4 ...
Attempt update of interactive bash profile on regular UNIX...
Added sdkman init snippet to /home/devuser/.bashrc
Attempt update of zsh profile...
Updated existing /home/devuser/.zshrc

All done!

You are subscribed to the STABLE channel.

Please open a new terminal, or run the following in the existing one:

    source "/home/devuser/.sdkman/bin/sdkman-init.sh"

Then issue the following command:

    sdk help

Enjoy!!!
```

Cárga el script de Shell tal como lo sugirió el instalador.

```bash
->  source "/home/devuser/.sdkman/bin/sdkman-init.sh"
```

Verifica la versión de sdk.

```bash
-> sdk version
SDKMAN!
script: 5.19.0
native: 0.7.4 (linux x86_64)
```

La instalación agrega un comando `export` a `~/.bashrc`.

```bash
#ESTO DEBE ESTAR AL FINAL DEL ARCHIVO PARA QUE SDKMAN FUNCIONE!!!
export SDKMAN_DIR="$HOME/.sdkman"
[[ -s "$HOME/.sdkman/bin/sdkman-init.sh" ]] && source "$HOME/.sdkman/bin/sdkman-init.sh"
```

De esta manera, cada vez que abras una terminal, SDKMAN estará disponible para su uso.

Lista todos los candidatos.

```bash
-> sdk list
```

## Instalar y gestionar versiones de Java

Lista todas las versiones de JAVA con `sdk list java`

Lista JAVA específicamente.

```bash
-> sdk list java

================================================================================
Available Java Versions for Linux 64bit
================================================================================
 Vendor        | Use | Version      | Dist    | Status     | Identifier
--------------------------------------------------------------------------------
…
 Oracle        |     | 26.0.1       | oracle  |            | 26.0.1-oracle
               |     | 25.0.3       | oracle  |            | 25.0.3-oracle
               |     | 21.0.11      | oracle  |            | 21.0.11-oracle
               |     | 17.0.12      | oracle  |            | 17.0.12-oracle
…

Zulu           |     | 26.crac      | zulu    |            | 26.crac-zulu
               |     | 26.fx        | zulu    |            | 26.fx-zulu
               |     | 26.0.1.crac  | zulu    |            | 26.0.1.crac-zulu
               |     | 26.0.1.fx    | zulu    |            | 26.0.1.fx-zulu
               |     | 26.0.1       | zulu    |            | 26.0.1-zulu
               |     | 25.0.3.crac  | zulu    |            | 25.0.3.crac-zulu
               |     | 25.0.3.fx    | zulu    |            | 25.0.3.fx-zulu
               |     | 25.0.3       | zulu    |            | 25.0.3-zulu
               |     | 25.0.2.crac  | zulu    |            | 25.0.2.crac-zulu
               |     | 25.0.2.fx    | zulu    |            | 25.0.2.fx-zulu
               |     | 21.0.11.crac | zulu    |            | 21.0.11.crac-zulu
               |     | 21.0.11.fx   | zulu    |            | 21.0.11.fx-zulu
               |     | 21.0.11      | zulu    |            | 21.0.11-zulu
               |     | 21.0.10.crac | zulu    |            | 21.0.10.crac-zulu
               |     | 21.0.10.fx   | zulu    |            | 21.0.10.fx-zulu
               |     | 17.0.19.crac | zulu    |            | 17.0.19.crac-zulu
               |     | 17.0.19.fx   | zulu    |            | 17.0.19.fx-zulu
               |     | 17.0.19      | zulu    |            | 17.0.19-zulu
               |     | 17.0.18.crac | zulu    |            | 17.0.18.crac-zulu
               |     | 17.0.18.fx   | zulu    |            | 17.0.18.fx-zulu
               |     | 11.0.31.fx   | zulu    |            | 11.0.31.fx-zulu
               |     | 11.0.31      | zulu    |            | 11.0.31-zulu
               |     | 11.0.30.fx   | zulu    |            | 11.0.30.fx-zulu
               |     | 8.0.492.fx   | zulu    |            | 8.0.492.fx-zulu
               |     | 8.0.492      | zulu    |            | 8.0.492-zulu
               |     | 8.0.482.fx   | zulu    |            | 8.0.482.fx-zulu
               |     | 7.0.352      | zulu    |            | 7.0.352-zulu
               |     | 6.0.119      | zulu    |            | 6.0.119-zulu
=================================================================================
```

La lista muestra las versiones disponibles de Java a esta fecha. Muestra el proveedor (Vendor), el estado (Status) y el identificador (Identifier) para cada versión. La lista es actualizada pasado el tiempo cuando viejas versiones son obsoletas y nuevas versiones son publicadas.

Si ejecutas el comando de instalación y presionas tab, listará todo así (salida recortada)

```bash
-> sdk install java <presiona tab>

Display all 285 possibilities? (y or n)
11.0.14.1-jbr        17.0.16-ms           23.0.2-zulu          25.ea.16-graal
11.0.15-trava        17.0.16-sapmchn      23.0.2.fx-librca     25.ea.16-open
11.0.25-albba        17.0.16-sem          23.0.2.fx-zulu       25.ea.17-graal
(…snip…)
17.0.15-tem          23.0.1.crac-zulu     24.fx-librca         8.0.462-librca
17.0.15-zulu         23.0.2-amzn          24.fx-zulu           8.0.462-sem
17.0.15.crac-librca  23.0.2-graal         25.ea.10-open        8.0.462-tem
17.0.15.crac-zulu    23.0.2-graalce       25.ea.13-graal       8.0.462-zulu
17.0.15.fx-librca    23.0.2-librca        25.ea.14-graal       8.0.462.fx-librca
17.0.15.fx-zulu      23.0.2-oracle        25.ea.14-open
17.0.16-amzn         23.0.2-sapmchn       25.ea.15-graal
17.0.16-librca       23.0.2-tem           25.ea.15-open
```

La sintaxis del comando de instalación es:

```bash
sdk install java <version>
```

La versión puede ser el identificador completo o una parte del mismo, como `11.0.28-zulu` o simplemente `11`.

{: .important }
Es mejor practica especificar exactamente la versión completa a instalar para evitar confusiones. Por ejemplo `11.0.28-zulu`. 

Instalar una versión específica de java

```bash
-> sdk install java 8.0.462-zulu
Downloading: java 8.0.462-zulu
In progress...
########## 100.0%
Repackaging Java 8.0.462-zulu...
Done repackaging...
```

Ahora tratemos de instalar otra versión. Nota que, si ya tienes otra versión instalada previamente, preguntará si deseas establecer esta nueva instalación como predeterminada. Simplemente responde `y` o `n` según lo que desees.

```bash
-> sdk install java 11.0.28-zulu
Downloading: java 11.0.28-zulu
In progress...
########## 100.0%
Repackaging Java 11.0.28-zulu...
Done repackaging...
Installing: java 11.0.28-zulu
Done installing!
Do you want java 11.0.28-zulu to be set as default? (Y/n): y
Setting java 11.0.28-zulu as default.

-> sdk install java 17.0.16-zulu
Downloading: java 17.0.16-zulu
In progress...
##########  100.0%
Repackaging Java 17.0.16-zulu...
Done repackaging...
Installing: java 17.0.16-zulu
Done installing!
Do you want java 17.0.16-zulu to be set as default? (Y/n): n
```

Verifica cuál es la version de java actual activa por defecto.

```bash
-> sdk current java
Using java version 11.0.28-zulu
```

## Desinstalar Version de Java

Para desinstalar alguna version de JAVA simplemente haz lo contrario del comando de instalación.

```bash
-> sdk uninstall java 11.0.28-zulu
```

## Listar Versiones de Java

Lista solo las versiones de JAVA.

El comando de listar muestra las versiones instaladas marcadas port la palabra `installed`.

```bash
-> sdk list java

               |     | 11.0.26      | zulu    |            | 11.0.26-zulu
               | >>> | 8.0.462      | zulu    | installed  | 8.0.462-zulu
               |     | 8.0.452.fx   | zulu    |            | 8.0.452.fx-zulu
               |     | 8.0.452      | zulu    |            | 8.0.452-zulu
```

## Ubicación de las versiones de Java en SDKMAN

Hemos estado ejecutando todos los comandos de SDKMAN con nuestro usuario en nuestro directorio home.

En Ubuntu, SDKMAN coloca todas las instalaciones en $HOME/.sdkman/candidates/java

```bash
-> ls -l /home/devuser/.sdkman/candidates/java
total 12
drwxr-xr-x 10 devuser devuser 4096 Jul  2 12:10 11.0.28-zulu/
drwxr-xr-x 10 devuser devuser 4096 Jun 30 16:33 17.0.16-zulu/
drwxr-xr-x  9 devuser devuser 4096 Jul  2 11:57 8.0.462-zulu/
lrwxrwxrwx  1 devuser devuser   12 Jul 26 23:02 current -> 11.0.28-zulu/
```

La versión activa por defecto está claramente marcada con la palabra `current`, o _corriente_.

## Cambiar entre diferentes versiones de Java

Una vez que hemos instalado varias versiones de Java, ahora vamos a cambiar entre ellas.

Usar Java17

```bash
Sat 2025Jul26 23:13:14 UTC
devuser@devops-u1
~/.sdkman/candidates/java
hist:111 -> sdk use java  17.0.16-zulu
Using java version 17.0.16-zulu in this shell.

-> java -version
openjdk version "17.0.16" 2025-07-15 LTS
OpenJDK Runtime Environment Zulu17.60+17-CA (build 17.0.16+8-LTS)
OpenJDK 64-Bit Server VM Zulu17.60+17-CA (build 17.0.16+8-LTS, mixed mode, sharing)
```

Usar Java8

```bash
-> sdk use java 8.0.462-zulu
Using java version 8.0.462-zulu in this shell.

-> java -version
openjdk version "1.8.0_462"
OpenJDK Runtime Environment (Zulu 8.88.0.19-CA-linux64) (build 1.8.0_462-b08)
OpenJDK 64-Bit Server VM (Zulu 8.88.0.19-CA-linux64) (build 25.462-b08, mixed mode
```

Usar Java11

```bash
-> sdk use java 11.0.28-zulu

-> java -version
openjdk version "11.0.28" 2025-07-15 LTS
OpenJDK Runtime Environment Zulu11.82+19-CA (build 11.0.28+6-LTS)
OpenJDK 64-Bit Server VM Zulu11.82+19-CA (build 11.0.28+6-LTS, mixed mode)
```

## Verificar la versión de Java

Aunque hemos cambiado la versión de Java en la sesión actual, el enlace simbólico `current` no cambia. Esto es normal, ya que `sdk use java` solo afecta a la sesión actual del shell.

En seguida verificamos la version establecida por defecto.

```bash
-> sdk current java
Using java version 11.0.28-zulu
```

Ahora observamos que la versión actual reflejada en el directorio de instalación no cambia cuando usas `sdk use java <version>`.

```bash
Sat 2025Jul26 23:22:37 UTC
devuser@devops-u1
~
hist:147 -> ls -l /home/devuser/.sdkman/candidates/java
total 12
drwxr-xr-x 10 devuser devuser 4096 Jul  2 12:10 11.0.28-zulu/
drwxr-xr-x 10 devuser devuser 4096 Jun 30 16:33 17.0.16-zulu/
drwxr-xr-x  9 devuser devuser 4096 Jul  2 11:57 8.0.462-zulu/
lrwxrwxrwx  1 devuser devuser   12 Jul 26 23:02 current -> 11.0.28-zulu/
```

El comando `sdk use java` ... solo cambia las variables de entorno `JAVA_HOME` y `PATH` para la sesión actual del shell. NO cambia el enlace simbólico actual en el directorio de SDKMAN!.

El enlace simbólico actual solo se actualiza cuando ejecutas `sdk default java ...` o `sdk install java ...`, no con `sdk use java ...`.

Así que usas `sdk use java` para un cambio temporal, solo en la shell (no actualiza el enlace simbólico).

Usa `sdk default java` para un cambio permanente, y actualiza el enlace simbólico para afectar nuevas shells.

## Establecer una versión de Java como predeterminada

Asi que cambiemos completamene a usar `17.0.16-zulu` y verifiquemos el ajuste.

```bash
-> sdk use java 17.0.16-zulu
Using java version 17.0.16-zulu in this shell.

-> sdk default java 17.0.16-zulu
setting java 17.0.16-zulu as the default version for all shells.

-> sdk current java
Using java version 17.0.16-zulu

-> java -version
openjdk version "17.0.16" 2025-07-15 LTS
OpenJDK Runtime Environment Zulu17.60+17-CA (build 17.0.16+8-LTS)
OpenJDK 64-Bit Server VM Zulu17.60+17-CA (build 17.0.16+8-LTS, mixed mode, sharing)

-> ls -l /home/devuser/.sdkman/candidates/java
total 12
drwxr-xr-x 10 devuser devuser 4096 Jul  2 12:10 11.0.28-zulu/
drwxr-xr-x 10 devuser devuser 4096 Jun 30 16:33 17.0.16-zulu/
drwxr-xr-x  9 devuser devuser 4096 Jul  2 11:57 8.0.462-zulu/
lrwxrwxrwx  1 devuser devuser   50 Jul 27 03:24 current -> /home/devuser/.sdkman/candidates/java/17.0.16-zulu/
```

## Referencias 

- [Guía de SDKMAN!](https://www.baeldung.com/java-sdkman-intro)
- [Pagina Manual de SDKMAN!](https://sdkman.io/usage/)

[Regresar a la página principal]({{site.baseurl}}/).