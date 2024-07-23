---
layout: default
title: Manejando Usuarios
permalink: /manejando-usuarios/
parent: Usuarios
grand_parent: Linux
has_children: false
has_toc: false
nav_order: 2
---

# Manejando Usuarios Locales e Linux

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

## Cuentas Locales En Linux

En corto, una cuenta local es aquella que se maneja usando comandos nativos que se encuentran an nivel del sistema operativo y que no necesita de herramientas externas en la red para funcionar.

En esta leccion:
- Tomaremos un vistazo al manejo de cuentas locales
- Veremos los achivos necesarios para manejar cuentas locales
- Usaremos comandos para crear, borrar o modificar cuentas de usuarios.
- Entenderemos como mantener contraseñas.

{: .warning }
Estos ejercicios no deben hacerse en un sistema de produccíon sin entender los efectos que un cambio puede causar.

##  Tipos de Usuarios 

En Linux hay varios tipos de usuarios:
- Usuario Regular      
- Root (Super Usuario) 
- Cuenta de Sistema   

Cada tipo de usuario tiene un rol especifico con alcance diferente. 

Cuentas que requerien interaccion intensa de entrada y salida de datos necesitan poder leer y escribir archivos en diferentes areas. Otros cuentas requieren la habilidad de correr programas que pueden leer y escribir a areas restringidas. El acceso se da de acuerdo al tipo de trabajo que la cuenta requiere; una cuenta no debe tener mas aceso del que requiera para funcionar.

## Archivos Para Manejo De Cuentas Locals

Linux usa tres archivos para manejar cuentas locales
- **/etc/passwd**: información de cuentas de usuario
- **/etc/shadow**: información de seguridad de cuentas de usuario
- **/etc/group**: información de groups del sistema

{: .highlight }
Todos los archivos son legibles en texto claro

Los archivos son normalmente manejados por un Administrador de Sistemas de forma manual o programática.

Debe tenerse mucho cuidado de no causar corrupción que pueded potencialmente bloquear acceso al sistema. Por esta razón es recomendable mantener copias de los archivos en un lugar seguro.

A continuación veamos detalles de cada archivo.

### Archivo /etc/passwd

El archivo `/etc/passwd` Contiene los detalles que identifican la cuenta misma. En particular tiene el nombre del usuario, su id, y su directorio de inicio.

Este es un ejemplo en `/etc/passwd` que muestra una entrada típica para un usuario llamado `user1`.
```bash
user1:x:2015:2016:Usuario Uno:/home/user1:/bin/bash
```
La entrada contiene la siguiente información:
-  `user1`: nombre del usuario:
- `x`: indica que el usuario tiene una contraseña en `/etc/shadow`
- `2015`: el id numérico del usuario
- `2016`: el id numérico del grupo al que el usuario pertenece
- `Usuario Uno`: el campo del comentario acerca del usuario
- `/home/user1`: directorio de inicio
- `/bin/bash`: el shell que el usuario recibe al entrar al sistema

{: .highlight }
El Sistema Operativo usa el número de id para diferenciar Usuarios Regulars de Cuentas de Sistema

En Ubuntu, un número de ID menos de `100` indica que son Cuentas de Sistema. Estos ID menores de `100` son reservados para Cuentas de Sistema porque requieren permisos para acceder recursos restringidos.<br> 

El rango de ID numérico para Cuentas de Sistema es entre `0` y `999`. Muchas aplicaciones de Fuenta Abierta tales como PostgreSQL crean un usuario con número de id en el rango de los cien para darle acceso a areas tales como IPC (Inter Process Communication - RFC62) [^1]

