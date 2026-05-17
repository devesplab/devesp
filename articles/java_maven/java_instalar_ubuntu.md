---
layout: default
title: Java Instalar en Ubuntu
permalink: /java-intalar_ubuntu/
parent: Articulos
has_children: false
has_toc: false
nav_order: 3
---

{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

# Instalación de JAVA OpenJDK

**DESCRIPCIÓN**

En esta lección:
- instalar Java OpenJDK en Ubuntu
- obtener la distribución de JAVA OpenJDK
- descomprimir la distribución
- instalar el binario de Maven

**DEPENDENCIAS**

Ninguna.

**REQUISITOS**

- Ubuntu 18.04 o superior.
- Privilegios de sudo.
- Conexión a Internet para descargar los paquetes.
- ~200 MB de espacio libre en disco para el entorno de ejecución (runtime) de OpenJDK; ~400–600 MB para el JDK con herramientas de desarrollo.

**ADVERTENCIAS**

Este documento está destinado específicamente a la instalación de OpenJDK a partir de los paquetes de Ubuntu.

## Entorno de trabajo

Esta lección se lleva a cabo en un sistema Linux Ubuntu.
```
-> lsb_release -a
Distributor ID:	Ubuntu
Description:	Ubuntu 24.04.4 LTS
Release:	24.04
Codename:	noble
```

## Prerrequisitos

Necesita privilegios SUDO, una conexión a Internet y aproximadamente 200 MB de espacio en disco.

## Distribución binaria

Diríjase a la [compilación de OpenJDK de Canonical para Ubuntu](https://ubuntu.com/toolchains/java) y obtenga una distribución.

## Instalación del Java JDK

En primer lugar, actualice la lista de paquetes de Ubuntu.
```
sudo apt update
```

Para instalar el Java Development Kit _**predeterminado**_ para tu versión de Ubuntu, ejecuta:
```
sudo apt install default-jdk
```

Si buscas una version especifica, puedes listar todas las versiones disponibles de OpenJDK.
```
sudo apt-cache search ^openjdk
```

Instala una versión específica de OpenJDK con:
```
sudo apt install openjdk-<version>-jdk
```
Por ejemplo, instalemos `openjdk-21`:
```
sudo apt-get install -y --no-install-recommends openjdk-21-jdk
```

Ver los paquetes instalados:
```
-> dpkg -l | grep openjdk
ii  openjdk-21-jdk:amd64            21.0.10+7-1~24.04     amd64   OpenJDK Development Kit (JDK)
ii  openjdk-21-jdk-headless:amd64   21.0.10+7-1~24.04     amd64   OpenJDK Development Kit (JDK) (headless)
ii  openjdk-21-jre:amd64            21.0.10+7-1~24.04     amd64   OpenJDK Java runtime, using Hotspot JIT
ii  openjdk-21-jre-headless:amd64   21.0.10+7-1~24.04     amd64   OpenJDK Java runtime, using Hotspot JIT (headless)
```

Verifique la extracción del archivo. Los binarios de Java estarán disponibles en el directorio correspondiente a esa versión específica.
```
-> ls /usr/lib/jvm/java-21-openjdk-amd64/bin
jar*
jarsigner*
java*
javac*
javadoc*
...snip..
```

Utilice el comando `readlink` para obtener la ruta al binario de Java.
```
-> readlink -f /usr/bin/java
/usr/lib/jvm/java-21-openjdk-amd64/bin/java
```

Crea un archivo para las variables de entorno de Java. Haz que el archivo sea ejecutable.
```
-> sudo vi /etc/profile.d/java.sh

-> sudo chmod +x /etc/profile.d/java.sh
```

En el archivo, añada las siguientes líneas para establecer las variables de entorno.<br>
Añada la ruta del directorio de Java obtenida con el comando `readlink`.
```
# System-wide Java and Maven environment
export JAVA_HOME=/usr/lib/jvm/java-21-openjdk-amd64
export PATH="${JAVA_HOME}/bin:${PATH}"
```
Asegúrese de que las rutas existan (ajuste la variable JAVA_HOME a la ruta real de la JVM instalada, obtenida mediante `readlink -f /usr/bin/java`).<br>
Incluya `${JAVA_HOME}/bin` en la variable PATH para que los comandos `java` y `javac` estén disponibles para todos los usuarios.<br>
Coloque el archivo en `/etc/profile.d/` (los scripts ubicados allí son cargados por `/etc/profile` para las *login shells*). Para las *non-login shells*, algunas distribuciones cargan `/etc/profile`; para cubrir también las *non-login shells* interactivas, puede crear un pequeño enlace simbólico o cargar este archivo desde `/etc/bash.bashrc` si fuera necesario.<br>
Utilice rutas absolutas y evite incluir en este archivo comandos que puedan fallar durante las etapas iniciales del arranque del sistema.<br>

Tras añadir el archivo, reinicie el sistema o cárguelo manualmente (`source`) para aplicar los cambios de inmediato en la sesión actual.
```
source /etc/profile.d/java.sh
```

{: .note }
Los archivos en `/etc/profile.d` se leen automáticamente para las sesiones de inicio de sesión interactivas y las sesiones gráficas que utilizan `/etc/profile`.

Utilice el comando `env` para verificar las variables de entorno.
```
-> env | grep -i java
```

Verifique la instalación con
```
echo $JAVA_HOME
which java
java -version
```

## Desinstalar OpenJDK

Esta operación eliminará por completo la versión de OpenJDK seleccionada del sistema.

Ejecute estos comandos (utilice sudo):

Eliminar el paquete, pero conservar los archivos de configuración:
```
sudo apt remove openjdk-21-jdk
```
Eliminar el paquete y los archivos de configuración:
```
sudo apt purge openjdk-21-jdk
```
Elimine cualquier dependencia instalada automáticamente que ya no se utilice:
```
sudo apt autoremove --purge
```
Verifique que no queden binarios de Java de ese paquete:
```
dpkg -l | grep openjdk
which java
java -version
```
Si existen otros paquetes de OpenJDK y también desea eliminarlos, reemplace `openjdk-21-jdk` con el nombre del paquete que aparece en la lista de `dpkg -l | grep openjdk`.

Elimine el archivo de perfil si lo había creado.
```
rm /etc/profile.d/java.sh
```

## Conclusion

La utilidad de Java sirve para construir proyectos Java.

Java es un lenguaje de programación que se utiliza para indicarle a una computadora qué debe hacer. Permite escribir programas que pueden ejecutarse en una gran variedad de dispositivos sin necesidad de modificar el código. Java se emplea habitualmente para desarrollar sitios web, aplicaciones móviles (Android), programas de escritorio y software para servidores. Ofrece herramientas que hacen que los programas sean más seguros y fáciles de gestionar (como la verificación de errores antes de la ejecución). Los desarrolladores prefieren Java porque es estable, cuenta con un amplio soporte y dispone de una gran cantidad de bibliotecas reutilizables.

## Referencias 

### Glosario De Comandos

readlink
: Imprime el valor de un enlace simbólico o el nombre canónico del archivo.

### Referencias Utiles

- Usando [SDKMAN!](./java_sdkman.md) para manejar versiones de Java y otras utilidades.
- [How to set up a development environment for Java on Ubuntu](https://documentation.ubuntu.com/ubuntu-for-developers/howto/java-setup/#install-java)
- [Develop with Java on Ubuntu](https://documentation.ubuntu.com/ubuntu-for-developers/tutorials/java-use/#use-java)

Paginas Manuales
- [readlink](https://manpages.ubuntu.com/manpages/stonking/man1/readlink.1.html)
