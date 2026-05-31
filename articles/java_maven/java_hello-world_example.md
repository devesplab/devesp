---
layout: default
title: Java Ejemplo - Hola Mundo
permalink: /java-helloworld/
parent: Articulos
has_children: false
has_toc: false
nav_order: 2
---

## Ejemplo Básico de Java en Ubuntu

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

Este tutorial proporciona orientación básica sobre el uso de la cadena de herramientas de Java para el desarrollo en Ubuntu. Muestra cómo crear un programa "¡Hola, mundo!" y explica cómo crear proyectos utilizando Maven.

Hablamos sobre la configuración y construcción de un nuevo proyecto Java utilizando la herramienta **Apache Maven**.

En esta lección:

- Entendimiento básico de que es [Maven Central](#que-es-maven-central)
- Definición básica de un [archivo POM](#que-es-archivo-de-pom)
- Utilizar el comando `mvn` para crear un esqueleto de proyecto Java
- Compilar el proyecto.
- Ejecute la aplicación Java

**DEPENDENCIAS**

ninguno

**REQUISITOS**

- Kit de desarrollo Java; consulte Instalación del kit de desarrollo de Java.
- Apache Maven
- acceso a internet

**ADVERTENCIA**

El ejemplo descrito aquí no funcionará sin una conexión a Internet. Maven requiere una conexión a Maven Central.

## Ambiente de trabajo

Esta lección se realiza en un sistema Linux Ubuntu.

```bash
-> lsb_release -a
Distributor ID:	Ubuntu
Description:	Ubuntu 24.04.4 LTS
Release:	24.04
Codename:	noble
```

## Cuando Usar Maven

Usa Maven cuando necesites una herramienta de compilación estandarizada, declarativa y gestión de dependencias para proyectos Java (y JVM).

Por ejemplo:

- construir proyectos de Java
- integrar con ambientes the CI/CD
- publicar artefactos a Maven Central

## Creando un proyecto Java usando Maven

Crea un directorio para contener el proyecto y cambie al directorio,

```bash
mkdir myJavaProject && cd myJavaProject
```

Crea un nuevo proyecto Java pasando el argumento `archetype:generate` a maven:

```bash
mvn archetype:generate -DgroupId=com.yourcompany \
     -DartifactId=helloworld -Dversion=1.0-SNAPSHOT \
     -Dpackage=com.yourcompany.helloworld \
     -DarchetypeGroupId=org.apache.maven.archetypes \
     -DarchetypeArtifactId=maven-archetype-quickstart \
     -DarchetypeVersion=1.0
```

> Vea la definición básica de [Arquetipo Maven](#que-es-arquetipe-de-maven).

Salida esperada de la generación del arquetipo (listado parcial):

```log
...snip...
[INFO] Using property: groupId = com.yourcompany
[INFO] Using property: artifactId = helloworld
[INFO] Using property: version = 1.0-SNAPSHOT
[INFO] Using property: package = com.yourcompany.helloworld
Confirm properties configuration:
groupId: com.yourcompany
artifactId: helloworld
version: 1.0-SNAPSHOT
package: com.yourcompany.helloworld
[INFO] ----------------------------------------------------------------------------
[INFO] Using following parameters for creating project from Old (1.x) Archetype: maven-archetype-quickstart:1.0
[INFO] ----------------------------------------------------------------------------
[INFO] Parameter: basedir, Value: /home/devuser/myJavaProject
[INFO] Parameter: package, Value: com.yourcompany.helloworld
[INFO] Parameter: groupId, Value: com.yourcompany
[INFO] Parameter: artifactId, Value: helloworld
[INFO] Parameter: packageName, Value: com.yourcompany.helloworld
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] project created from Old (1.x) Archetype in dir: /home/devuser/myJavaProject/helloworld
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  10.305 s
[INFO] Finished at: 2026-05-09T16:35:40Z
[INFO] ------------------------------------------------------------------------
```

Presione el teclado `Enter` cuando se le solicite que confirme su selección.

Esto crea un nuevo proyecto utilizando **Maven Quickstart Archetype**.

Maven establece una estructura básica del proyecto:

```bash
devuser@client1
~/myJavaProject
hist:37 -> tree
.
└── helloworld
    ├── pom.xml
    └── src
        ├── main
        │   └── java
        │       └── com
        │           └── yourcompany
        │               └── helloworld
        │                   └── App.java
        └── test
            └── java
                └── com
                    └── yourcompany
                        └── helloworld
                            └── AppTest.java

13 directories, 3 files
```

Eso incluye una aplicación 'Hello World' y una prueba unitaria:

ARCHIVO:`helloworld/src/main/java/com/yourcompany/helloworld/App.java`

```java
package com.yourcompany.helloworld;

/**
 * Hello world!
 *
 */
public class App
{
    public static void main( String[] args )
    {
        System.out.println( "Hello World!" );
    }
}
```

ARCHIVO:`src/test/java/com/yourcompany/helloworld/AppTest.java`

```java
package com.yourcompany.helloworld;

import junit.framework.Test;
import junit.framework.TestCase;
import junit.framework.TestSuite;

/**
 * Unit test for simple App.
 */
public class AppTest
    extends TestCase
{
    /**
     * Create the test case
     *
     * @param testName name of the test case
     */
    public AppTest( String testName )
    {
        super( testName );
    }

    /**
     * @return the suite of tests being tested
     */
    public static Test suite()
    {
        return new TestSuite( AppTest.class );
    }

    /**
     * Rigourous Test :-)
     */
    public void testApp()
    {
        assertTrue( true );
    }
}
```

Vaya al directorio que contiene el proyecto.

```bash
devuser@client1
~/myJavaProject
hist:43 -> cd helloworld/
```

El proyecto contiene un archivo POM listo para usar.<br>

ARCHIVO: `~/myJavaProject/helloworld/pom.xml`

```log
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.yourcompany</groupId>
  <artifactId>helloworld</artifactId>
  <packaging>jar</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>helloworld</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```

Compile y empaquete la aplicación:

```bash
mvn -Dmaven.compiler.release=8 package
```

Esto construye y ejecuta pruebas unitarias.

{: .note }
> Maven descargará las dependencias requeridas desde el [Repositorio Central de Maven](https://repo.maven.apache.org/maven2) que es el repositorio remoto predeterminado utilizado por Maven si no se declaran repositorios en el archivo POM. Maven comprueba el archivo `~/.m2/repository` primero y solo descarga artefactos de Maven Central si el archivo no existe.

Salida esperada de `mvn` (listado parcial):

```log
...snip...
[INFO] Building jar: /home/devuser/myJavaProject/helloworld/target/helloworld-1.0-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  6.800 s
[INFO] Finished at: 2026-05-09T16:45:02Z
[INFO] ------------------------------------------------------------------------
```

El resultado indica que el objetivo se compiló correctamente.

Se crea un nuevo directorio llamado "target".

```bash
devuser@client1
~/myJavaProject/helloworld
hist:45 -> ls -l
total 12
-rw-rw-r-- 1 devuser devuser  648 May  9 16:35 pom.xml
drwxrwxr-x 4 devuser devuser 4096 May  9 16:35 src/
drwxrwxr-x 9 devuser devuser 4096 May  9 16:45 target
```

El directorio `target` contiene el código de compilación, que incluye el ejecutable JAR.

```bash
devuser@client1
~/myJavaProject/helloworld
hist:46 -> tree target
target
├── classes
│   └── com
│       └── yourcompany
│           └── helloworld
│               └── App.class
├── generated-sources
│   └── annotations
├── generated-test-sources
│   └── test-annotations
├── helloworld-1.0-SNAPSHOT.jar
├── maven-archiver
│   └── pom.properties
├── maven-status
│   └── maven-compiler-plugin
│       ├── compile
│       │   └── default-compile
│       │       ├── createdFiles.lst
│       │       └── inputFiles.lst
│       └── testCompile
│           └── default-testCompile
│               ├── createdFiles.lst
│               └── inputFiles.lst
├── surefire-reports
│   ├── 2026-05-09T16-45-00_616-jvmRun1.dumpstream
│   ├── TEST-com.yourcompany.helloworld.AppTest.xml
│   └── com.yourcompany.helloworld.AppTest.txt
└── test-classes
    └── com
        └── yourcompany
            └── helloworld
                └── AppTest.class

21 directories, 11 files
```

Ejecute la aplicación:

```bash
-> java -cp target/classes com.yourcompany.helloworld.App
Hello World!
```

## Solución de problemas en un contenedor Docker

Si se ejecuta en un contenedor, Java podría lanzar un error como este:

```log
[0.001s][warning][os,container] Cgroup cpu controller path at '/sys/fs/cgroup' seems to \
have moved to '/../../user.slice/user-1001.slice/session-c3.scope', detected limits won't be accurate
```

Para silenciar ese mensaje, crea el archivo `/etc/profile.d/java.sh` con la configuración para la compatibilidad con CGroup.

```bash
-> sudo tee /etc/profile.d/java.sh > /dev/null <<'EOF'
export _JAVA_OPTIONS='-XX:+UnlockDiagnosticVMOptions -XX:-UseContainerSupport'
EOF
```
Asegúrese de que el archivo sea propiedad de root y sea legible:

```bash
sudo chown root:root /etc/profile.d/java.sh 

sudo chmod 644 /etc/profile.d/java.sh
```

Los nuevos shells de inicio de sesión lo detectarán automáticamente; para aplicarlo ahora a su shell actual:

```bash
-> source /etc/profile.d/java.sh
```

Verifique la configuración.

```bash
-> echo "$_JAVA_OPTIONS"
-XX:+UnlockDiagnosticVMOptions -XX:-UseContainerSupport
```

Ejecuta la aplicación Java.

```bash
-> java -cp target/classes com.yourcompany.helloworld.App
Picked up _JAVA_OPTIONS: -XX:+UnlockDiagnosticVMOptions -XX:-UseContainerSupport
Hello World!
```

## Conclusion

Tras configurar el entorno Java tal como se explica en este documento, deberíamos poder crear proyectos Java y ejecutar la aplicación.

## Referencias 

### Que es Maven Central? {#que-es-maven-central}

El Repositorio Central de Maven es el repositorio principal público de bibliotecas y artefactos para Java y la JVM. Los desarrolladores y las herramientas de construcción (Maven, Gradle, SBT, etc.) obtienen de él las dependencias publicadas y pueden publicar allí sus propios artefactos finalizados para que otros puedan consumirlos.

Sus características principales son:

- Aloja artefactos binarios versionados (archivos JAR), metadatos (archivos POM) y sumas de verificación (checksums).
- Utiliza un sistema de coordenadas `groupId:artifactId:version` para identificar los artefactos.
- Se accede a él a través de HTTP(S); las herramientas de construcción utilizan automáticamente los puntos de acceso (endpoints) habituales.
- Aplica reglas de firma/validación y de publicación (a través de Sonatype OSSRH para proyectos nuevos) para el despliegue oficial en el Repositorio Central.
- Es ampliamente replicado y almacenado en caché por redes de distribución de contenidos (CDN) para garantizar su disponibilidad global.

El uso típicos de Maven Central es la resolución de dependencias durante el proceso de construcción, recuperación de dependencias transitivas y distribución de bibliotecas.

### Definición básica de un archivo POM {#que-es-archivo-de-pom}

Un archivo POM (Project Object Model) es un pequeño archivo XML que le indica a Maven qué es su proyecto y qué necesita. 

En términos sencillos:

- Identifica el proyecto (grupo, artefacto, versión).
- Enumera las bibliotecas de las que depende su código para que Maven pueda descargarlas.
- Define cómo se debe construir el proyecto (tipo de empaquetado, complementos, pasos de construcción).
- Puede contener metadatos del proyecto (nombre, URL, desarrolladores), así como la configuración de repositorios y perfiles.

Los archivos POM le proporcionan a Maven todos los elementos y componentes necesarios para construir la aplicación.

### Definición básica de Arquetipo Maven Quickstart {#que-es-arquetipe-de-maven}

El Arquetipo Maven Quickstart es una plantilla de proyecto (arquetipo) que genera una estructura mínima y estándar de proyecto en Java Maven con archivos básicos para empezar rápidamente.

Genera: 

- Diseño estándar de directorios (`src/main/java`, `src/test/java`, `recursos`) 
- Un ejemplo `App.java` y `AppTest.java` 
- un `pom.xml` con `groupId`, `artifactId`, `version`, empaquetado y configuración básica de compilación/prueba 
El propósito es proporcionar un punto de partida sencillo y convencional para nuevos proyectos Java para que puedas ejecutar compilación/prueba/paquete mvn inmediatamente. 

Comando común para generar uno:

```
mvn archetype:generate -DgroupId=com.example -DartifactId=my-app \
  -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

```

### Propósito del directorio de destino (target)

El directorio de destino (target directory) es la carpeta de salida de la compilación utilizada por las herramientas de construcción de Java (Maven, por defecto) para almacenar todos los archivos generados durante el proceso de construcción. Su propósito y contenido típico son:

- Clases compiladas: archivos .class generados a partir de los archivos fuente .java (p. ej., target/classes).
- Artefactos empaquetados: archivos JAR, WAR o EAR producidos durante la construcción (p. ej., target/myapp-1.0.jar).
- Resultados de pruebas: clases de prueba compiladas e informes de pruebas (p. ej., target/test-classes, surefire-reports).
- Archivos temporales de construcción: fuentes generadas, copias de recursos, archivos comprimidos expandidos y directorios de trabajo específicos de los plugins.
- Metadatos de construcción: sumas de verificación (checksums), archivos de manifiesto y otros artefactos utilizados por el ciclo de vida de la construcción.

## Glosario De Commandos

`java`
: el binario de Java para ejecutar aplicaciones Java

`mvn`
: la utilidad Maven para construir proyectos Java

### Referencias Útiles

- [Apache Maven](https://maven.apache.org/index.html) Project.
  - [Maven Archetypes](https://maven.apache.org/archetypes/index.html) list.
  - [Running Apache Maven](https://maven.apache.org/run.html)
- Official documentation for [Canonical’s build of OpenJDK for Ubuntu](https://ubuntu.com/toolchains/java)

[Return to main page]({{site.baseurl}}/)