[^1]:[Inter Process Communication - RFC62](https://datatracker.ietf.org/doc/html/rfc62)

En resúmen una Cuenta Regular tiene acceso a recursos explicitamente asignados a ella, mientras que una Cuenta de Sistema tiene acceso a más areas que incluyen recursos internos restringidos.

En Ubuntu el rango puede enforzarce al abilitar estas lineas en el archivo `/etc/login.defs`:
```bash
# System accounts
SYS_GID_MIN              100
SYS_GID_MAX              999
```

### Archivo /etc/group

El archivo `/etc/group` contine la relacion de groups a los que los usuarios pertenencen.

{: .note }
Cuando se crea un usuario, Linux crea un grupo con el mismo nombre del usuario y agrega el usuarioa ese grupo.

Este es un ejemplo en `/etc/group` que muestra una entrada típica para un usuario llamado `user1`.
```bash
-> grep user1 /etc/group
sudo:x:27:devuser,user1,user2,user3
user1:x:2016:user1
```
La entrada contiene la siguiente información:
- `user1`: el nombre del grupo.
- `x`: campo de marcador de posición usado tradicionalmente para la contraseña del grupo, pero no se usa en los sistemas modernos.
- `2016`: este es el ID de grupo (GID) del grupo.
- `user1`: lista de usuarios que son miembros del grupo user1.

En el ejemplo arriba, el nombre del grupo es `user1`, y el nombre del usuario también es `user1`. Un usuario siempre es agegado a un group primario que lleva el mismo nombre del usuario. No se puede remover al usuario de su grupo primario; tratando de hacer esa operación resulta en un error:
```
-> deluser user1 user1
/usr/sbin/deluser: You may not remove the user from their primary group.
```


### Archivo /etc/shadow

El archivo `/etc/shadow` es un archivo con vista restricta solo para `root` o usuarios con privilegios de `sudo`. Contiene la contraseña que el usuario usa para entrar al sistema.

Este es un ejemplo en `/etc/shadow` que muestra una entrada típica para un usuario llamado `user1`.
```bash
-> sudo grep user1 /etc/shadow
user1:$6$Ozq/xNc8$LADRiie3bHjAp8gkxWvOZlccGthFvmujkbpEoc4jTnf2rgAMFN5ojd2s.ZOikGJQvvF8YEnQzKXVGb2tEVOLZ0:17246:0:99999:7:::
```
La entrada contiene la siguiente información:
- `usuario1`: El nombre de usuario del usuario.
- `$6$` al principio indica que la contraseña se cifró utilizando el algoritmo hash SHA-512.
- `Ozq/xNc8$LADRiie3bHjAp8gkxWvOZlccGthFvmujkbpEoc4jTnf2rgAMFN5ojd2s.ZOikGJQvvF8YEnQzKXVGb2tEVOLZ0`: Esta es la contraseña cifrada del usuario.
- `17246`: el número de días desde la última vez que se cambió la contraseña.
- `0`: El número mínimo de días antes de que se pueda cambiar la contraseña.
- `99999`: El número máximo de días antes de que se deba cambiar la contraseña.
- `7`: La cantidad de días antes de que el usuario reciba una advertencia de que su contraseña está a punto de caducar. 
- `Campos en blanco`: Pueden contener información adicional como cuándo se cambió la contraseña por última vez, fecha de vencimiento de la cuenta, etc. En este caso, se dejan en blanco.

## Manteniendo Cuentas De Usuarios

Hay varias tareas que podemos ejecutar para manejar usuarios en Linux
- verificar la existencia de un usuario
- crear usuarios
- borrar usuarios
- modificar usuarios
- bloquear usuarios

Veamos a continuación los detalles de cada tarea.

### Verificar Usuario 

Es buena practica verificar si un usuario existe antes de atentar crearlo, o modifiarlo.

Estos comandos pueden usarse para listar todos los usuarios que existen en el sistema.
```bash
-> cut -d: -f1 /etc/passwd

-> awk -F: '{print $1}' /etc/passwd
```
Enseguida, veamos is el usuario ficticio `user1` existe.
```bash

-> id user1
-> grep user1 /etc/passwd
```
> **NOTA:** Cambia `user1` por el nombre a chequear.

### Crear Usuario

En Ubuntu y RedHat hay dos comandos para crear usuarios: `useradd` y `adduser`.

La diferencia básica entre `/usr/sbin/useradd` y `/usr/sbin/adduser` es que `useradd` es mas simple de usar, mientras que `adduser` es una interfaz más amigable e interactivo`, es decir, nos permite entrar la información requerida al momento de crear la cuenta.

El comando `useradd` se puede utilizar para crear nuevos usuarios con configuraciones y opciones _predeterminadas_ especificadas en la línea de comando o en archivos de configuración. Requiere especificar todos los parámetros _explícitamente_ al crear una nueva cuenta de usuario.

Por otro lado, `adduser` es un comando más versátil que solicita al usuario la información necesaria para crear una nueva cuenta de usuario, como el nombre de usuario, la contraseña y la ubicación del directorio de inicio. También proporciona más opciones para personalizar la cuenta de usuario durante el proceso de creación.

En general, `adduser` es más conveniente y fácil de usar para crear nuevos usuarios, mientras que `useradd` es más adecuado para secuencias de comandos o **_procesos automatizados_** donde no se requiere entrada manual.

En los ejemplos que siguen usaremos los parámetros siguientes:
- nombre de usuaro: `appuser`
- directorio de inicio: `/home/appuser`

#### Crear Usuario Con Adduser

Para crear un usuario interactivamente, simplemente pasemos el nombre del usuario como argumento al comando `adduser`.

> Tanto el id del usuario como el id del group son generados automáticamente

```yaml
-> adduser appuser
Adding user `appuser' ...
Adding new group `appuser' (1000) ...
Adding new user `appuser' (1000) with group `appuser' ...
Creating home directory `/home/appuser' ...
Copying files from `/etc/skel' ...
New password:
Retype new password:
passwd: password updated successfully
Changing the user information for appuser
Enter the new value, or press ENTER for the default
	Full Name []: App User
	Room Number []: R101
	Work Phone []: (111) 111-111
	Home Phone []: (222) 222-222
	Other []: For Application Execution
Is the information correct? [Y/n] y
```
En la tarea anterior hemos entrado la informacíon siguiente:
- Nombre completo (Full Name)
- Número de cuarto (Room Number)
- Teléfono de Trabajo (Work Phone)
- Teléfono de Casa (Home Phone)
- Otros (Other)

{: .note }
Entrar la información anterior es completamente opcional. La cuenta puede crearse sin esos detalles.

Toda esa información se agrega al campo del comentario on `/etc/passwd`.
Si deseas, no entres esos detalles, y solo presiona `Enter` o `Return` por cada pregunta.

Luego verificamos que el usuario fue creado
```bash
-> grep appuser /etc/passwd
appuser:x:1000:1000:App User,R101,(111) 111-111,(222) 222-222,For Application Execution:/home/appuser:/bin/babash
```

Aqui verificamos que el directorio de inicio fue creado. 
```bash
-> ls -ld /home/appuser
drwxr-x--- 2 appuser appuser 4096 May 18 21:08 /home/appuser/

-> ls -la /home/appuser/
total 24
drwxr-x--- 2 appuser appuser 4096 May 18 21:08 ./
drwxr-xr-x 1 root    root    4096 May 18 21:08 ../
-rw-r--r-- 1 appuser appuser  220 May 18 21:08 .bash_logout
-rw-r--r-- 1 appuser appuser 3771 May 18 21:08 .bashrc
-rw-r--r-- 1 appuser appuser  807 May 18 21:08 .profile
```

El comando `adduser` mostró la linea `Copying files from /etc/skel` indicando la acción que copió los archivos escondidos que muestra el comando `ls -la`. Cabe notar que el comando `useradd` no hace esto.

Vemos que la contraseña encripta a sido creada
```bash
-> grep appuser /etc/bashadow
appuser:$y$j9T$.aN1RbQXlVwzRNvI18uCR.$mPzqtEz2WTyb03eJ2.C7lsroaAjFdfdAh.TssUyYZs7:19861:0:99999:7:::
```

El grupo del usuario se ha creado automáticamente
```bash
-> grep appuser /etc/group
appuser:x:1000:
```

#### Crear Usuario Con Useradd

El comando `useradd` no es interactivo. Simplemente pasemos el nombre usuario como argumento.
```bash
-> useradd techuser
```

Lo primero que notamos es que el campo donde van los comentarios esta vacio.
```bash
-> grep techuser /etc/passwd
techuser:x:2096:2097::/home/techuser:/bin/sh
```

El directorio de inicio del usuario _no fue creado_. Lo debemos crear manualmente!
```bash
->  ls -l /home/techuser
ls: cannot access '/home/techuser': No such file or directory
```

Luego vemos que no se ha generado la contraseña encripta. 
```bash
-> grep techuser /etc/shadow
techuser:!:19861:0:99999:7:::
```

Tampoco se agregaron los detalles en los ultimos dos campos de la entrada.

El símbolo `!` en esta entrada en `/etc/shadow` representa que la contraseña de la cuenta "techuser" está bloqueada. 
Esto significa que el usuario no podrá iniciar una sesión con una contraseña, puesto que no existe.

{: .warning }
El usuario se ha creado sin contraseña y debemos crearla nosotros mismos usando el commando `passwd`.

Para crear la contraseña del usuario usemos el comando `passwd`. <br>
Debemos entrar la nueva contraseña dos veces.
```bash
-> passwd techuser
New password:
Retype new password:
passwd: password updated successfully
```

Verifiquemos que la contraseña se creo.
```bash
-> grep techuser /etc/shadow
techuser:$y$j9T$gQtiW96hN/wjuClWzU0sr.$/NSWKHNtTg01XtCVUPrBrC4jBM3lPa.bqx21d/evRq4:19861:0:99999:7:::
```

### Modificar Usuario

El comando `chage` se utiliza para cambiar la configuración de caducidad y antigüedad de la contraseña de una cuenta de usuario. Este comando se puede utilizar para establecer atributos específicos como la fecha de vencimiento de la contraseña, la antigüedad mínima y máxima de la contraseña, el período de advertencia y el período inactivo para una cuenta de usuario.

El comando `usermod` se utiliza para modificar varios atributos de una cuenta de usuario existente, como el nombre de usuario, UID, grupos, directorio de inicio, shell, fecha de vencimiento y más. Este comando proporciona un conjunto más completo de opciones para administrar cuentas de usuario en comparación con `chage`.

Si bien ambos comandos se pueden usar para modificar los atributos de la cuenta de usuario, el comando `chage` se centra principalmente en administrar _la configuración de antigüedad de la contraseña_, mientras que el comando `usermod` es más general y versátil para realizar diversas modificaciones en las cuentas de usuario.

Veamos a continuacion mas detalles de chage and usermod.

#### Modificar Usuarios con CHAGE

En sistemas the Linux usamos el comando `chage` para modificar atributos a cuentas de usuarios que ya existen. Este comando requiere que esté disponible el archivo de contraseña oculta `/etc/shadow`.<br>
El comando `chage` está restringido al usuario `root`, excepto la opción `-l`, que puede ser utilizado por un usuario sin privilegios para determinar cuándo caducará su contraseña o cuenta.

Aqui, el usuario `appuser` usa el comando `chage -l` para chequear el estado de su cuenta. 
```bash
appuser@ubuntu2204-1-devesp:~$ chage -l appuser
Last password change                                      : May 18, 2024
Password expires                                          : never
Password inactive                                         : never
Account expires                                           : never
Minimum number of days between password change            : 0
Maximum number of days between password change            : 99999
Number of days of warning before password expires         : 7
```
En particular, muestra cuando caducará su contraseña, en esta caso nunca.

Ahora agamos algunas cambios de fecha y tiempo.

Establecer la fecha de vencimiento de la cuenta
```bash
-> chage --expiredate 2024-12-31 appuser
```
Establecer el numero de 10 dias como aviso antes que venza la contraseña.<br>
Indica que el usuario recibirá un aviso por diez días antes que se venza su contraseña.
```bash
-> chage  --warndays 10  appuser
```
Establecer el número mínimo de días antes de cambiar la contraseña.<br>
Este ajuste dice la contraseña caduca llegado el numero de dias indicado.
```bash
-> chage  --mindays 180  appuser
```
Establecer el número máximo de días antes de cambiar la contraseña.<br>
Este ajuste dice el usuario debe cambiar la contraseña cuando se cumple el número de dias indicado.
```bash
-> chage  --maxdays 200  appuser
```
Establecer el número de dias que la cuenta puede estar inactiva en el sistema.<br>
Esto indica el usuario (la contraseña) puede estar inactivo por no más del número de dias indicado.
```bash
-> chage  --inactive 30  appuser
```
Todas las configuraciones anteriores se pueden agrupar en un solo comando así.
```bash
-> chage --expiredate 2024-12-31 --warndays 10 --mindays 180 --maxdays 200 --inactive 30  appuser
```

Ahora, veamos todos los cambios hechos.
```bash
-> chage -l appuser
Last password change                                      : May 18, 2024
Password expires                                          : Dec 04, 2024
Password inactive                                         : Jan 03, 2025
Account expires                                           : Dec 31, 2024
Minimum number of days between password change            : 180
Maximum number of days between password change            : 200
Number of days of warning before password expires         : 10
```

Todos los cambion se ven en `/etc/shadow` en los campos correspondientes.
```bash
-> grep appuser /etc/shadow
appuser:$y$j9T$P2Wk5imw0AqKdMnzMHCoz.$vJxs5FqmYoorHrUAgN18Qq2Z98Xq00BmAK4/lkqHQG1:19861:180:200:10:30:20088:
```

Para más información ver la ayuda en linea del comando.
```bash
->  chage --help
```

#### Modificar Usuarios con USERMOD

La página manual del comando `usermod` dice lo siguiente:
"_El comando usermod modifica los archivos de la cuenta del sistema para reflejar los cambios que se especificado en la línea de comando._"

Cambiemos el UID a `2000` para `appuser`
```bash
-> usermod --uid 2000 appuser
```

Cambiemos la contraseña de `appuser` a `secret`. El cambio se refleja en `/etc/shadow`.
```bash
-> usermod --password secret appuser

-> grep appuser /etc/shadow
appuser:secret:19861:180:200:10:30:20088:
```

Agreguemos el ususario `appuser` al group `staff` en adición a los que ya pertenece.
```bash  
-> usermod --groups  staff appuser
-> id appuser
uid=2000(appuser) gid=1000(appuser) groups=1000(appuser),50(staff)
```
El comando `id <usuario>` muestra todos los grupos a los que un usuario pertence.

Enseguida, borremos el usuario `appuser` del group `staff`.<br>
Para borrar un usuario de un grupo, hay que listar los groups a los que queremos mantenar y dejar fuera los que queremos borrar. En este ejemplo, el `appuser` pertencera solo al `appgroup`.
```bash
usermod --groups appgroup appuser
```

### Bloquer Usuario

Podemos bloquear o desbloquear un usuario con el commando `usermod`.<br>
- El ajuste `--lock` bloquea
- El ajuste `--unlock` desbloquea

```bash
-> usermod --lock appuser
-> usermod --unlock appuser
```

Para más información ver la ayuda en linea del comando.
```bash
->  usermod --help
```

Tambien podemos bloquer o desbloquer con el commando `passwd`.<br>
Para bloquear un usuario usemos la bandera `-l` asi:
```bash
-> passwd -l techuser
```

Podemos chequear el estado del usuario de esta manera:
```bash
-> grep techuser /etc/shadow
techuser:!$y$j9T$gQtiW96hN/wjuClWzU0sr.$/NSWKHNtTg01XtCVUPrBrC4jBM3lPa.bqx21d/evRq4:19861:0:99999:7:::
```
El símbolo `!` se agregó despues del primer `:` indicando esta cuenta esta bloqueada y no puede usarse para entrar al sistema.

Para remover el bloqueo de un usuario usando `passwd` usemos la bandera `-u` asi:
```bash
-> passwd -u techuser
passwd: password expiry information changed.
```
El símbolo `!` se remueve después de esta operación indicando que esta cuenta ha sido reabilitada para entrar al sistema.

### Borrar Usuario

En Ubuntu y RedHat hay dos comandos para crear usuarios: `deluser` y `userdel`.

{: .warning }
Debemos decidir si deseamos mantener or borrar el directorio de inicio cuando borramos una cuenta de usuario en Linux

En los dos ejemplos que siguen, el directorio de inicio es preservado.

El comando `deluser` pregunta el nombre del usuario a borrar.
```bash
-> deluser
Enter a user name to remove: appuser
Removing user `appuser' ...
Warning: group `appuser' has no more members.
Done.
```

El comando `userdel` requiere que pasemos el nombre del usuario a borrar como parámetro.
```bash
-> userdel techuser
```

En algunos casos es importante preservar los datos que un usuario a colectado durante su trabajo. Lo mas posible es que los datos se encuentren en el directorio de inicio del usuario. Al borrar el usuario podemos pasar una opción al comando `deluser` para crear una copia de archivo de los datos.

En este ejemplo que sigue hacemos lo siguiente:
- Usamos el comando `mkdir` para crear `/tmp/appuser` como destinación para la copia de seguridad del directorio del usuario
- usamos la opción `--backup-to` para hacer una copia de seguridad en el directorio `/tmp/appuser`
- usamos la opción `--remove-home` para borrar el directorio de inicio del usuario
- borramos el usuario `appuser`
- usamos los comandos `ls` y `tar` para verificar que la copia de seguridad fue creada

```bash
-> mkdir /tmp/appuser

-> deluser appuser  --remove-home  --backup-to /tmp/appuser
Looking for files to backup/remove ...
Backing up files to be removed to /tmp/appuser ...
backup_name = /tmp/appuser/appuser.tar
/bin/tar: Removing leading `/' from member names
/bin/tar: Removing leading `/' from hard link targets
Removing files ...
Removing user `appuser' ...
Warning: group `appuser' has no more members.
Done.

-> ls -l /tmp/appuser/appuser.tar.gz
-rw------- 1 root root 2384 May 18 22:12 /tmp/appuser/appuser.tar.gz

-> tar -tzvf /tmp/appuser/appuser.tar.gz
-rw-r--r-- appuser/appuser 3771 2024-05-18 21:08 home/appuser/.bashrc
-rw-r--r-- appuser/appuser  807 2024-05-18 21:08 home/appuser/.profile
-rw-r--r-- appuser/appuser  220 2024-05-18 21:08 home/appuser/.bash_logout
-rw------- appuser/appuser   51 2024-05-18 22:10 home/appuser/.bash_history
-rw-rw-r-- appuser/appuser   29 2024-05-18 22:10 home/appuser/myfile
```
El comando solo eliminó el usuario `appuser` y eliminó su directorio de inicio mientras realizó una copia de seguridad del contenido en `/tmp/appuser`.

Finalmente, usemos el comando `id` para verificar que el usuario has sido borrado.
```bash
-> id appuser
id: 'appuser': no such user
```

## Conclusion

Linux provee todas las herramientas necesarios para manejar acceso al sistema. Se debe tener en consideración el tipo de acceso que una cuenta requiere y solo dar el acceso mínimo para que el usuario funcione sin impedimento.

Todos los comandos para manejar cuentas estas disponibles en Ubuntu y RedHat.

Está bien usar Cuentas Locales en sistemas de uso personal, de desarrollo, laboration o ambientes pequeños, pero en ambientes mas complejos es recomendable experimentar con herramientas mas sólidas y seguras tales come LDAP o AD.

## Referencias 

### Glosario De Comandos

Los comandos siguientes son usados frecuentemente en sesiones de Linux.

adduser, useradd
: agregar cuenta de usuario en Linux

deluser, userdel
: borrar cuenta de usuario en Linux

passwd
: dar or cambinar contraseña de usuario en Linux

mkdir
: crear directorio

grep, awk, cut
: buscar cadena de letras

### Referencias Utiles

Paginas Manuales (Ubuntu)

- [adduser](https://manpages.ubuntu.com/manpages/focal/en/man8/adduser.8.html)
- [useradd](https://manpages.ubuntu.com/manpages/focal/en/man8/useradd.8.html)
- [deluser](https://manpages.ubuntu.com/manpages/focal/en/man8/deluser.8.html)
- [userdel](https://manpages.ubuntu.com/manpages/focal/en/man8/userdel.8.html)
- [passwd](https://manpages.ubuntu.com/manpages/focal/en/man5/passwd.5.html)
- [shadow](https://manpages.ubuntu.com/manpages/focal/en/man5/shadow.5.html)
- [chage](https://manpages.ubuntu.com/manpages/focal/en/man1/chage.1.html)
- [usermod](https://manpages.ubuntu.com/manpages/focal/en/man8/usermod.8.html)
