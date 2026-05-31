---
layout: page
title: Ansible
permalink: /ansible/
nav_order: 4
has_children: true
has_toc: false
---

<div class="topnav">
 <a class="active" href="../index">Home</a>
 <a href="../about">About</a>
 <a href="../news">News</a>  
</div>

# Ansible

{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

## Ansible como herramienta

Podemos visualizar la arquitectura de Ansible en una forma simple.

```
       ┌─────────────────────────────────────────────────┐                 
       │                 Nodo de Control                 │         
       │ ----------------------------------------------- │         
       │          Ansible      //       Inventario       │                 
       └────────────────────────┬────────────────────────┘           
              ┌─────────────────┼─────────────────┐                           
              │                 │                 │                
┌─────────────▼─────────────────▼─────────────────▼───────────────┐
│                        Nodos Clientes                           │
| --------------------------------------------------------------- |
│  ┌──────────┐         ┌───────────┐           ┌───────────┐     │
│  │ node1    │         │ node2     │           │ node3     │     │
│  └──────────┘         └───────────┘           └───────────┘     │
└─────────────────────────────────────────────────────────────────┘
```

La infraestructura consiste de tres componentes:

1. El nodo de control donde existe el binario de Ansible y el código necesario (playbooks) para configurar los clients.
2. El inventario conteniendo las lista de clientes a manejar
3. Los nodos, o clients a manejar

## Propósito General de uso de Ansible

Al trabajar en un ambiente de computación ejecutamos una variedad de tareas al configurar sistemas:

- entramos comandos en el indicador
- escribimos programas
- instalamos aplicaciones
- editamos archivos de configuración
- paramos y empezamos servicios

Al principio la tarea puede ser tolerable pero después de un tiempo el trabajo se vuelve repetitivo, pero eventualmente cuando el número de sistemas crece podemos cometer errores costosos. Una cosa es trabajar en un par de sistemas, otra cosa es hacerlo en diez, cuarenta o cien sistemas.

El uso primario de Ansible es orquestar la automatización, configuración y manejo de un ambiente con una variedad de nodos y aplicaciones a menor y gran escala. Al correr los playbooks de Ansible nos aseguramos que el ambiente es exactamente el mismo cada vez que corremos los playbooks.

{: .highlight }
**Ansible sirve para orquestar la automatización, configuración y manejo de un ambiente de computación.**

Ansible trabaja en varios sistemas operativos y proveedores de nubes. Esto hace de Ansible una herramienta muy versátil de uso general.

## Mejores prácticas para usar Ansible

**Organización de proyecto**

Como en todo tipo de proyecto, las cosas empiezan simples, pero eventualmente la estructura crece en complejidad. Una buena practica es organizar los diferentes componentes de manera que sean fáciles de rastrear y mantener.

**Usar variables**

Debemos evitar a todo costo en escribir valores directamente en los playbooks, en vez de eso, usemos variables que obtengan valores dinámicamente. Eso es crucial en ambientes que no son uniformes y requerimos inyectar valores específicos, por ejemplo nombres de usuarios, números de cuentas, designaciones geográficas, etc.

**Estructura del Inventario**

Ansible es dependiente del inventario para correr. El inventario debe estructurarse de tal manera que el playbook se aplique solo al area deseada y en la secuencia deseada. Si es posible, debemos usar inventarios dinámicos porque seguramente la lista de clientes es elástica, es decir crecerá o disminuirá pasado el tiempo.

**Manejo de Errores**

Es importante probar situaciones que pueden causar problemas. Debemos tener en cuenta que hay situaciones en las que no debe ignorarse un situación problemática. Un ejemplo es cuando deseamos bajar un archivo de configuración y un fallo de red impide completar ese paso, lo que a su vez impide que podamos empezar un servicio que requiere el archivo de configuración.

**Seguridad**

Debemos usar `become` para dar el privilegio requerido para ejecutar el playbook. Pero quizá mas importante es aplicar altas medidas de seguridad al **Nodo de Control** siendo que tiene acceso al ambiente entero.

{: .warning }
Debemos aplicar altas medidas de seguridad al Nodo de Control siendo que tiene acceso al ambiente entero.

**Documentación**

Como en todo sistema de desarrollo, es deseable tener un sistema de documentación claro y exhaustivo que explica puntos claves de los playbooks. En particular el flujo de ejecución, dependencias, uso de variables, posibles causas de fallos, limitaciones de recursos, aspectos de seguridad y otros aspectos que afecten el resultado final.

## Limitaciones del uso de Ansible

Ansible usa el modelo the empuje para manejar clientes, es decir, el usuario debe iniciar la operación en el nodo de control. Eso significa que si algún cliente a un cierto punto difiere de la configuración estándar, Ansible no lo revertirá al estado original automáticamente. Este modelo de Ansible es diferente de Puppet y Chef por ejemplo, los cuales inician el trabajo cuando detectan que un cliente ha cambiado.

Ansible provee apoye sustancial a ambientes de Linux, pero no asi a ambientes de Windows. Por esta razón podemos tener un poco de problemas cuando deseamos aplicar configuraciones en ambientes heterogéneos.

Un detalle importante es que el usuario debe estructurar los playbooks de manera lógica puesto que Ansible no provee la manera de organizar las tareas de una manera particular. Esto se deja al juicio del usuario para asegurarse de la secuencia correcta de ejecución.

## En Resumen

Ansible es una herramienta usada para automatizar manejo de configuración a gran escala. Al aplicar mejores prácticas podemos asegurarnos que tenemos un ambiente de desarrollo adecuado. Un estructura clara de organización lleva a una ejecución predecible y fácil de entender. Un entendimiento de las limitaciones evitará car en trampas que causen problemas. Al usarse adecuadamente, Ansible puede ser una herramienta que mejora la productividad.

{: .highlight }
Al usarse adecuadamente, Ansible puede ser una herramienta que mejora la productividad.

Ansible es una herramienta activamente desarrollada y mantenida por un gran numero de desarrolladores. Asi que podemos estar seguros que usaremos un producto confiable.

## Referencias

Las referencias siguientes están actualizadas para leer acerca de Ansible

- [Documentación Oficial de Ansible](https://docs.ansible.com)
- [Ansible Colaborativo](https://www.redhat.com/en/ansible-collaborative)
- [Repositorio de Git de Ansible](https://github.com/ansible/ansible)

Donde encontrar ayuda?

- [Ansible en Stackoverflow](https://stackoverflow.com/questions/tagged/ansible)

> Desafortunadamente no hay una sección en Español para Ansible en Stackoverflow

[Return to main page]({{site.baseurl}}/).
