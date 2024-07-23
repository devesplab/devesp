---
layout: default
title: Permisios De Ficheros y Directorios
permalink: /permisos_ficheros_directorios/
parent: Ficheros y Carpetas
grand_parent: Linux
has_children: true
has_toc: false
nav_order: 2
---

# Permisos De Ficheros y Directorios

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

## Importancia de Entender Permisos y Propiedades de Ficheros y Directorios

No podemos funcionar óptimamente en el entorno de Linux sin entender que son permisos y propiedades de Ficheros. Debemos tener un entendimiento mínimo del propósito de Umask y porque sirve establecer permisos de ficheros. En esta página veremos las utilidades nativas disponibles en Linux para establecer permisos de Ficheros y Directorios

Comprender los permisos de archivos y directorios en Linux es importante por varias razones:

**Seguridad:** los permisos de archivos y directorios determinan quién puede acceder, modificar y ejecutar archivos y directorios. Al configurar los permisos adecuados, puede controlar y restringir el acceso a datos confidenciales y evitar que usuarios no autorizados realicen cambios en archivos importantes.

**Integridad de los datos:** los permisos de archivos y directorios ayudan a garantizar que usuarios no autorizados no modifiquen ni eliminen accidentalmente archivos importantes. Al establecer los permisos adecuados, puede proteger sus datos para que no sean manipulados o perdidos.

**Cumplimiento:** Ciertas regulaciones o estándares de seguridad, como GDPR o PCI DSS, pueden requerir permisos de archivos y directorios. Al comprender e implementar los permisos adecuados, puede garantizar el cumplimiento de estas regulaciones y proteger a su organización de posibles multas o sanciones.

**Colaboración:** comprender los permisos de archivos y directorios es fundamental para una colaboración eficaz en un entorno multiusuario. Al configurar los permisos correctamente, puede permitir o restringir el acceso a archivos y directorios específicos, lo que permite a los usuarios trabajar juntos en proyectos mientras se mantiene la seguridad de los datos.

## Que son permisos y propiedad de Ficheros

Hay varias acciones que un usuario usualmente quiere hacer cuando trabaja con Ficheros y Carpetas en un ambiente de Linux:
- crear, acceder, renombrar, mover or boarrar un archivo o una carpeta
- leer un archivo
- escribir a un archivo para actualizarlo
- ejecutar un programa para hacer alguna tarea

Cada una de esas actividades es controlada al nivel del usuario dueño de los recursos, de un grupo de usuarios o el resto de usuarios en el sistema que no pertenecen a un grupo en particular.

En Linux cada uno de esas actividades recibe un valor numérico.

| Tipo De Permiso       | Valor Numérico|
| ----------------------|---------------|
| (r)ead    (leer)      |       4       |
| (w)rite   (escribir)  |       2       | 
| e(x)ecute (ejecutar)  |       1       | 

Estos valores se pueden sumar para asignar los permisos deseados.<br>
Si queremos dar permiso de leer solamente, usamos el valor 4.<br>
Si queremos dar permiso de leer y escribir juntamente, usamos el valor agregado de 4 y 2, es decir seis: `(4+2)=6`.<br>
Para dar permiso completo de leer, escribir y ejecutar, usames el valor agregado de 4, 2 y 1, es decir siete: `(4+2+1)=7`.<br>

Los permisos pueden darse a tres categorias de usuarios: 
- **dueño:** el individuo que ha creado o se le ha asignado los recursos 
- **grupo:** un grupo de usuarios en el sistema al que se le da acceso a los recursos pertenecientes a un usuario que puede o no ser parte del grupo.
- **otros:** o resto del mundo, todos los demas usuarios que no son dueños de los recursos

El tópico siguiente explica el origen de esos valores numéricos.

## Entendiendo UMASK

Antes de entender y aplicar permisos, debemos entender que es UMASK y para que sirve.

{: .note }
UMASK es la máscara de creación del modo de archivo.

En Linux existe un número llamdo UMASK que es _la máscara de creacion del modo de archivo_ en el ambiente del usuario. Linux usa este valor por defecto para establecer permisos cuando creamos ficheros y directorios en nuestro ambiente.

Tanto en Ubuntu como en RedHat podemos encontrar el valor por defecto the `UMASK` en el archivo `/etc/login.defs`.
```bash
-> grep -i umask /etc/login.defs
#	UMASK		Default "umask" value.
# UMASK is the default umask value for pam_umask and is used by
# 022 is the "historical" value in Debian for UMASK
# If USERGROUPS_ENAB is set to "yes", that will modify this UMASK default value
UMASK		022	
```

Asi que, por defecto el valor de umask in Linux es `022`.

El valor numérico actual cuando creamos archivos y directories es governado por el valor de UMASK. Por defecto, el valor en `/etc/login.defs` se aplica al sistema entero para todos los usuarios.

{: .note }
El valor de UMASK puede cambiarse.

Aunque no es recomendable, el valor de UMASK puede cambiarse de acuerdo a la política local de Administración de Sistemas. Cambiando el valor no afecta recursos que ya existen, solo afectara recursos creados despues del cambio. 

Un usuario puede cambiar el umask en su propio ambiente al editar `~/.bashrc` y agregar un nuevo valor.
```bash
-> vi .bashrc
```
Agregar esta linea con el valor deseado.
```bash
umask 011
```
Releer la configuración para activar el cambio.
```bash
-> source .bashrc
```
Para ver el valor en nuestro ambiente podemos esar el comando `umask`.
```bash
-> umask
0011
```
Linux usa los tres últimos números, contando de derecha a izquierda, para calcular los permisos, en este ejemplo usara `011`.

## Permisos Predeterminados

Los permisos en Linux son el mecanismo usado para controlar acceso a recursos entre los usarios que entran al sistema.

