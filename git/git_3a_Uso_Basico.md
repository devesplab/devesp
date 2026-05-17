---
layout: default
title: Uso Básico de Git
permalink: /basico-git/
parent: Git
has_children: false
has_toc: false
nav_order: 2
---

# Uso Básico de Git

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

En esta página discutimos lo siguiente:
- inicializar una carpeta para uso con control de revision
- clonar un repositorio existente de github
- entender origen (origin) de github
- hacer cambios locales y propagarlos a un repositorio

En esta leccion usamos el sistema operativo Ubuntu.<br>
Usamos el cliente de Git >= 2.0

## Comandos Básicod de Git

Estos son los comandos que usamos con frequencia cuando interactuamos con git.

Inicializar repositorio:
```
git init myRepo
```
Hacer seguimiento de archivos que cambian:
```
git add <archivo> 
```
Confirmar cambios en un archivo:
```
git commit -am"mis actualizaciones"
```
Agregar un origin:
```
git remote add <nombre-de-origen> <git-url>
```
Empujar cambios locales:
```
git push <archivo> <rama>
```
Clonar un repositorio de git:
```
git clone <git-url>
```
Bajar las actualizaciones mas recientes:
```
git pull origin <branch>
```

## Usando Git Por Vez Primera

A continuación vamos a ejercitar el uso de git.

En nuestro ordenador local es fácil empezar usar git para control de revision. 

En este ejemplo básico, creamos un directorio, cambiamos a ese directorio y lo inicializamos para que este bajo control de revision con git.
```bash
-> mkdir data
-> cd data
-> git init
```
A partir de este momento tenemos la habilidad de registrar todo cambio hecho a los datos del repositorio.

El comando `git init` crea el directorio escondido `.git`
```bash
-> ls -la ~/data
total 28
drwxrwxr-x 3 devuser devuser 4096 Jun  9 01:05 ./
drwxr-xr-x 1 devuser devuser 4096 Jun  9 01:07 ../
drwxrwxr-x 8 devuser devuser 4096 Jun  9 01:06 .git/

-> ls -l ~/data/.git
total 48
-rw-rw-r--  1 devuser devuser    9 Jun  9 01:06 COMMIT_EDITMSG
-rw-rw-r--  1 devuser devuser   99 Jun  9 01:02 FETCH_HEAD
-rw-rw-r--  1 devuser devuser   21 Jun  9 01:02 HEAD
drwxrwxr-x  2 devuser devuser 4096 Jun  9 01:02 branches/
-rw-rw-r--  1 devuser devuser  310 Jun  9 01:02 config
-rw-rw-r--  1 devuser devuser   73 Jun  9 01:02 description
drwxrwxr-x  2 devuser devuser 4096 Jun  9 01:02 hooks/
-rw-rw-r--  1 devuser devuser  281 Jun  9 01:06 index
drwxrwxr-x  2 devuser devuser 4096 Jun  9 01:02 info/
drwxrwxr-x  3 devuser devuser 4096 Jun  9 01:02 logs/
drwxrwxr-x 11 devuser devuser 4096 Jun  9 01:06 objects/
drwxrwxr-x  5 devuser devuser 4096 Jun  9 01:02 refs/

```

Mantén el directorio `.git` privado y bajo control de versiones solo por Git: no lo muevas a tu árbol de trabajo, no lo añadas a otros repositorios y nunca lo comprometas en otro repositorio; Haz una copia de seguridad por separado (o usa git bundle) si necesitas una copia de seguridad completa del repositorio.

{: .warning }
El directorio `.git` nunca debe ser intencionalmente alterado manualmente. No se debe copiar, mover, editar or borrar archivos o carpetas. 

El directorio `.git` es donde Git guarda toda la información y metadata del repositorio. Contiene toda la historia del projecto, incluyendo los cambios hechos, ramas, etiquetas, y ajustes de configuración. Este directorio es escencial para mantener un registro necesario para el control de revisión. En caso que el directorio `.git` sea borrado, se pierde toda referencia historica del desarrollo del projecto.

## Manejando La Rama Predeterminada

Inicialmente, la rama predeterminada de un repositorio de github recien creado se llama `master`. Sin embargo, hoy día la convención general es nombrar la rama `main`.

Git crea la rama predeterminada cuando inicializamos un repositorio con el propósito de usarla como la rama primaria de desarrollo, es decir, el lugar donde hacemos toda operacion inicial de desarrollo. Otros desarrolladores vendran a esta rama como lugar de empiezo para su trabajo.

{: .note }
Podemos usar cualquier nombre que deseamos para nombrar la rama prederminada de un repositorio de github.

Si así lo deseamos, podemos configurar nuestro sistema local de tal manera que cada vez que creamos un repositorio tomará `main` como el nombre de la rama predeterminada. Para lograr esto usamos el comando siguiente:

```bash
-> git config --global init.defaultBranch main
```

Para verificar el ajuste usemos este comando.

```bash
-> git config --global init.defaultbranch
```

