---
layout: default
title: Docker Compose
permalink: /docker_compose_basics/
parent: Docker
nav_order: 20
---
---

# Conceptos Básicos de Docker Compose
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

## Qué es Docker Compose?

La documentación oficial en linea de [Docker Complse](https://docs.docker.com/compose/) dice lo siguiete:

> Docker Compose es una herramienta para definir y ejecutar aplicaciones multicontenedor. Es la clave para lograr una experiencia de desarrollo e implementación optimizada y eficiente.<br><br>
> Compose simplifica el control de toda la pila de aplicaciones, lo que facilita la administración de servicios, redes y volúmenes en un único archivo de configuración YAML comprensible. Luego, con un solo comando, puede crear e iniciar todos los servicios desde su archivo de configuración._

Asi que Docker Compose es una herramienta bastante útil que puede usarse para instanciar un ambiente complete de desarrollo o aprendizaje.

Lo primero que se hace es crear un archive llamado `compose.yml`.
Luego usamos la utilidad de docker compose para empezar y usar el ambiente.
Por ultimo, podemos destruir y recrear el ambiente como sea necesario.

## Crear el archivo compose.yaml

A continuación creamos una carpeta donde podamos trabajar. Cambiamos a la carpeta y creamos el archivo `compose.yml`.
```
-> mkdir ~/docker_compose/compose_test
-> cd ~/docker_compose/compose_test
-> vi compose.yml
```

{: .important }
En estos ejercicios usamos la imagen de docker `dockio/u2204-devesp:1.0` especificamente creada para el sitio **DevEsp**.


Este es un contenido simple de un archivo de docker compose que contiene código para dos servicios. Cada servicio equivale a un contenedor.
```
services:

# U22 - FIRST
  ubuntu2204-1-devesp:
    image: dockio/u2204-devesp:1.0
    hostname: ubuntuOne
    container_name: ubuntu2204-1-ubuntuOne
    privileged: true 
    entrypoint: ["/usr/sbin/init"]    
    restart: on-failure
    volumes:
      - "./:/hostdata"    
    command: ["sleep", "infinity"] 

# U22 - SECOND
  ubuntu2204-2-devesp:
    image: dockio/u2204-devesp:1.0  
    hostname: ubuntuTwo
    entrypoint: ["/usr/sbin/init"]    
    container_name: ubuntu2204-2-ubuntuTwo
    privileged: true 
    restart: on-failure
    volumes:
      - "./:/hostdata"    
    command: ["sleep", "infinity"]
```

La estructura del archivo es simple:
- **services**: es la linea que encabeza el archivo
- **ubuntu2204-1-devesp**: es el nombre del servicio
- **image**: la imagen de docker que usamos para instanciar el contenedor
- **hostname**: el nombre de host con que deseamos personalizar el contenedor y diferenciarlo de otros
- **container_name**: el nombre del contenedor con que deseamos personalizar el contenedor y diferenciarlo de otros
- **entrypoint**: el punto de entrada indicando que deseamos correr el proceso de `init`
- **volumes**: son carpetas del host de docker para compartir con el contenedor
- **command**: truco usado para mantener el contenedor corriendo infinitamente

A seguir, creamos el ambiente.<br>

{: .note }
Usamos la bandera `-d` para crear el ambiento en el fondo y no interrumpir accesso a la terminal.

```
-> docker compose up -d
[+] Running 3/3
 ✔ Network compose_test_default      Created              0.0s
 ✔ Container ubuntu2204-2-ubuntuTwo  Started              0.0s
 ✔ Container ubuntu2204-1-ubuntuOne  Started              0.0s
```

## Estructura del Ambiente Activo

EL argumento `config` nos permite ver los detalles que docker compose uso para crear el ambiente.<br>
Esto es muy util para corroborar los parametros de configuracion en caso de problemas.

```
-> docker compose config
name: compose_test
services:
  ubuntu2204-1-devesp:
    command:
    - sleep
    - infinity
    container_name: ubuntu2204-1-ubuntuOne
    entrypoint:
    - /usr/sbin/init
    hostname: ubuntuOne
    image: dockio/u2204-devesp:1.0
    networks:
      default: null
    privileged: true
    restart: on-failure
    volumes:
    - type: bind
      source: /Users/devuser/practice_docker/docker_compose/compose_test/compose.yml
      target: /hostdata
      bind:
        create_host_path: true
  ubuntu2204-2-devesp:
    command:
    - sleep
    - infinity
    container_name: ubuntu2204-2-ubuntuTwo
    entrypoint:
    - /usr/sbin/init
    hostname: ubuntuTwo
    image: dockio/u2204-devesp:1.0
    networks:
      default: null
    privileged: true
    restart: on-failure
    volumes:
    - type: bind
      source: /Users/devuser/practice_docker/docker_compose/compose_test/compose.yml
      target: /hostdata
      bind:
        create_host_path: true
networks:
  default:
    name: compose_test_default
```

## Listar Contenedoros

Ahora podenos listar contenedores.

```
-> docker compose ls
NAME                                                     STATUS              CONFIG FILES
compose_test                                             running(2)          /Users/devuser/practice_docker/docker_compose/compose_test/compose.yml
```

Esto muestra la informacion de los contenedores que estan corriendo.
```
->  docker compose ps
NAME                     IMAGE                     COMMAND                           SERVICE               CREATED         STATUS         PORTS
ubuntu2204-1-ubuntuOne   dockio/u2204-devesp:1.0   "/usr/sbin/init sleep infinity"   ubuntu2204-1-devesp   2 minutes ago   Up 2 minutes
ubuntu2204-2-ubuntuTwo   dockio/u2204-devesp:1.0   "/usr/sbin/init sleep infinity"   ubuntu2204-2-devesp   2 minutes ago   Up 2 minutes
```

Las columnas se definen así:

NAME
: el nombre dado en `container_name` en el archive `compose.yml`

IMAGE
: la imagen de docker usada para instanciar el contenedor

COMMAND
: el comando usado como punto de entrada al contenedor

SERVICE
: el nombre del servicio que identifica al contendor especificado en el archive `compose.yml`

CREATED
: tiempo que ha pasado desde que el contenedor fue instanciado

STATUS
: la condición actual del contenedor

PORTS
: números de puertos si han sido definidos en el archive `compose.yml`

Podemos obtener el nombre del servicio.
```
-> docker-compose config --services
ubuntu2204-1-devesp
ubuntu2204-2-devesp
```

## Entrar a un contenedor

Para entrar a un contenedor y tener accesso a la terminal, podemos usar el comando a seguir en el host de Docker.<br>
Para esto usamos el nombre del contenedor bajo la columna NAME de la salida de `docker compose ps`.
```
-> docker exec -it ubuntu2204-1-ubuntuOne bash
```
Esa acción nos dara accesso a la terminal del contenedor.

Una vez dentro del contenedor podemos verificar usando el comando `hostname`.
```
root@ubuntuOne $ hostname
ubuntuOne
```
Desde ese momento podemos interactuar con el contenedor de acuerdo al propósito que tenemos en mente.


## Parar y Restablecer un Contenedor

Podemos parar un contenedor específico usando el nombre del servicio.
```
 -> docker compose stop ubuntu2204-2-devesp
[+] Stopping 1/1
 ✔ Container ubuntu2204-2-ubuntuTwo  Stopped
```

Tambien podemos re-empezar un contendor que hemos parado.
```
-> docker compose start ubuntu2204-2-devesp
[+] Running 1/1
 ✔ Container ubuntu2204-2-ubuntuTwo  Started
```

## Reiniciar un Contenedor dn Docker Compose

Tuve un escenario con la siguiente situación:
    • Un entorno de docker compose estaba completamente levantado y en ejecución
    • Había instalado node_exporter en tres máquinas virtuales Ubuntu
    • Quería que la instancia de P8s recolectara métricas de esas máquinas virtuales Ubuntu.

Para lograr esto, hice lo siguiente:
    • En el host Docker, mientras estaba en VSCode, edité el archivo prometheus.yml (no inicié sesión en el contenedor de P8s)
    • Reinicié el contenedor de P8s
    • Verifiqué en la WebUI de P8s que podía ver el job_name y las métricas de los targets

Reinicié el contenedor
```
-> docker compose restart prometheus-cicd
[+] Reiniciando 1/1
 ✔ Contenedor prometheus-cicd  Iniciado
```

## Destruir y Restablecer un contenedor

Tomando la list de servicios dados for el comando `docker-compose config --services`
Aplicamos la sintaxis a seguir"
```
-> docker compose down <service> 

-> docker compose up <service> &
```

{: .highlight }
Usamos el signo comercial `&` para indicar que el contenedor corra en el fondo y regresar a la linea de comandos.<br>

Por ejemplo, aqui destruimos y recobramos el contenedor `ubuntu2204-2-devesp`:
```
-> docker compose down ubuntu2204-2-devesp
[+] Running 2/1
 ✔ Container ubuntu2204-2-ubuntuTwo  Removed                         10.2s
 ! Network compose_test_default      Resource is still in use

-> docker compose up ubuntu2204-2-devesp &
[+] Running 1/0
 ✔ Container ubuntu2204-2-ubuntuTwo  Created                          0.0s
Attaching to ubuntu2204-2-ubuntuTwo
```

## Destruir el Ambiente de Docker Compose

Podemos destruir completamente el ambiente de docker compose usando el argumento `down`.

```
-> docker compose down -v
[+] Running 2/2
[+] Running 3/3untu2204-1-ubuntuOne  Stopped                          10.2s
 ✔ Container ubuntu2204-1-ubuntuOne  Removed                          10.2s
 ✔ Container ubuntu2204-2-ubuntuTwo  Removed                          10.2s
 ✔ Network compose_test_default      Removed
```

{: .warning }
Al destruir el ambiente se pierden todas las persolizaciones y datos creadas dentro del contenedor.

Para evitar perder data que hemos generado se recomienda agregar un volumen[^1] a la configuración. Por esta razón agregamos esto al archivo usado aqui. 
```
    volumes:
      - "./:/hostdata
```

Ahora corroboremos que no hay mas contenedors corriendo.
```
-> docker compose ps
NAME      IMAGE     COMMAND   SERVICE   CREATED   STATUS    PORTS
```


## Referencias

Ver la documentación oficial en linea de [Docker Complse](https://docs.docker.com/compose/)

[^1]: Ver la documentación acerca [volumenes en Docker Compose](https://docs.docker.com/engine/storage/volumes/)

[Return to main page]({{site.baseurl}}/).