---
layout: default
title: Tipos de Comandos en Linux
permalink: /tipos-comandos/
parent: El Shell
grand_parent: Linux
has_children: false
has_toc: false
nav_order: 4
---

# Tipos de Comandos en Linux

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

## Que es un Comando?

(what is it, why you need them, and what can you do with them?)
## Estructura de un Comando

Los comandos en Linux tienen cierto formato mas o menos consistente.
```
<comando> <opciones> <argumentos>
```
Podemos definir cada parte de esta manera:
* comando
  * el nombre del archivo o programa con atributo de ejecución
  * ejecuta la acción deseada
* opciones
  * modifica o mejora la salida del comando
  * frequentemente se combina con otras opciones
  * como el nombre lo indica, su uso es opcional
* argumentos
  * el objeto contra el que el comando ejecuta la acción
  * a menudo se puede pasar mas de un argumento
  * su uso puede ser opcional, y frequentemente tiene un sujeto por defecto

## Tipos de Comandos

Linux provee una gran cantidad de comandos y utilidades para manejar ares especificas del sistema operativo. 

**Comandos de Uso General**

Esto son comandos que son usualmente usados para interactuar con el sistem de manera generalizada. Por ejemplo: ls, cd, chown, useradd, etc.

**Comandos de Procesos**

Estos comandos so usados para manejar procesos del sistem y aplicaciones.

**Comandos de Network**

Esto son comandos usados en el manejo y configuracion de NICs, IP Addresses, routes, etc.
## Ejecucíon Simple o Enlazada

Usualmente entramos comandos cuya función es simple.

Hacer eco de una palabra en la terminal.
```
$ echo "Hola, Mundo!"
```
Podemos embuir otros comandos. Aqui damos un saludo e integramos el comando `date` dentro del comando `echo` para mostrar el saludo y la fecha de hoy. Notese que el comando entero a echo esta en doble comillas.
```
$ echo "Hola, Mundo! Hoy es `date`"
```
En este ejemplo, separamos los comandos con un semicolon `;` para ejecutar los comandos en sucesión.
```
$ echo "Hola, Mundo!"; date
```
Aqui hacemos eco de la frase, usamos la barra vertical `|` (pipe) para manipular la salida del comando y obtener un resultado modificado.
```
$ echo "Hola, Mundo!" | grep -i "mundo"
```
Ahora usemos redirección con el símbolo `>` para mandar la salida del comando a un archivo. Luego hacemos uso del comando `cat` para ver el resultado.
```
$ echo "Hola, Mundo!" > holamundo.txt

$ cat holamundo.txt
```

## Comandos en Programas

Esta bien que usemos instrucciones generales en el indicador, pero cuando las tareas crecen en complejidad es necesario agrupar tareas en programs (scripts). 
 
Veamos un bash script que combine todos los ejemplos anteriormente expuestos.
```bash
#!/bin/bash
#-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# Di hola y muestra la fecha
#
# Variables:
#   DATE: especifica la fecha en formato mes/dia/año 
#    LOG: apunta a un archivo de registro para capturar la salida del comandos
#
#-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
DATE=`date +"%m/%d/%y"`
LOG=greeting.log

# verificar que LOG existe, y si no, crearlo.
[ -f ${LOG} ] || echo "INFO: creando ${LOG} porque no existe" && touch ${LOG}

# decir hola y mandar el resultado el archivo LOG
function greeting(){
  echo "Hola, Mundo! Hoy es ${DATE}" > ${LOG}
} 

# ejecutemos la funcion
greeting
```
En este programa pasan varias cosas:
- declaramos variables para almacenar valores que usamos en el código
- verificamos la existencia de un archivo y tomamos una acción de acuerdo al resultado
- hacemos uso de una funcion para aislar un sección del código que hace una tarea específica
- usamos el nombre de la funcion para ejecutar el código

Una cosa es clara, los programas ofrecen el método de agrupar comandos para ejecutar tareas que de otra manera seria imposible en la CLI.

Mas tarde veremos en detalle la composición y estructura de programas de Bash.

[Return to main page]({{site.baseurl}}/).