Para listar todos los ajustes que tenemos usemos este comando

```bash
-> git config --global --list
```

Si tiempo después de hacer el ajuste a usar `main` como rama predeterminada, queremos usar una nueva rama llamada `develop`, la podemos cambiar así:

```bash
git config --global init.defaultBranch develop
```

Si no deseamos tener rama predeterminada en nuestro sistema local, hacemos lo siguiente:

```bash
-> git config --global --unset init.defaultBranch
```

Github registra todos los ajustes personalizados en el archvo `$HOME/.gitconfig`. El ajuste para rama predeterminada se ve así:

```bash
[init]
	defaultBranch = main
```

Además de hacer el ajuste con `git config`, debemos cambiar el repositorio en que estamos trabajando de esta manera:

```bash
-> git branch -m main
```

## Clonar Repositorio Externo

Github provee el cliente de `git` para manipular control de revision en nuestra sistema local.

{: .important }
Si aún no lo has hecho, sigue las instructiones para instalar el cliente de git en tu sistema.

Para usar un repositorio existente en [github](https://github.com), solo tenemos que clonarlo usando el cliente the git.
```
-> git clone https://github.com/devesplab/git-devesp.git
```

El comando anterior creará un directorio con el nombre del repositorio `git-devesp`. Podemos cambiar a ese directorio y empezar a trabajar.
```
-> cd git-devesp
```

## Manejando El Origen

El origen ("origin" en Inglés) de git es una referencia a la URL o API de punto de donde podemos bajar o empujar cambios.

Cuando clonamos un repositorio usando `git clone`, el origin es `origin` for defecto y apuntando a la URL de Github.

Cuando creamos un repositorio local, debemos agregar la referencia al origen para poder bajar y empujar cambios.

Siguiendo el ejemplo que estamos discutiendo:
- creamos un nuevo directorio
- cambiamos al directorio
- agregamos un origen remoto usando un TOKEN para autenticación.
- verificamos el origen remoto
- immediatamente bajamos el contenido del repositorio y la rama predeterminada

```bash
devuser@ubuntu2204-2-devesp
-> mkdir data

devuser@ubuntu2204-2-devesp
hist:41 ->  cd data

devuser@ubuntu2204-2-devesp
~/data 
-> git init

devuser@ubuntu2204-2-devesp
~/data 
-> git remote add origin https://devesplab:<myGitToken>@github.com/devesplab/git-devesp.git

devuser@ubuntu2204-2-devesp  git(main)
~/data 
-> git remote -v
origin	https://devesplab:<myGitToken>@github.com/devesplab/git-devesp.git (fetch)
origin	https://devesplab:<myGitToken>@github.com/devesplab/git-devesp.git (push)

devuser@ubuntu2204-2-devesp
~/data 
-> git pull origin main
```

{: .warning }
Github no permite el uso de contraseñas. Debemos usar un TOKEN para autenticación.

En el ejemplo anterior truncamos el token para mejor legibilidad.

## Hacer y Empujar Cambios

En nuestro ejemplo, usamos el comando `git status` que muestra que inicialmente no tenemos nada.
```bash
devuser@ubuntu2204-2-devesp  git(main)
~/data
-> git status
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

De ahora en adelante hacemos referencia a "origin" para cualquier operación que envuelva bajar o empujar cambios. Enseguida creamos un nuevo archivo.
```bash
devuser@ubuntu2204-2-devesp  git(main)
~/data
-> echo "Hello, Devesp!" > hello.txt

```
Y ahora empujamos el nuevo cambio.
```bash
devuser@ubuntu2204-2-devesp  git(main)
~/data
-> git add hello.txt

devuser@ubuntu2204-2-devesp  git(main)
~/data
-> git commit -am "agregar data"

devuser@ubuntu2204-2-devesp  git(main)
~/data
-> git push origin main
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 6 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 299 bytes | 299.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/devesplab/git-devesp.git
   9a719b8..a3e6d62  main -> main
```

Para verificar solo habramos el repositorio en el navegador web.
```bash
https://github.com/devesplab/git-devesp.git
```

## Conclusion

Una vez que nuestro trabajo como desarrolladores crece en complejidad, es necesario rastrear los cambios que se hacen pasado el tiempo. Puede que halla mas de un desarrollador trabajando en un proyecto, o talvéz hay varios porciones interralacionas que tienen que registrarse. Git ayuda a mantener el auditaje y preservar los cambios históricos de un proyecto.

## Referencias 

### Glosario De Comandos

Los comandos siguientes son usados frecuentemente en sesiones de Linux.

git 
: cliente de github para la manipulación local de control de version de repositorios de github

### Referencias Utiles

DevEsp :: Linux

- [git-devesp](https://github.com/devesplab/git-devesp.git)

Referencias en línea:
- [Documentación General de Git](https://git-scm.com/docs)
- Client de [git](https://git-scm.com/docs/git)
- Parámetros de [git-config](https://git-scm.com/docs/git-config/2.22.0)

[Return to main page]({{site.baseurl}}/).