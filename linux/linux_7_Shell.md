---
layout: default
title: El Shell
permalink: /linux-shell/
parent: Linux
has_children: true
has_toc: false
nav_order: 7
---

# LINUX :: Shell :: Qué es el Shell en Linux?

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

**DESCRIPCION**

En esta leccion:
- Qué es el SHELL en Linux?
- El SHELL Predeterminado

{: .highlight }
Este documente es en el contexto del Sistema Operativo UBUNTU a menos que se aclare de otra manera.

**DEPENDENCIAS**

Usando `/usr/sbin/nologin` como opción de shell para un usuario causa que ese usuario no pueda entrar al sistema. Por lo tanto, no lo uses a menos que entiendas su effecto.

**REQUERIMIENTOS**

Sistema de linux Ubuntu. <br>

**ADVERTENCIA**

ninguna.

## Working Environment

En esta leccion usamos el sistema operativo Ubuntu.

## Qué es el SHELL en Linux?

Para poder interactuar con el sistema operativo de Linux, necesitamos la manera de entrar comandos y obtener la salida de los mismos. Para lograr esto, Linux provee lo que se conoce como el Shell, el cual es un programa que sirve para interactuar con el sistema a travez de una terminal.  

El archivo `/etc/shells` contiene la lista de shells disponibles. La lista difiere entre sistemas operativos.

En Ubuntu vemos esta lista.
```
-> cat /etc/shells
# /etc/shells: valid login shells
/bin/sh
/bin/bash
/usr/bin/bash
/bin/rbash
/usr/bin/rbash
/usr/bin/sh
/bin/dash
/usr/bin/dash
/usr/bin/screen
/usr/bin/tmux
```

En Redhat vemos esta lista.
```
-> cat /etc/shells
/bin/sh
/bin/bash
/usr/bin/sh
/usr/bin/bash
```

El sistema operativo hace disponible un shell de la lista para servir como shell predeterminado para usuarios que entran al sistema. 

Hay [^cuentas de sistema] que no necesitan un shell. En este caso especial, existe la opción de usar el comando `nologin` con la idea de _**prevenir**_ a este tipo de cuenta de entrar al sistema.

El comando `nologin` esta presente en Ubuntu y RedHat.
```
devuser@ubuntu2204-1-devesp 
~/linux-devesp
hist:204 -> ls -l `which nologin`
-rwxr-xr-x 1 root root 14640 Feb  6 12:54 /usr/sbin/nologin*
```

Esta es una lista parcial de cuentas de sistema en Ubuntu mostrando `/usr/sbin/nologin` en lugar de un shell.
```
devuser@ubuntu2204-1-devesp 
~/linux-devesp
hist:202 -> grep nologin /etc/passwd
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
sshd:x:107:65534::/run/sshd:/usr/sbin/nologin
```
Esencialmente nologin indica que no puede entrar al sistema usando un nombre de usuario y contraseña. Por ejemplo, `sshd` es el daemon que maneja conecciones con el cliente SSH para entrar remotamente al sistema; no es un usuario y no necesita un shell. Lo mismo pasa con `lp` que se usa para manejar procesos de printear a un instrumento externo. La misma lógica aplica a toda cuenta de sistema que no requiere sesiones interactivas en una terminal.

## El SHELL Predeterminado

En Ubuntu y RedHat existe un ajuste en el archivo `/etc/default/useradd` que especifica el SHELL predeterminado para usuarios. Este será el shell que el usuario usará para entrar comandos e interactuar con el sistema.

{: .note }
Por regla general el BASH o SH shell se da por shell predeterminado en varios sabores de Linux.

En Ubuntu, el shell predeterminado es `/bin/sh`
```
-> grep SHELL /etc/default/useradd
# The SHELL variable specifies the default login shell on your
# Similar to DSHELL in adduser. However, we use "sh" here because
SHELL=/bin/sh
```

En Ubuntu, el shell predeterminado es `/bin/bash`
```
 -> grep SHELL /etc/default/useradd
SHELL=/bin/bash
```

El comando siguiente muestra cual es nuestro shell corriente.
```
-> echo $SHELL
/bin/bash
```

SHELL es una variable disponible en el entorno del usuario. El valor de la variable se puede ver con el comando `env`.
```
-> env | grep -i shell
SHELL=/bin/bash
```

Un usuario puede cambiar a cualquiera de los shells disponibles en el sisema. Si el shell deseado no esta presente en el sistema, puede ser instalado si existe la facilidad de hacerlo; generalmente esto no es necesario.

## Conclusion

Es recomendable aprender como usar el shell de manera proficiente para tomar ventaja de muchos usos versatiles. 

## Referencias 

### Glosario De Comandos

export
: comando usado para crear variables con NOMBRE y VALOR en el SHELL

env
: sirve para mostrar variables con NOMBRE y VALOR en el ambiente del usario.

### Referencias Utiles

DevEsp :: Linux
- https://docs.devesp.com/linux-en-espa%C3%B1ol/

Paginas Manuales
- [env](https://manpages.ubuntu.com/manpages/focal/en/man1/env.1.html)

Otras referencias
- [GNU Bash](https://www.gnu.org/software/bash//)
- [Referencia del Manual de Bash](https://www.gnu.org/software/bash/manual/bash.html)
