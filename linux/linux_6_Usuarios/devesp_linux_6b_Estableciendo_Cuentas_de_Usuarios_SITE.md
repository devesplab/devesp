---
layout: default
title: Estableciendo Cuentas De Usuarios
permalink: /entorno-de-usuarios/
parent: Usuarios
grand_parent: Linux
has_children: false
has_toc: false
nav_order: 1
---

# Estableciendo Cuentas De Usuarios en Linux

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

## La Primera Cuenta de Usuario

Cuando un sistema de Linux es instanciado por primera vez, solo tiene la cuenta del Super Usuario "root". La persona u orgnaización que ha instanciado el sistema determina como crear y administrar cuentas adicionales de usuarios. La manera de crear cuentas varias desde lo más simple creando cuentas locales hasta lo más complejo usando herramientas de terceros en un entorno empresarial de producción.

En esta leccion discutimos brevemente el entorno donde se establecen las cuentas de usuarios.

## Métodos De Crear Cuentas de Usuarios

Hay varios métodos de crear cuentas de usuarios. La manera de implementación establece un marco muy diferente en como crear y mantener dichas cuentas. Es decir, el uso una cuenta local es muy diferente de una cuenta de red; una cuenta de red requiere conección de red, mientras que una cuenta local no necesita estar en la read para usarse.

### Cuentas Locales

En corto, una cuenta local es aquella que usa comandos nativos que se encuentran an nivel del sistema operativo y que no necesita de herramientas externas en la red para funcionar.

Cuentas locales son creadas en el sistema donde se intenta operar. Generalmente esto sistemas son de uso personal en los que seguridad no es tan importante como sistemas de negocios.

Linux usa tres archivos para manejar cuentas locales
- /etc/passwd
- /etc/shadow
- /etc/group

{: .highlight }
Todos los archivos son legibles en texto claro

Para mas información ver la página [Manejando Usuarios](./devesp_linux_6c_Manejando_Usuarios_SITE.md) en donde discutimos los detalles pertinentes al manejo de cuentas locales.

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

El tema de Kerberos es muy complejo y no apto para discusion en este espacio. Para mas información ver el enlace a la documentatión en linea en la sección de [referencias](#referencias).

### NIS

NIS (Servicio de información de red) es un sistema que se usa en la autenticación y gestión centralizada de usuarios. La idea era que que sistemas en una red tenian acceso a las cuentas de usuario, contraseñas y membresías de grupos

Sin embargo, NIS se considera una tecnología obsoleta y tiene vulnerabilidades de seguridad, por lo que es recomendable tornar a herramientas más seguras como LDAP (Lightweight Directory Access Protocol) o AD (Active Directory) para la gestión y autenticación de usuarios en un entorno Linux.

El tema de NIS es muy complejo y no apto para discusion en este espacio. Para mas información ver el enlace a la documentatión en linea en la sección de [referencias](#referencias).

## Conclusion

El manejo de cuentas en Linux va desde lo mas simple a lo mas complejo. La elección de método que hagas depende del entorno.

Para un sistema de uso personal, el uso de Cuentas Locales es suficiente. 

Para un sistema en red en un ambiente de negocios de una empresa, es mejor usar herramientas seguras tales como LDAP o AD.

## [](referencias)Referencias

### Glosario De Comandos y Terminos

AD
: AD es "Active Directory" o "Directorio Activo"

LDAP
: LDAP es "Lightweight Directory Access Protocol" o "Protocolo ligero de acceso a directorios"

Kerberos
: Kerberos es un protocolo de autenticación de red que se utiliza para proporcionar autenticación segura para usuarios y servicios en un entorno de red.

[^1]: IPC "Comunicación entre procesos" o "Inter Process Communication" se refiere a un conjunto de métodos y protocolos utilizados para la comunicación y el intercambio de datos entre dos o más procesos que se ejecutan simultáneamente en un sistema. Permite que diferentes procesos se comuniquen entre sí y compartan datos, permitiéndoles trabajar juntos y coordinar sus acciones. Los métodos comunes de IPC incluyen canalizaciones, sockets, colas de mensajes, memoria compartida y semáforos.

### Referencias Utiles

Herramientas de Terceros
- [LDAP](https://ldap.com/)
- [Microsoft AD](https://learn.microsoft.com/en-us/windows-server/security/windows-authentication/windows-authentication-overview)
- [Kerberos](https://kerberos.org/)

[Return to main page]({{site.baseurl}}/).
