---
layout: default
title: Maven Instalar en Ubuntu
permalink: /maven-intalar_ubuntu/
parent: Articulos
has_children: false
has_toc: false
nav_order: 4
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

# Instalación MAVEN en Ubuntu

**DESCRIPCIÓN**

Maven es una herramienta que ayuda a los desarrolladores de Java a crear y gestionar sus proyectos. En términos simples:

- Compila su código en programas ejecutables (compilaciones).
- Descarga y administra automáticamente las bibliotecas que su proyecto necesita (dependencias).
- Define un diseño de proyecto estándar y comandos comunes para que las compilaciones sean consistentes.
- Puede ejecutar pruebas, empaquetar su aplicación (por ejemplo, en un .jar) y crear documentación.

Maven es principalmente para proyectos basados ​​en Java y JVM (Java, Kotlin, Scala, Groovy). También admite la creación de artefactos relacionados (JAR, WAR, EAR) y proyectos de múltiples módulos. Con complementos, se puede utilizar para tareas que no sean Java (generar documentos, ejecutar scripts, empaquetar recursos nativos) e incluso otros lenguajes, pero el soporte completo de primera clase es para ecosistemas JVM.

Apache Maven puede ser instalado por la mayoría de los administradores de paquetes o manualmente descargando el archivo y agregándolo a su PATH.
Puede instalar la distribución en cualquier carpeta de su elección siempre y cuando que tenga permisos de escritura.

En esta lección exploraremos lo siguiente:
- obtener la distribución maven
- descomprimir la distribución
- instalar el binario maven

**REQUISITOS**

- Requiere Java JDK instalado antes de usar maven.
- Debe tener disponible el comando TAR y UNZIP.
- Para referencia ver el documento como [Installar Java](./java_instalar_ubuntu.md).

Para instalar Maven en Ubuntu, necesita tener instalado un kit de desarrollo de Java (JDK), específicamente JDK 8 o superior para Maven 3.9.15, y JDK 17 o superior para Maven 4.x. Además, asegúrese de que su sistema tenga suficiente espacio en disco y memoria para admitir la instalación y el funcionamiento de Maven.

**ADVERTENCIAS**

Los problemas comunes al instalar Maven en Ubuntu incluyen versiones desactualizadas del administrador de paquetes predeterminado y configuraciones incorrectas de las variables de entorno. Para resolverlos, puede descargar manualmente los archivos binarios de Maven más recientes desde el sitio web oficial de Apache Maven y configurar las variables de entorno necesarias como `JAVA_HOME` y `M2_HOME`.

## Ambiente de trabajo

Esta lección se realiza en un sistema Linux Ubuntu.
```
-> lsb_release -a
Distributor ID:	Ubuntu
Description:	Ubuntu 24.04.4 LTS
Release:	24.04
Codename:	noble
```

## Requisitos previos

Necesita tener instalado un kit de desarrollo de Java (JDK). Configure la variable de entorno `JAVA_HOME` en el `PATH` de su instalación de JDK o tenga el ejecutable de Java en su `PATH`.

La versión estable actual `3.9.15` requiere `JDK 8+`, pero cualquier versión reciente funcionará bien.

## Distribución binaria

