---
layout: default
title: Entorno De Usuarios
permalink: /entorno-de-usuarios/
parent: Usuarios
grand_parent: Linux
has_children: true
has_toc: false
nav_order: 1
---

# LINUX :: Usuarios :: Entorno De Usuarios

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
- Conoceremos donde se establecen las cuentas de usuarios.
- Veremos la estructura de los archivos para manejar cuentas locales
- Entederemos la diferencia entre Usuario Regular y Cuenta de Sistemas

**DEPENDENCIAS**

ninguna

**REQUERIMIENTOS**

Sistema de Linux Ubuntu. <br>
Acceso a la terminal de Linux.<br>
Alguos comandos requieren privilegios elevados.

**ADVERTENCIA**

ninguna.

## Working Environment

En esta leccion usamos el sistema operativo Ubuntu.

## Métodos De Crear Cuentas de Usuarios

Hay varios métodos de crear cuentas de usuarios. La manera de implementación establece un marco muy diferente en como crear y mantener cuentas de usuarios. Es decir, el uso una cuenta local es muy diferente de una cuenta de red; una cuenta de red requiere conección de red, mientras que una cuenta local no necesita estar en la read para usarse.

### LDAP / AD 

LDAP y AD se usan en ambientes de Red y son considerablemente mas complejas en su aplicación y administración. LDAP (Protocolo Ligero de Acceso a Directorios) y AD (Directorio activo) son servicios de directorio que se utilizan para administrar cuentas de usuario y permisos en un entorno de red. 

Veamos una comparación entre las cuentas de usuario de dominio LDAP y AD:

Autenticación
: LDAP o AD puede usarse para autenticar cuentas de usuario. Pero AD ofrece funciones adicionales como inicio de sesión único, autenticación multifactor e integración on sistemas basados en Windows.

Autorización
: Tanto LDAP como AD brindan servicios de autorización para controlar el acceso a los recursos según los roles y permisos del usuario. Sin embargo, AD ofrece un control más granular sobre los permisos y las políticas de grupo.

Administración
: AD proporciona una consola de administración centralizada para administrar cuentas de suarios, grupos y políticas en una infraestructura de red. LDAP es más ligero y flexible, pero puede requerir herramientas adicionales para la gestión de usuarios.

Integración
: AD está estrechamente integrado con el sistema operativo Windows y otros productos de Microsoft, lo que facilita la administración de cuentas de usuario y el control de acceso en un entorno de Windows. LDAP es más independiente de la plataforma y se puede utilizar con una amplia gama de sistemas y aplicaciones.

Escalabilidad
: AD está diseñado para escalar a entornos empresariales grandes y puede admitir miles de cuentas de usuarios, grupos y permisos. LDAP puede requerir configuración y optimización adicionales ara lograr escalabilidad en entornos grandes.

### Kerberos

Kerberos es un protocolo de autenticación de red para la administración y autenticación de usuarios. Faciliata el acceso a múltiples servicios o sistemas con un único conjunto de credenciales.

Los usuarios reciben un ticket Kerberos, que sirve como prueba de identidad al acceder a servicios o recursos en la red. Esto ayuda a evitar el acceso no autorizado y proteger la información confidencial.

Cabe notar que Kerberos puede integrarse con LDAP para información y autorización de usuarios. Esto permite un proceso de autenticación seguro y fluido en toda la red.

### NIS

NIS (Servicio de información de red) es un sistema que se usa en la autenticación y gestión centralizada de usuarios. La idea era que que sistemas en una red tenian acceso a las cuentas de usuario, contraseñas y membresías de grupos

Sin embargo, NIS se considera una tecnología obsoleta y tiene vulnerabilidades de seguridad, por lo que es recomendable tornar a herramientas más seguras como LDAP (Lightweight Directory Access Protocol) o AD (Active Directory) para la gestión y autenticación de usuarios en un entorno Linux.

### Cuentas Locales

Cuentas locales son creadas en el sistema donde se intenta operar. Generalmente esto sistemas son de uso personal en los que seguridad no es tan importante como sistemas de negocios.

### Archivos Para Manejo De Cuentas Locals

Linux usa tres archivos para manejar cuentas locales
- /etc/passwd
- /etc/shadow
- /etc/group

{: .highlight }
Todos los archivos son legibles en texto claro

### Estructura de Una Cuenta Local

Hemos discutido que hay varios tipos de usuarios
- Usuario Regular      
- Root (Super Usuario) 
- Cuenta de Sistema   

## Conclusion

El manejo de cuentas en Linux va desde lo mas simple a lo mas complejo. La elección de método que hagas depende del entorno.

Para un sistema de uso personal, el uso de Cuentas Locales es suficiente. 

Para un sistema en red en un ambiente de negocios de una empresa, es mejor usar herramientas seguras tales como LDAP o AD.

## Referencias 

### Glosario De Comandos y Terminos

AD
: AD es "Active Directory" o "Directorio Activo"

LDAP
: LDAP es "Lightweight Directory Access Protocol" o "Protocolo ligero de acceso a directorios"

[^1]: IPC "Comunicación entre procesos" o "Inter Process Communication" se refiere a un conjunto de métodos y protocolos utilizados para la comunicación y el intercambio de datos entre dos o más procesos que se ejecutan simultáneamente en un sistema. Permite que diferentes procesos se comuniquen entre sí y compartan datos, permitiéndoles trabajar juntos y coordinar sus acciones. Los métodos comunes de IPC incluyen canalizaciones, sockets, colas de mensajes, memoria compartida y semáforos.

### Referencias Utiles

DevEsp :: Linux
- https://docs.devesp.com/linux-en-espa%C3%B1ol/

Herramienta [LDAP](https://ldap.com/)

Herramienta [Microsoft AD](https://learn.microsoft.com/en-us/windows-server/security/windows-authentication/windows-authentication-overview)

Herramienta [Kerberos](https://kerberos.org/)
