---
layout: default
title: Procesos
permalink: /procesos/
parent: Linux
has_children: true
has_toc: false
nav_order: 9
---

## Procesos en Linux

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

## Que es un Proceso?  

Un proceso Unix es una instancia de un programa que se ejecuta en el sistema operativo Unix. Cuando se ejecuta un comando o una aplicación, el Unix Kernel crea un proceso para gestionar su ejecución. Linux hace uso del Kernel para segmentar recursos internos separados para cada proceso.

El Linux Kernel juega el siguiente rol:

- alocar y manejar los recursos internos necesarios para la ejecución de un proceso
- manejar el horario cuando empezar o terminar un proceso
- interactuar con el IPC (Inter Process Communication) para que diferentes procesos se comuniquen entre si a traves de pipas, señales y sockets.

Un proceso pertenece al usuario especifico que lo empezó. El usuario tiene cierto control sobre el proceso tales como monitorear la ejecución, terminarlo, o cambiar la prioridad.

Cada proceso es identificado por un nombre predefinido, un id numérico llamado `process id` y un id numérico padre llamado `parent process id`.

En general, un proceso de Linux es una unidad independiente de ejecución que interactúa con el Kernel y con otros proceses para ejecutar tareas y compartir recursos de una manera coordinada.

Cada proceso requiere un espacio de memoria que incluye Segmento de código, Segmento de datos, Montón (Heap) y Pila (Stack).

El Segmento de código contiene el código ejecutable.
El Segmento de datos almacena variables globales y variables estáticas.
El Montón (Heap) un área de memoria dinámica utilizada para variables cuyo tamaño puede cambiar durante la ejecución.
La Pila (Stack) se utiliza para llamadas de función, variables locales e información de control.

### Estado de Proceso

Un proceso puede estar en varios estados, incluidos:

En ejecución
: indica que esta activo en la table de procesos.

En espera
: espera a que se produzca algún evento como requisito para continuar

Detenido
: se detiene temporalmente, generalmente por señales

Zombie
: Ejecución completada pero que aún tiene una entrada en la tabla de procesos, esperando que su padre lea su estado de salida

No todos los procesos son iguales. Cada proceso se clasifica de acuerdo a su importancia y es priorizado por el Kernel de Unix de acuerdo a la criticalidad del mismo. La prioridad se usa como medida para asignar recursos y el tiempo de CPU entre ellos.

### ID de Proceso (PID, PPID)

En cualquier momento dado pueden haber docenas o cientos de procesos activos en un sistema. Unix asigna a cada proceso un identificador numérico único para diferenciarlos y asi poder realizar un seguimiento y gestionarlo. El valor numérico es conocido como `PID`, el cual es un valor numérico asignado por el Kernel para identificar y manejar el proceso. El PID es muy útil para diagnosticar problemas que ocurren durante la ejecución de tareas que requieren arreglos u optimización. 

Generalmente cada proceso tiene origen de un proceso padre el cual se conoce como `PPID`. Los procesos pueden crear subprocesos (procesos hijos). Cada proceso tiene un proceso padre, excepto el proceso inicial (normalmente con PID 1, conocido como init en un sistema Unix típico).

Para ver la tabla de procesos activos usamos el comando `ps` y le agregamos algunas banderas para modificar la salida del comando.

```bash
root@ubuntu2204-2-devesp
/
hist:15 -> ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 02:40 ?        00:00:00 /usr/sbin/init sleep infinity
root          27       1  0 02:40 ?        00:00:00 /lib/systemd/systemd-journald
systemd+      46       1  0 02:40 ?        00:00:00 /lib/systemd/systemd-resolved
root         642       1  0 02:40 ?        00:00:00 /usr/lib/postfix/sbin/master -w
postfix      644     642  0 02:40 ?        00:00:00 qmgr -l -t unix -u
postfix      692     642  0 03:25 ?        00:00:00 pickup -l -t unix -u -c
```

La salida muestra las columnas siguientes:

UID
: id del usuario al que pertenece el proceso

PID
: proceso de id, valor numérico que identifica el proceso

PPID
: id del proceso padre, valor numérico que identifica el proceso padre

C
: porcentaje de utilización de CPU

STIME
: tiempo que ha transcurrido desde que el proceso empezó

TTY
: tipo de terminal asociado con el proceso

TIME
: tiempo total de CPU que el proceso ha consumido

CMD
: comando utilizado para instanciar el proceso

{: .highlight }
El proceso  **init** es el proceso padre con valor numérico `1`.

Es de notar que el primer proceso el la tabla arriba tiene `PID` igual a `1`, lo cual indica que es el proceso **init**, el cual es el proceso primario conocido como _**proceso padre**_. Luego siguen varios procesos con `PPID` igual a `1`, lo cual indica que son subprocesos (procesos hijos) del proceso **init**.

## El IPC (Comunicación Entre Procesos)

Los procesos en Unix deben comunicarse entre si para coordinar sus acciones. Nativamente, los procesos no tienen la capacidad de comunicarse entre si. Este trabajo le pertenece al IPC, o Comunicación Entre Procesos el cual usa mecanismos como tuberías, colas de mensajes, memoria compartida y sockets que se usan para la comunicación y el intercambio de datos entre dos o más procesos que se ejecutan simultáneamente en un sistema. Permite que diferentes procesos se comuniquen entre sí y compartan datos, permitiéndoles trabajar juntos y coordinar sus acciones.

Los procesos pueden enviar y recibir señales para gestionar la ejecución (por ejemplo, finalizar, pausar) a través del mecanismo de señales de Unix.

Hay varios mecanismos de IPC disponibles en los sistemas Unix, cada uno con sus propias características y casos de uso. Los métodos de IPC más comunes incluyen:

- **Conexiones:** un canal de comunicación unidireccional que conecta la salida de un proceso con la entrada de otro. Las conexiones con nombre (o FIFO) permiten la comunicación entre procesos no relacionados a través del sistema de archivos.
- **Colas de mensajes:** un método que permite que los procesos envíen y reciban mensajes en una estructura de cola. Los mensajes se pueden priorizar, lo que brinda más control sobre el orden en el que se procesan.
- **Memoria compartida:** un segmento de memoria que se  puede compartir entre varios procesos, lo que les permite leer y escribir en el mismo espacio de memoria. Este método es muy eficiente ya que permite el acceso directo a los datos compartidos sin necesidad de copiarlos.
- **Semáforos:** mecanismo de sincronización utilizado para controlar el acceso a recursos compartidos por parte de múltiples procesos. Ayudan a evitar condiciones de carrera al permitir que solo una cierta cantidad de procesos accedan a un recurso al mismo tiempo.
- **Sockets:** aunque se utilizan principalmente para la comunicación a través de una red, los sockets de dominio Unix también se pueden utilizar para la comunicación entre procesos en la misma máquina, lo que permite la comunicación tanto por flujo como por datagrama.

Estos métodos de IPC ofrecen diferentes ventajas y desventajas en términos de complejidad, rendimiento y facilidad de uso, lo que permite a los desarrolladores elegir el mecanismo más adecuado según los requisitos específicos de su aplicación.

## Referencias

### Glosario De Comandos

### Referencias Útiles

- [rfc62 de IPC](https://datatracker.ietf.org/doc/html/rfc62)

[Return to main page]({{site.baseurl}}/).