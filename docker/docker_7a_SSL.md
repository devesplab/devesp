---
layout: default
title: Docker SSL 
permalink: /docker_ssl/
parent: Docker
has_children: false
has_toc: false
nav_order: 6
---

# Usando SSL en Docker
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

## Crear un Contenedor Docker Usando un Certificado Auto-firmado

En este articulo mostramos como usar Certificados SSL con un container de Docker. Al final del ejercicio podremos tener acceso al servidor web en un navegador para ver la pagina en HTTPS. Ve la referencia al final de esta pagina y acceder el código usado en este ejemplo.

Para empezar, crea el directorio donde vamos a trabajar y genera un certificado auto-firmado usando el comando `openssl`:

```
-> mkdir -p ./DockerBuilds/myNginxSSL
-> cd ./DockerBuilds/myNginxSSL
-> openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout server.key -out server.crt -subj "/C=US/ST=State/L=City/O=Organization/CN=localhost"
```

Crea un Dockerfile que importará correctamente estos certificados:

  1. Usa Ubuntu 22.04 como imagen base
  2. Instala los paquetes SSL necesarios
  3. Crea los directorios requeridos
  4. Copia los certificados existentes (`server.crt` y `server.key`) a sus ubicaciones correctas:
    ○ El certificado va en `/etc/ssl/certs/`
    ○ La clave privada va en `/etc/ssl/private/`
  5. Establece los permisos apropiados:<br>
    ○ `644` para el certificado (legible por todos)<br>
    ○ `600` para la clave privada (legible solo por root)
  6. Verifica que los certificados estén en su lugar
  7. Actualiza el almacén de certificados CA

Crea el archivo con nombre `Dockerfile`.

```
# Use Ubuntu as base image
FROM ubuntu:22.04
LABEL maintainer="devuser@example.com"

# Install SSL-related packages and nginx
RUN apt-get update && \
    apt-get install -y \
    ca-certificates \
    nginx \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Create directory for certificates
RUN mkdir -p /etc/ssl/private

# Copy SSL certificate and key
COPY server.crt /etc/ssl/certs/
COPY server.key /etc/ssl/private/

# Set proper permissions
RUN chmod 644 /etc/ssl/certs/server.crt && \
    chmod 600 /etc/ssl/private/server.key

# Copy nginx configuration and website content
COPY my-nginx-ssl.conf /etc/nginx/conf.d/default.conf
COPY index.html /usr/share/nginx/html/

# Update CA certificates
RUN update-ca-certificates

# Expose HTTPS port
EXPOSE 443

# Start nginx
CMD ["nginx", "-g", "daemon off;"]
```

En el `Dockerfile`, los certificados estarán disponibles en las ubicaciones estándar cuando el contenedor se inicie.<br>
Agregamos nginx y servimos una página HTTPS segura (`index.html`) usando el certificado SSL (`server.crt`). <br>
Exponemos solo el puerto **443** para acceso web seguro.<br> 
Crea el archivo de configuración de nginx en un archivo llamado `my-nginx-ssl.conf`.

```
server {
    listen 443 ssl;
    server_name localhost;
    ssl_certificate /etc/ssl/certs/server.crt;
    ssl_certificate_key /etc/ssl/private/server.key;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;
    location / {
        root /usr/share/nginx/html;
        index index.html;
    }
}
```

Crea una página HTML simple creando un archivo llamado `index.html`.

```html
<!DOCTYPE html>
<html>
<head>
    <title>Greetings from DevESP</title>
</head>
<body>
    <h1>Hello from DevESP Secure NGINX!</h1>
    <p>This is a web server using a custom self-signed SSL certificate to serve a page over HTTPS.</p>
</body>
</html>
```

La estructura de la carpeta conteniedo todos los archivos se ve así:
```sh
-> ls -l myNginxSSL/
-rw-r--r--  1 devuser  devuser  822 Jun  8 12:37 Dockerfile
-rw-r--r--  1 devuser  devuser  250 Jun  8 12:29 index.html
-rw-r--r--  1 devuser  devuser  314 Jun  1 15:12 my-nginx-ssl.conf
-rw-r--r--  1 devuser  devuser 1294 Jun  1 15:04 server.crt
-rw-------  1 devuser  devuser 1704 Jun  1 15:04 server.key
```

Construye el contenedor de la siguiente manera.

Ve al directorio superior que contiene el Dockerfile.
```sh
-> cd ./DockerBuilds/myNginxSSL
```

Construye la imagen docker.
```sh
-> docker build -t my-nginx-ssl . 
```

Ejecuta el contenedor mapeando el puerto 443 desde el host docker al contenedor.
```sh
-> docker run -dit -p 443:443 my-nginx-ssl
```

Verifica el contenedor en ejecución
```sh
-> docker ps
CONTAINER ID   IMAGE             COMMAND                  CREATED         STATUS         PORTS                        NAMES
acb12f63ce40   my-nginx-ssl      "nginx -g 'daemon of…"   3 minutes ago   Up 3 minutes   0.0.0.0:443->443/tcp         serene_lederberg
```

Abre un navegador y ve a la página `https://localhost/`

![](../../assets/images/docker_ssl_1.png){:width="550px"}

{: .warning }
Inicialmente, verás una advertencia de seguridad porque estamos usando un certificado auto-firmado. Haz clic para aceptar el riesgo y ver la página.

Una vez que la página es visible, haz clic en el icono del candado junto a la URL donde podrás ver los detalles del certificado.

![](../../assets/images/docker_ssl_2.png){:width="550px"}

Hemos implementado exitosamente un servidor web Nginx usando Certificados SSL sirviendo tráfico a través de HTTPS.

## Referencia

Navega a github.com [myNginxSSL](https://github.com/devesplab/docker-devesp/tree/main/myNginxSSL) para ver el código usado en este documento.

[Return to main page]({{site.baseurl}}/).