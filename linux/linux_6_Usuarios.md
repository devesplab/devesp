---
layout: default
title: Usuarios
permalink: /usuarios/
parent: Linux
has_children: true
has_toc: false
nav_order: 6
---

# LINUX :: Usuarios :: Conceptos De Usuarios

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

Entenderemos que es un usuario, que hace en el sistema y cuál es la esfera de acción.

Linux es un sistema compartido por usuarios multiples y es necesario tener un método de catalogar, auditar y administrar el acceso a los recursos del ambiente operativo. Para alcanzar ese objetivo Linux usa **Cuentas de Usuarios**. Por lo tanto es importante entender este tema desde el ángulo de Administración De Sistemas.

**DEPENDENCIAS**

ninguna

**REQUERIMIENTOS**

Sistema de linux Ubuntu. <br>
Alguos comandos requieren privilegios elevados.

**ADVERTENCIA**

ninguna.

## Working Environment

En esta leccion usamos el sistema operativo Ubuntu.

## Conceptos de Usuarios y Grupos

Un Usuario en Linux tiene un nombre de usuario que se usa para identificar a un operador en forma distintiva de otros operadores. El nombre del usuario se usa para entrar al sistema y acceder recursos tales como archivos y carpetas.

Linux usa grupos para juntar usuarios que tiene algo en común, por ejemplo, compartir una carpeta o un proceso. Un grupo en si no es un usuario, no se puede entrar a un sistema usando el nombre del grupo.

{: .note }
Es posible que un usuario pertenesca a grupos multiples!

No viene al caso decir que debemos asegurarnos que no debería haber duplicación de nombres de usuario o groups.

## Tipos de Usuarios

Dependiendo del radio de acción los usuarios en Linux tienen diferente clasificación.

| Tipo De Usuario      | Esfera De Acción                 |
| ---------------------| -------------------------------- |
| Usuario Regular      | Reducido al directorio Hogar     |
| Super Usuario        | Acceso completo al sistema       | 
| Cuenta de Sistema    | Acceso a un proceo o aplicación  | 

Un ejempo the usuario regular is `devuser` que puede ser una cuenta local creada for el Administrador de Sistemas.<br>
Un ejemplo de super usuario es `root` que esta presente en cada sistema de Linux y tiene 100% acceso al sistem entero.<br>
Un ejemplo de Cuenta de Sistema es `postgresql` que se usa para correr la aplicacíon de PostgreSQL usada para base de datos. En cierto modo tiene acceso privilegiado porque toca recursos reservados del sistema. Este tipo de cuenta tambien se conoce como **Cuenta de Servicio**, la cual no esta asignada a una persona en particular sino que sirve para correr una aplicación o proceso del sistema usada por muchas personas o servicios varios.

Frecuentemente es necesario asignar acceso elevado a un usuario regular. Esto se alcanza al otorgar
un rol de super usuario usando SUDO. 

Por regla general, y para limitar el impacto, se da el derecho de menos privilegio en cuanto sea posible.

## Acerca de Nombres de Usuarios

Los nombres de usuarios tienen ciertas reglas que deberian ser observadas 
- técnicamente el nombre de usuario puede ser una sola letra, y no mas de 32. Sin embargo, un nombre de usuario de una sola letra es una mala práctica
- no deben usarse símbolos especiales tales como `&%$£π#()~` y similares
- no debe empezar con un número

## Conclusión

Es importante tener un entendimiento básico de prácticas generales que concierne a nombres de usuarios en Linux. Una vez que el patrón es establecido, es fácil tener prácticas para administrar los usuarios. Esto es particularmente crucial en organizaciones con dozenas, cientos y hasta miles de usuarios que comparten procesos extremadamente complejos.

## Referencias 

### Glosario De Comandos

Los términos siguientes son usados frecuentemente en sesiones de Linux.

usuario
: es una entidad que gana acceso a un ambiente de linux usando una cuenta de usuario

super usuario
: es un usuario con acceso elevado en un ambiente de linux

cuenta de sistema ( o cuenta de servicio)
: son cuentas de usuarios con acceso especial que corren aplicaciones o procesos con ciertos privilegios no disponibles a usuarios regulars

### Referencias Utiles

DevEsp :: Linux
- https://docs.devesp.com/linux-en-espa%C3%B1ol/

[Paginas Manuales de Ubuntu](https://manpages.ubuntu.com)
