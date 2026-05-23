---
layout: default
title: Usuarios
permalink: /usuarios/
parent: Linux
has_children: true
has_toc: false
nav_order: 6
---

# Conceptos De Usuarios

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

## Ques es un Usuario en Linux?

Entenderemos que es un usuario, que hace en el sistema y cuál es la esfera de acción.

Linux es un sistema compartido por usuarios multiples y es necesario tener un método de catalogar, auditar y administrar el acceso a los recursos del ambiente operativo. Para alcanzar ese objetivo Linux usa **Cuentas de Usuarios**. Por lo tanto es importante entender este tema desde el ángulo de Administración De Sistemas.

Los usuarios de Linux tienen ciertas particularidades: los usuarios de Linux suelen ser personas conocedoras de la tecnología y con buenos conocimientos de tecnología informática y programación. Por regla general, los usuarios de Linux suelen preferir el software de código abierto y valoran la flexibilidad y las opciones de personalización disponibles. Tambien tienden a ser más conscientes de la seguridad y más centrados en la privacidad en comparación con los usuarios de otros sistemas operativos. La categoría de usuarios varía desde los usuarios domésticos ocasionales hasta desarrolladores profesionales y administradores de sistemas, cada uno con sus propias necesidades y preferencias específicas.  Los usuarios de Linux son parte de una comunidad global que colabora en el desarrollo y mejora del ecosistema Linux, compartiendo conocimientos y recursos entre sí.

En lo referente al modo de interacción con el sistema, los usuarios de Linux suelen tener una fuerte preferencia por la interfaz de línea de comandos y se sienten cómodos realizando tareas utilizando la terminal.

## Conceptos de Usuarios y Grupos

Linux hace uso de nombres de usuarios para identificar a un operador en forma distintiva de otros operadores. El nombre del usuario se usa para entrar al sistema y acceder recursos tales como archivos y carpetas.

Linux usa grupos para juntar usuarios que tiene algo en común, por ejemplo, compartir una carpeta, un proceso, una tarea o un proyecto. Un grupo en si no es un usuario, no se puede entrar a un sistema usando el nombre del grupo.

{: .note }
Es posible que un usuario pertenesca a grupos multiples!

Esta demas decir que debemos asegurarnos que no debería haber duplicación de nombres de usuario o groups.

## Tipos de Usuarios

Dependiendo del radio de acción los usuarios en Linux tienen diferente clasificación.

| Tipo De Usuario      | Esfera De Acción                 |
| ---------------------| -------------------------------- |
| Usuario Regular      | Reducido al directorio Hogar     |
| Super Usuario        | Acceso completo al sistema       | 
| Cuenta de Sistema    | Acceso a un proceo o aplicación  | 

Un ejempo the usuario regular es `devuser` que puede ser una cuenta local creada for el Administrador de Sistemas.<br>

Un ejemplo de **Super Usuario** es `root` que esta presente en cada sistema de Linux y tiene 100% acceso al sistem entero.<br>

Un ejemplo de **Cuenta de Sistema** es `sshd` que se usa para correr la aplicacíon de SSH usada para acceso remoto seguro en la red. En cierto modo, una Cuenta de Sistema tiene acceso privilegiado porque toca recursos reservados del sistema. Este tipo de cuenta tambien se conoce como **Cuenta de Servicio**, la cual no esta asignada a una persona en particular sino que sirve para correr una aplicación o proceso del sistema usada por muchas personas o servicios varios.

Veamos a continuación una ilustración breve del efecto de usar diferente tipos de usuarios.

Entremos como el usuario `root` a un sistema y verifiquemos. Luego tratemos de ver el archivo `/etc/shadow` que es muy restrictivo.
```
-> sudo su -

-> id
uid=0(root) gid=0(root) groups=0(root)

-> tail /etc/shadow
systemd-timesync:*:19553:0:99999:7:::
tcpdump:*:19553:0:99999:7:::
postfix:*:19553:0:99999:7:::
sshd:*:19553:0:99999:7:::
```
No hay problema con el usuario `root` puesto que tiene aceso a 100% del sistema.

Cambiemos a un usuario regular y tratemos de ver el archivo `/etc/shadow`.
```
-> su - devuser

-> id
uid=2045(devuser) gid=2046(devuser) groups=2046(devuser),27(sudo)

-> tail /etc/shadow
tail: cannot open '/etc/shadow' for reading: Permission denied
```
El usuario no tiene el permiso necesario para ver ese archivo.

Ahora cambiemos a el usuario `sshd` que es la Cuenta de Sistema que maneja SSH. 
```
-> su - sshd
This account is currently not available.
```
El sistema indica que no podemos entrar con ese tipo de cuenta.

Frecuentemente es necesario asignar acceso elevado a un usuario regular. Esto se alcanza al otorgar un rol de super usuario usando SUDO. 

Por regla general, y para limitar el impacto, se da el derecho de menos privilegio en cuanto sea posible. Esto significa que un usuario no debe tener acceso a mas de lo que necesitar para funcionar en su entorno.

## Acerca de Nombres de Usuarios

Los nombres de usuarios tienen ciertas reglas que deberian ser observadas 
- técnicamente el nombre de usuario puede ser una sola letra, y no mas de 32. Sin embargo, un nombre de usuario de una sola letra es necesariamente una mala práctica
- no deben usarse símbolos especiales tales como `&%$£π#()~` y similares
- un nombre de usuario no debe empezar con un número

## Conclusión

Es importante tener un entendimiento básico de prácticas generales que concierne a nombres de usuarios en Linux. Una vez que el patrón es establecido, es fácil tener prácticas para administrar los usuarios. Esto es particularmente crucial en organizaciones con dozenas, cientos y hasta miles de usuarios que comparten procesos extremadamente complejos.

## Referencias 

### Glosario De Términos y Comandos

Los términos siguientes son usados frecuentemente en sesiones de Linux.

usuario
: es una entidad que gana acceso a un ambiente de linux usando una cuenta de usuario

super usuario
: es un usuario con acceso elevado en un ambiente de linux

cuenta de sistema ( o cuenta de servicio)
: son cuentas de usuarios con acceso especial que corren aplicaciones o procesos con ciertos privilegios no disponibles a usuarios regulars

### Referencias Utiles

Páginas Manuales

[Paginas Manuales de Ubuntu](https://manpages.ubuntu.com)

[Return to main page]({{site.baseurl}}/).
