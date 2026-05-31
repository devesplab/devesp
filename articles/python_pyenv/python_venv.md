---
layout: default
title: Python Entorno Virtual en Ubuntu
permalink: /python-venv-ubuntu/
parent: Articulos
has_children: false
has_toc: false
nav_order: 2
---

## Entorno Virtual de Python en Ubuntu

{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

**DESCRIPCIÓN**

Este documento explica cómo crear un entorno virtual Python (venv) en Ubuntu 24.04 para un usuario determinado. Por usuario significa que puedes instalar esto en tu directorio personal y usarlo en lugar de la instalación por defecto de Python para todo el sistema.

Para crear un entorno virtual en Python en Ubuntu 24.04, usas el módulo `venv` que Python provee via PIP. Esto aísla las dependencias por proyectos en la instalación específica del ambiente virtual de Python.

En esta lección:

- instalar python3
- Crear un entorno virtual de Python
- Resolución de problemas al crear un entorno virtual de Python
- eliminar un entorno virtual de python

**DEPENDENCIAS**

ninguna

**REQUERIMIENTOS**

Sistema Ubuntu Linux. <br> 
Algunos comandos requieren privilegios elevados.

**ADVERTENCIA**

ningunos.

## Ambiente de trabajo

En esta lección usamos el sistema operativo Ubuntu.

```bash
-> lsb_release -a
Distributor ID:	Ubuntu
Description:	Ubuntu 24.04.4 LTS
Release:	24.04
Codename:	noble
```

## Guía Rápida

Instalación manual de Python.

{: .note}
Python3 está disponible por defecto en Ubuntu 24.04.

Python3 está disponible por defecto en Ubuntu 24.04.
Pero, si quieres instalar una versión manualmente, haz los comandos que aparecen a continuación.

```bash
# instalar dependencias
sudo apt-get install software-properties-common

# agregar repositorio
sudo add-apt-repository ppa:deadsnakes/ppa

# actualizar el sistema
sudo apt-get update

# instalar paquetes
apt-get install python3 python3-dev
```

## Crea Carpeta Del Entorno Virtual

Abre el terminal. <br>
Navega al directorio de tu proyecto usando el comando cd. 

Si aún no tienes una carpeta de proyectos, crea una usando `mkdir` y luego cambia a esa carpeta con el comando `cd`.

Puedes crear tantos ambientes virtuales como quieras en la ubicación que prefieras.<br>
Elige el nombre de directorio que quieras. Por ejemplo, aqui he decidido crear la carpeta `$HOME/python-venv`.

```bash
# crear la carpeta de destino
mkdir $HOME/python-venv

# verificar la carpeta fue creada
ls -ld $HOME/python-venv
```

Cambiemos a la nueva carpeta. 

```bash
cd $HOME/python-venv
```

Hasta este punto la carpeta no tiene contenido.

## Crea El Entorno Virtual

Ejecuta el siguiente comando para crear un entorno virtual. Es una práctica habitual llamar el entorno `venv` o `.venv`, pero puedes ponerle el nombre que quieras.

Sintaxis:

- `venv`: es el módulo que Python utiliza para crear el entorno
- `virtual-env-name`: es el nombre del entorno que elijas

```bash
python3 -m venv <virtual-env-name>
```

{: .note }
> `venv` es un módulo nativo de Python <br>
> puedes nombrar a tu entorno virtual de Python como quieras.

Opcionalmente, podemos identificar la versión de python y usarla en el nombre del ambiente virtual para pode identificarlo fácilmente en caso que queramos crear ambientes para diferentes versiones de Python.

```bash
# identificar la version de python
-> python3 --version
Python 3.12.3

# usar la version en el nombre del ambiente virtual a crear
-> python3 -m venv venv-3.12.3

# listar el resultado
-> ls -ld venv-3.12.3/
drwxrwxr-x 5 devuser devuser 4096 May  9 21:08 venv-3.12.3/
```

En este ejemplo, la version de python es `3.12.3`, y decidimos crear un entorno llamado `venv-3.12.3`. Esta acción crea una carpeta llamada `venv-3.12.3` que coincide con el nombre del ambiente. Esta carpeta contendrá todo lo necesario para un entorno de Python completo y listo para desarrollar software.

En este listado parcial usamos el comando `tree` para mostrar el contenido de la carpeta BIN del entorno que hemos creado.

```bash
-> tree venv-3.12.3/ | head -15
venv-3.12.3/
├── bin
│   ├── Activate.ps1
│   ├── activate
│   ├── activate.csh
│   ├── activate.fish
│   ├── pip
│   ├── pip3
│   ├── pip3.12
│   ├── python -> python3
│   ├── python3 -> /usr/bin/python3
│   └── python3.12 -> python3
├── include
│   └── python3.12
├── lib
```

## Activa El Python Venv

Re-lee del archivo de esta manera para activar el entorno:

```bash
# activate Python venv
source ~/python-venv/venv-3.12.3/bin/activate
```

A partir de ahora el entorno de python esta listo para usar.

El indicador cambiará para mostrar el nombre del entorno virtual en la primera línea. En este ejemplo se ve `(venv-3.12.3)`.

```bash
(venv-3.12.3)
Sat 2026May09 21:13:33 UTC
devuser@client1
~/python-venv
hist:109 -> 
```

Ahora, verifiquemos cual es el binario de `python` y `pip` predeterminado.

```bash
-> which python
/home/devuser/python-venv/venv-3.12.3/bin/python

-> python --version
Python 3.12.3

-> which pip
/home/devuser/python-venv/venv-3.12.3/bin/pip

-> pip --version
pip 24.0 from /home/devuser/python-venv/venv-3.12.3/lib/python3.12/site-packages/pip (python 3.12)
```

De ahora en adelante, por defecto, usarás los binarios en este entorno virtual.

Puedes desactivar el entorno virtual de Python simplemente ejecutando:

```bash
-> deactivate
```

## Establecer PyEnv en Ubuntu 24.04 (.bashrc / .zshrc)

Añadimos la línea de exportación en `~/.bashrc` para habilitar el entorno cuando iniciamos una sesión en un terminal.

```bash
export PATH=/home/devuser/python-venv/venv-3.12.3/bin:$PATH
source ~/python-venv/venv-3.12.3/bin/activate
```

De esa manera no tenemos que activar el entorno de python manualmente.

## Resolución de problemas

Siendo que el entorno de trabajo cambia de sistema a sistema, es casi inevitable que encontraremos problemas. Veamos a continuación la solución de algunos casos.

### Fallo para crear un entorno virtual de Python

Aquí hubo un problema y no se logró crear un entorno virtual de Python.

```bash
-> python3 -m venv venv1
The virtual environment was not created successfully because ensurepip is not
available.  On Debian/Ubuntu systems, you need to install the python3-venv
package using the following command.

    apt install python3.12-venv

You may need to use sudo with that command.  After installing the python3-venv
package, recreate your virtual environment.

Failing command: /home/devuser/python-venv/venv1/bin/python3
```

Básicamente, el error indica que nuestro sistema no contiene los paquetes necesarios para crear entornos de python. El mensaje nos dice que debemos usar `apt` para instalar las dependencias requeridas.

Comprobemos la versión instalada en Python.

```bash
-> python3 --version
Python 3.12.3
```

Como indica el mensaje, instala el paquete usando el número de versión indicado arriba.

```bash
sudo apt install python3.12-venv
```

Aquí vemos la salida parcial del comando anterior para instalar el paquete:

```bash
...snip...
Unpacking python3.12-venv (3.12.3-1ubuntu0.13) ...
Setting up python3-setuptools-whl (68.1.2-2ubuntu1.2) ...
Setting up python3-pip-whl (24.0+dfsg-1ubuntu1.3) ...
Setting up python3.12-venv (3.12.3-1ubuntu0.13) .
```

`python3-venv`: es un paquete de Debian/Ubuntu que instala el módulo `stdlib` venv y cualquier archivo de soporte a nivel de sistema operativo necesario para crear entornos virtuales desde el sistema Python.

`python3 -m venv`:  es el comando que realmente crea un entorno virtual usando el módulo `venv` desde el intérprete python3 que invoques.

## Desinstalar Un venv de Python

Para desinstalar un venv en Python, simplemente elimina el directorio en el que lo creaste.

```bash
rm -rf ~/python-venv/venv-3.12.3
```

Además, elimina cualquier variable de exportación en `~/.bashrc` si has añadido alguna.

## Conclusion

Este proceso mostró cómo crear un entorno local de Python listo para su desarrollo completo. Podemos escribir y ejecutar código. Instala módulos adicionales de python para ampliar su funcionalidad. Podemos experimentar creando más de un entorno virtual y adaptarlo a un caso de uso específico.

## Referencias

### Glosario De Comandos

python3
: El binario estándar de Python

pip
: Utilidad para gestionar módulos de Python

### Referencias Útiles

- [Documentación Oficial De Python](https://www.python.org/doc/)
- [Cómo configurar un entorno de desarrollo para Python en Ubuntu](https://documentation.ubuntu.com/ubuntu-for-developers/howto/python-setup/)

[Return to main page]({{site.baseurl}}/)