Vaya al [Sitio Oficial de Maven](https://maven.apache.org/download.cgi) y obtenga una distribución.

Para instalar Apache Maven, extraiga el archivo y agregue su directorio bin a  `PATH`. Esto funciona en cualquier sistema operativo, pero la configuración de `PATH` y las variables de entorno depende del sistema operativo.

Los pasos detallados son:

1. Descargue el archivo de distribución binaria de Apache Maven.

2. Extraiga el archivo de distribución en cualquier directorio. Utilice descomprimir `apache-maven-3.9.15-bin.zip` o `tar xzvf apache-maven-3.9.15-bin.tar.gz` dependiendo del archivo.

3. Agregue el directorio bin del directorio creado apache-maven-3.9.15 a la variable de entorno PATH

4. Confirme con `mvn -v` en un nuevo shell. El resultado debería ser similar a:
```
Apache Maven 3.9.15 (98b2cdbfdb5f1ac8781f537ea9acccaed7922349)
Inicio de Maven: /opt/apache-maven-3.9.15
Versión de Java: 1.8.0_45, proveedor: Oracle Corporation
Inicio de Java: /Biblioteca/Java/JavaVirtualMachines/jdk1.8.0_45.jdk/Contents/Home/jre
Configuración regional predeterminada: en_US, codificación de plataforma: UTF-8
Nombre del sistema operativo: "mac os x", versión: "10.8.5", arco: "x86_64", familia: "mac"
```

## Descargar Maven

En este ejemplo, usaremos la distribución Maven `apache-maven-3.9.15`. Ajústelo para la versión que desee.

Vaya a la página [Descarga de Maven](https://maven.apache.org/download.cgi) y obtenga una distribución.
```
-> wget https://dlcdn.apache.org/maven/maven-3/3.9.15/binaries/apache-maven-3.9.15-bin.tar.gz
```

Extraiga el archivo descargado.
```
-> sudo tar -zxvf apache-maven-3.9.15-bin.tar.gz -C /opt
```

Verifique la extracción del archivo.
```
-> ls /opt/apache-maven-3.9.15
LICENCIA
AVISO
LÉAME.txt
papelera/
arranque/
configuración/
biblioteca/
```

Crea un enlace simbólico a una ubicación conocida como `/usr/local/bin`.
```
-> sudo ln -s /opt/apache-maven-3.9.15 /usr/local/bin/maven

-> ls -l /usr/local/bin/maven
lrwxrwxrwx 1 raíz raíz 24 8 de mayo 04:49 /usr/local/bin/maven -> /opt/apache-maven-3.9.15/
```

Crea un archivo para las variables de entorno de Maven. Haga que el archivo sea ejecutable.
```
-> sudo vi /etc/profile.d/maven.sh

-> sudo chmod +x /etc/profile.d/maven.sh
```

En el archivo, agregue las siguientes líneas para configurar las variables de entorno.
```
export JAVA_HOME=/usr/lib/jvm/java-21-openjdk-amd64
export M2_HOME=/opt/apache-maven-3.9.15
export MAVEN_HOME=/opt/apache-maven-3.9.15
export PATH=${M2_HOME}/bin:${PATH}
```

Cargue las variables de entorno.
```
source /etc/profile.d/maven.sh
```

Los archivos en `/etc/profile.d` se leen automáticamente para shells de inicio de sesión interactivos e inicios de sesión gráficos que generan `/etc/profile`.

Utilice el comando `env` para verificar las variables de entorno.
```
-> env | grep -i maven
PWD=/opt/apache-maven-3.9.15
M2_HOME=/opt/apache-maven-3.9.15
MAVEN_HOME=/opt/apache-maven-3.9.15
PATH=/opt/apache-maven-3.9.15/bin:/home/devuser/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin

```

Verifique la instalación con `mvn -v`.
```
-> mvn -v
Apache Maven 3.9.15 (98b2cdbfdb5f1ac8781f537ea9acccaed7922349)
Inicio de Maven: /opt/apache-maven-3.9.15
Versión de Java: 21.0.10, proveedor: Ubuntu, tiempo de ejecución: /usr/lib/jvm/java-21-openjdk-amd64
Configuración regional predeterminada: en, codificación de plataforma: UTF-8
Nombre del sistema operativo: "linux", versión: "6.3.13-linuxkit", arco: "amd64", familia: "unix"
```

Maven ya está listo para usarse en este sistema.

## Conclusión

La utilidad maven es para construir proyectos JAVA que juega el rol de administrador que facilita la creación, el intercambio y el mantenimiento de proyectos Java. Maven es una herramienta muy conocida en la comunidad de desarrolladores de software y, como tal, está bien documentada y respaldada.

## Referencias

### Glosario de comandos.

tar
: expandir un archivo TAR

### Referencias útiles

- [Proyecto Apache Maven](https://maven.apache.org/install.html)

Paginas Manuales
- [tar](https://manpages.ubuntu.com/manpages/stonking/man1/tar.1.html)