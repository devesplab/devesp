---
layout: default
title: Openssl Crear Certificado PEM
permalink: /openssl-create-simple-cert/
parent: Articulos
has_children: false
has_toc: false
nav_order: 1
---

## Creación de Certificados PEM con Openssl


{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

**DESCRIPCION**

Guía sencilla para generar un certificado PEM SSL sin cifrar. 

Este es un método sencillo en el que no tienes que introducir una frase de acceso al generar el certificado. Por ello, no tienes que introducir una frase de contraseña al empezar Apache/Nginx con un certificado creado como se muestra aquí. No hace falta decir que esto es solo para aprender y no debería hacerse en un entorno de producción. 

El resultado final es un archivo `server.key` y `server.crt` que puede usarse con un servidores web como Apache o Nginx.

**DEPENDENCIAS**

ninguna

**REQUERIMIENTOS**

Sistema de linux Ubuntu. <br>
Algunos comandos requieren privilegios elevados.<br>
Instalar el paquete de OpenSSL.

**ADVERTENCIA**

Aunque no está estrictamente prohibido, nunca deberías usar un certificado sin contraseña en un entorno de producción. Una clave privada no cifrada (sin frase de paso) en el disco puede ser utilizada por cualquier proceso o atacante que obtenga acceso de lectura al archivo; esto aumenta el riesgo de compromiso.

## Ambiente de trabajo

En esta leccion usamos el sistema operativo Ubuntu.

```bash
-> lsb_release -a
Distributor ID:	Ubuntu
Description:	Ubuntu 24.04.4 LTS
Release:	24.04
Codename:	noble
```

Version de OpenSSL.

```bash
-> openssl version
OpenSSL 3.0.13 30 Jan 2024 (Library: OpenSSL 3.0.13 30 Jan 2024)
```

## Guía Rápida

Comandos para crear un certificado SSL PEM sencillo sin contraseña:

```bash
# instalar openssl
sudo apt update
sudo apt install openssl ca-certificates -y

# crear certificado
openssl genrsa -out server.pem 2048
openssl req -new -x509 -key server.pem -out server.crt -days 9999
cp server.pem server.key
openssl req -new -x509 -key server.key -out server.crt -days 9999
```

## Instalar OpenSSL

Antes de continuar, instala el paquete de openssl:

```bash
sudo apt update
sudo apt install openssl ca-certificates -y
```

El `paquete ca-certificates` instala un conjunto curado de certificados raíz de CA de confianza (en formato PEM) utilizados por bibliotecas y aplicaciones del sistema (curl, OpenSSL, navegadores, apt, etc.) para verificar los servidores TLS/SSL y los datos firmados. 

En Ubuntu hace lo siguiente: 

- Proporciona el paquete `/etc/ssl/certs/` y archivos de certificación individuales. 
- Proporciona el `/etc/ca-certificates.conf` que lista las CA gestionadas. 
- Proporciona la utilidad `update-ca-certificates` para añadir/eliminar/actualizar las CA de confianza y regenerar el paquete.

## Crear Certificado

Crea un directorio donde hacer tu trabajo. Cambio en el directorio

```bash
Sat 2026May16 22:37:03 UTC
devuser@devesp
~
hist:288 -> mkdir ssl-workdir && cd ssl-workdir
```

Genera una nueva clave privada RSA de `2048 bits` y escríbele en el archivo `server.key`. El archivo clave estará en formato PEM (sin cifrar).

```bash
-> openssl genrsa -out server.key 2048
```

A continuación, genera un certificado `X.509` autofirmado a partir de la clave privada y escríbelo en `server.crt`. Pulsa "enter" en todas las preguntas dejando en blanco. El resultado es un archivo en texto plano. 

```bash
-> openssl req -new -x509 -key server.key -out server.crt -days 9999
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:
State or Province Name (full name) [Some-State]:
Locality Name (eg, city) []:
Organization Name (eg, company) [Internet Widgits Pty Ltd]:
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:
Email Address []:
```

Opciones utilizadas:

- `OpenSSL Req`: solicitud de certificados y utilidad generadora de certificados. 
- `-new`: crear una nueva solicitud (aquí usada para crear un certificado).
- `-x509`: emite un certificado autofirmado en lugar de un CSR.
- `-key server.key`: usar la clave privada en server.key.
- `-out server.crt`: escribe el certificado en server.crt (formato PEM).
- `-days 9999`: haz que el certificado sea válido durante 9999 días.

Por defecto, ese comando escribe un certificado X.509 codificado en PEM `(ASCII with "-----BEGIN CERTIFICATE-----"/"-----END CERTIFICATE-----")`.

Esto te pedirá campos temáticos (país, CN, etc.). Si `server.key` está cifrado, se te pedirá su contraseña. Para incluir extensiones (SANs) o producir un CSR en su lugar, se requieren diferentes banderas/configuraciones.

El proceso anterior produjo estos archivos:

- `server.crt`: el certificado PEM 
- `server.key`: la clave de certificado

```bash
-> ls -l
total 8
-rw-rw-r-- 1 devuser devuser 1249 May 16 22:42 server.crt
-rw------- 1 devuser devuser 1704 May 16 22:42 server.key
```

Utiliza el comando estándar `file` de Linux para comprobar el tipo de contenido

```bash
-> file server.key
server.key: OpenSSH private key (no password)

-> file server.crt
server.crt: PEM certificate
```

## Conclusion

Este documento proporciona una guía sencilla para crear un certificado PEM sin cifrar que puede usarse con fines de aprendizaje.

## Referencias

### Glosario De Comandos

Los comandos siguientes son usados frecuentemente en sesiones de Linux.

file:
: Comando estándar de Linux para comprobar el tipo de contenido de un archivo

openssl
: utilidad estándar para operar con certificados SSL

### Referencias Útiles

Recursos oficiales de OpenSSL:

- [Openssl Documentacion Oficial](https://www.openssl.org/)
- [Documentacion General OpenSSL](https://docs.openssl.org/master/man7/ossl-guide-introduction/)
- [OpenSSL Repositorio De Github](https://github.com/openssl/openssl.git)