Los suma de permisos son a seguir: 
- carpetas: `777`
- archivos: `666`

El `7` se traduce a `rwx` que indica leer, escribir, y ejecutar.<br>
El `6` se traduce a `rw-` que indica leer, escribir, no ejecutar.

Cada número aplica a una categoría de usuarios
```
    7        7      7
    6        6      6
[usuario] [grupo] [mundo]
```

Usando el valor por defecto the UMASK de `022` podemos ver los permisos que obtenemos.<br>

### Permisos Predeterminados de Carpetas

Básicamente, sustraemos la umask de `777` y nos da los permisos de ficheros. 

Cuando creamos un fichero los permisos seran `rwx rwx r-x`.
```
    0777 --> rwx rwx rwx
  - 0022 
  ------
    0775 --> rwx rwx r-x
```
Los valores indican lo siguiente:
- el usuario, como dueño obtiene acceso completo de leer, escribir y ejecutar
- el group, obtiene acceso completo de leer, escribir y ejecutar
- el mundo, obtiene acceso completo de leer y ejecutar, pero no escribir

{: .important }
Una carpeta no se puede acceder si no tiene permiso de ejecutar.

El permiso de ejecutar de una carpeta permite que podamos accederla. Si una carpeta no tiene persimo de ejecutar para un grupo de usuarios, entonces esos usuarios no pueden cambiar a esa carpeta. 

### Permisos Predeterminados de Ficheros

Básicamente, sustraemos la umask de `666` y nos da los permisos de archivo. 

Cuando creamos un archivo los permisos seran `rw- rw- r--`.

```
    0666 --> rw- rw- rw-
  - 0022
  ------
    0664 --> rw- rw- r--
```
Los valores indican lo siguiente:
- el usuario, como dueño obtiene acceso de leer y escribir, pero no ejecutar
- el group, obtiene acceso de leer y escribir, pero no ejecutar
- el mundo, obtiene acceso de leer solamente

{: .highlight }
Los permisos pueded cambiarse con el comando CHMOD.

En la sección que sigue veremos como usar el comando `chmod` para establecer permisos como lo deseamos.

## Usando CHMOD Para Asignar Permisos 

El usuario tiene libre albedrío para establecer permisos de archivos y directorios. El comando `chown` existe para ese propósito.

La sintax general es así:
```bash
-> chmod <octal> <carpeta>
-> chmod <octal> <archivo>
```
Pasamos dos argumentos al comando `chmod`:
1. el valor **octal** del permiso a aplicar
2. el fichero o directorio a cambiar

Por ejemplo:
```bash
-> chmod 750 miCarpeta
-> chmod 640 miArchivo.txt
```

El el caso anterior, hemos usado el valor octal (numérico) de `750` y `640` para establecer permisos. Si lo deseamos, en vez de números, podemos usar la letra correspondiente y el resultado será el mismo.
- u = usuario
- g = grupo
- o = otros
- a = todos (usuario, grupo, otros)

Por ejemplo:
```bash
chmod u=rwx,g=r-x,o=--- miCarpeta
chmod u=rw-,g=r--,o=--- miArchivo.txt
chmod a=rw-  myArchivo.txt
```
En este ejemplo indicamos explicitamente la categoria de usuario y el permisio que se le aplica. El símbolo `-` indica la ausencia de permiso.

Anteriormentete hemos dicho que el permiso de ejecutar de una carpeta permite que podamos accederla. Consideremos el ejemplo siguiente:
```bash
-> chmod 640 miCarpeta
```
En este caso, obtendremos un error al atentar acceder la carpeta con el comando `cd`. 
```bash
-> cd miCarpeta/
-bash: cd: miCarpeta/: Permission denied
```
La razón es que la carpeta no tiene permiso de ejecutar. Para arreglar el problema, demos el permiso necesario.
```bash
-> chmod +x miCarpeta
```

## Usando CHOWN Para Asignar Dueño

El comando `chmod` existe para aplicar permisos de propieded. El usuario tiene libre albedrío para establecer la propieded de archivos y directorios que le pertenecen. Sin embargo, esto debe ser hecho de manera juiciosa para no exponer información restringida. 

La sintax general es así:
```bash
-> chown {usuario|grupo} <carpeta>
-> chown {usuario|grupo} <archivo>
```
Pasamos dos argumentos al comando `chown`:
1. el nombre del usuario o grupo al que aplicamos el permiso
2. el fichero o directorio a cambiar

Por ejemplo:
```bash
-> chown migrupo miCarpeta
-> chown staff myArchivo.txt
```

## Conclusion

El entendimiento de los permisos de archivos y directorios en Linux es esencial para mantener la seguridad de los datos, proteger la información confidencial y garantizar el cumplimiento de las regulaciones y estándares.

Los permisos en Linux son el mecanismo usado para controlar acceso a recursos entre los usarios que entran al sistema.

El valor de umask es un valor númerico que existe por defecto en sistemas de linux. Este valor es usado para establecer permisos de ficheros y directorios.

## Referencias 

### Glosario De Comandos

Los comandos siguientes son usados frecuentemente en sesiones de Linux.

umask
: valor máscara establecida para crear ficheros y directorios

chmod
: comando para establecer permisos de ficheros y directorios

chown
: comando para establecer la propiedad de ficheros y directorios

### Referencias Utiles

Paginas Manuales
- [umask](https://manpages.ubuntu.com/manpages/focal/en/man1/umask.1posix.html)
- [chmod](https://manpages.ubuntu.com/manpages/focal/en/man1/chown.1.html)
- [chown](https://manpages.ubuntu.com/manpages/focal/en/man1/chmod.1posix.html)

[Return to main page]({{site.baseurl}}/).
