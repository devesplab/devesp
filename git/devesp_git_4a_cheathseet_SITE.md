---
layout: default
title:  Trucos Técnicos de Git
permalink: /git_cheatsheet/
parent: Git
has_children: false
has_toc: false
nav_order: 4
---

#  Trucos Técnicos de Git

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

## Flujo direccional de Git

Esto muestra el flujo típico de operaciones Git entre un cliente y un servidor. La dirección del flujo indica dónde la acción se inicia y dónde termina.

```sh
Operation  Direction       Description
---------- --------------  -------------------------------------------------------
git clone  server->client  descarga del repositorio inicial desde el repositorio remoto
git pull   server->client  descarga las últimas actualizaciones desde el repositorio remoto
git push   client->server  sube o publica tus cambios en el repositorio remoto
git add    client only     solo pone los archivos bajo control de revisión en el espacio de trabajo local
git commit client only     solo registra los cambios en el espacio de trabajo local
```
Se puede acceder a un repositorio git remoto de varias maneras según el protocolo:

- git://example.com/proj/project.git
- https://example.com/proj/project.git
- ssh://user@example.com/proj/project.git

Cada ejemplo utiliza un protocolo diferente.

{: .note }
Tenga en cuenta la ausencia de HTTP, el cual es un protocolo inseguro.

## Crear un repositorio Git

Esta es la primera tarea necesaria para implementar el control de revisiones en un proyecto.

Un usuario sigue una sequencia mas o menos así:

- Crear un proyecto en su máquina local.
- Ir a la ubicación donde se ubicará el proyecto. Debe ser una ubicación donde el usuario tenga acceso completo.
- Usar el cliente git para inicializar el repositorio git.

Ejemplo:

En este ejemplo, el usuario crea el directorio `projectX` en `$HOME`. Luego, cambia a esa ubicación y usa el cliente Git con el parámetro `init` para comenzar a implementar el control de revisión.

```
-> cd /home/devuser/
-> mkdir projectX
-> cd projectX/
-> git init
```

{: .note }
Más adelante, el usuario sube el repositorio a una ubicación externa como `github.com`, lo cual explicamos mas adelante.

## Operaciones de origen de GIT

Añadir un origen a un repositorio Git permite especificar el repositorio remoto predeterminado asociado a tu repositorio local. Esto simplifica la colaboración y el control de versiones, ya que proporciona un punto de referencia práctico para enviar cambios y obtener actualizaciones desde una ubicación remota compartida. El origen actúa como un _alias_ para la URL del repositorio remoto, que suele estar alojado en plataformas como GitHub, GitLab o Bitbucket. Esto facilita la gestión de conexiones remotas.

### Añadir origen

A continuacion establecemos donde queremos subir los cambios a un  projecto:
- Configura el cliente para que envíe sus confirmaciones al servidor Git remoto.<br>
- Proporciona el nombre de host o la dirección IP del servidor Git.<br>
- Para ello, debemos especificar la URL de origen remoto. [^1]

[^1]: Ver [administrar controles remotos](https://stackoverflow.com/questions/42830557/git-remote-add-origin-vs-remote-set-url-origin) on stackoverflow

Ver la ayuda del client `git` para [manejar remotos](https://git-scm.com/docs/git-remote#Documentation/git-remote.txt-remove).
```
NAME
       git-remote - manage set of tracked repositories
SYNOPSIS
       git remote [-v | --verbose]
       git remote add [-t <branch>] [-m <main>] [-f] [--mirror] <name> <url>
       git remote rename <old> <new>
       git remote rm <name>
       git remote set-head <name> (-a | -d | <branch>)
       git remote set-url [--push] <name> <newurl> [<oldurl>]
       git remote set-url --add [--push] <name> <newurl>
       git remote set-url --delete [--push] <name> <url>
       git remote [-v | --verbose] show [-n] <name>
       git remote prune [-n | --dry-run] <name>
       git remote [-v | --verbose] update [-p | --prune] [group | remote]...
```

**:: Usando `add`**

Usemos `add` para agregar un nuevo control remoto
```
-> cd projectX
-> git remote add origin git@<git-server>:<gituser>/project.git
```

**:: Usando `set-url`**

Usemos `set-url` para cambiar (o reemplazar) la URL de un repositorio remoto existente

Cambiar el origen para usar SSH
```
-> git remote set-url origin git@github.com:lungithub/gitrepo1.git
```
Cambiar el origen para usar HTTPS
```
-> git remote set-url origin https://github.com/lungithub/gitrepo1.git
```
Verificar la configuración de origen.
```
-> git config --get remote.origin.url
```
Lo mismo aparece en el archivo de configuración git en la sección `[remote]`.
```
[remote "origin"]
    url = git@172.16.15.199:project.git
    fetch = +refs/heads/*:refs/remotes/origin/*
```

Ver la lista de orígenes disponibles
```
-> git remote
-> git remote -v
```

## GIT Push


Tras realizar cambios en el proyecto, el objetivo es transferirlos a una ubicación remota.

Primero, debe agregar los cambios a las ramas locales.
```
-> git add .
```
Luego puedes comparar con la rama principal.
```
-> git diff --stat origin/main
```

Envía los cambios al servidor Git.<br> 
Debes estar en el directorio del proyecto para enviar los cambios.
```
-> git push origin main
```

### Eliminar origen

Pasos para eliminar un control remoto [^2].

[^2]: Más información sobre [removing-a-remote](https://help.github.com/articles/removing-a-remote/) en stackoverflow.

{: .important }
Puedes agregar un control remoto en cualquier momento. Puede ser el mismo que eliminaste u otro.

Syntax:
```
git remote rm destination
```
Example:
```
-> git remote rm origin   
```

### Comprueba el control remoto donde estamos enviando datos

Este comando confirma el remoto con el cual interactuamos.
```
-> git remote show origin
```

## Ignorar archivos en Git

En **MACOS X** ignore los directorios ocultos `.DS_Store` [^3].

[^3]: Vea esta publicación de stackoverflow sobre [ignoring .DS_Store]( http://stackoverflow.com/questions/18393498/gitignore-all-the-ds-store-files-in-every-folder-and-subfolder) activado en cada carpeta y subcarpeta

Esto excluye todo `.DS_Store` de un directorio. Puedes hacerlo en el directorio de tu proyecto.
```
-> find . -name .DS_Store -print0 | xargs -0 git rm --ignore-unmatch??
```
Crear un global `.gitignore`
```
-> echo ".DS_Store" > /Users/devuser/.gitignore
```
Configurar git para usar global `.gitignore`
```
-> git config --global core.excludesfile /Users/devuser/.gitignore
```

Añade contenido a `.gitignore`. <br>
Ten en cuenta que puedes añadir comentarios usando `#`.
```
-> cat .gitignore
.DS_Store
*.iso
*.log
*.tar.gz
*.tar
.*
*.[oa]
*~
# Ignore Chef key files and secrets
.chef/*.pem
.chef/encrypted_data_bag_secret
```
No queremos enviar archivos ISO, LOG, TAR o GZ. <br>
Añada patrones de expresiones regulares según sea necesario. <br>
La entrada `.*` ignora archivos ocultos como `.bashrc`.

## Operaciones con archivos GIT

Crea un archivo y ponlo bajo control de revisión.
```
-> cd /home/devuser/projectX
-> vi file.txt
-> git add file.txt
-> git commit -m"Added new file" file.txt
```
Si has realizado varios cambios en un archivo, puedes olvidarlos. Regresa a la versión más reciente registrada.
```
-> git checkout -- file.txt
```
Cambiar el nombre de un archivo
```
-> git mv <oldname> <newname>
```
Un archivo se puede eliminar del repositorio de dos maneras:

[a] solo del repositorio; el archivo permanece en el sistema de archivos local
```
-> git rm --cache <file>
```
[b] del repositorio y del sistema de archivos local

```
-> git rm <file>
```

## Clonar repositorio Git

Esta sección es solo de referencia. Deberíamos usar una **PAT** de Github para todas las operaciones del cliente Git.

[^4]: Aprender acerca de Github [Manage Personal Access Tokens](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)

{: .warning }
Se recomienda encarecidamente utilizar PAT de Github para las operaciones del cliente Git [^4].

"_Los tokens de acceso personal (PAT) son una alternativa al uso de contraseñas para la autenticación en GitHub cuando se utiliza la API de GitHub o la línea de comandos ._"

(a) Clonar usando el protocolo SSH

Si hubiera un proyecto nombrado `projectX` en el servidor git, entonces clónelo con el siguiente comando.
```
-> git clone git@git-server:/home/devuser/projectX
-> git clone git@192.168.65.139:bye.git
```
(b) Clonar usando el protocolo HTTPS
```
-> git clone https://github.com//mygitrepo.git
```

## ¿Cuál es la diferencia en un repositorio Git?

En algún momento querrás saber qué se confirmará antes de enviar cambios a un repositorio git. 

Éste es uno de los temas más discutidos en los foros públicos [^5].

[^5]: Leer el posteo de stackoverflow [how can i see what i about to push to git](http://stackoverflow.com/questions/3636914/how-can-i-see-what-i-am-about-to-push-with-git)

Descubra las diferencias entre su copia local y el repositorio remoto.

Primero, debe agregar los cambios a la rama local.
```
-> git add
```
Luego puedes comparar con la rama principal.

ME GUSTA ESTE: comparar copia local vs. servidor
```
-> git diff origin/main 
```
Comprobar la actividad del repositorioy
```
-> git log 
```
Verifique lo que se comprometerá.

```
-> git diff --staged
-> git diff --cached
```
¿Cómo puedo ver lo que estoy a punto de enviar con git?
```
-> git diff --stat origin/main HEAD
-> git diff origin/dev1
-> git push origin dev1 --dry-run
```
Usa [difftool](https://git-scm.com/docs/git-difftool) en MACOC, lo cual muestra una interfaz de usuario JAVA
```
-> git difftool origin/dev1
```

### Contenido del repositorio: ¿Qué cambió?

Comandos ejecutados en el cliente. 
Lista el contenido actual del repositorio.
```
-> git ls-files
-> git ls-tree -r main --name-only
-> git ls-tree -r main --full-name
-> git whatchanged
-> git ls-tree --full-tree -r HEAD
```

Muestra el contenido de un repositorio remoto. <br> 
No es un listado de archivos, sino el registro.
```
-> ssh git@git-servera "cd project1 && git log -n 10"
```

### Mostrar el contenido del archivo

Mostrar el contenido de un archivo en una rama

Sintaxis:
```
-> git show <branch>:file
```
Mostrar el contenido de un archivo en la rama dev1 y la rama principal.
```
-> git show dev1:cfile.txt
-> git show main:cfile.txt
```

### REGISTRO

El comando git log se utiliza para mostrar un historial cronológico de las confirmaciones en un repositorio Git. Proporciona detalles como hashes de las confirmaciones, información del autor, fechas y mensajes de confirmación, lo que permite a los usuarios revisar el historial de desarrollo del proyecto. El objetivo principal de git log es ayudar a los usuarios a comprender la secuencia de cambios, rastrear las modificaciones a lo largo del tiempo y analizar la evolución del código.

```
-> git log -p afile.txt	# show change history of a file
-> git log -p -2		# last two commits
-> git log			# show change history of all files

-> git log --oneline | nl -v0 | sed 's/^ \+/&HEAD~/'   # show commits like HEAD~X

-> git log --pretty=oneline
-> git log --pretty=format:"%h %s" --graph
```

## GIT Tirar

El comando `git pull` es para operaciones de `Fusionar - Sincronizar`. 
Garantiza que se obtengan los últimos cambios.

{: .important }
Se recomienda encarecidamente obtener los últimos cambios originales antes de empezar a trabajar en una base de código. De lo contrario, podrían surgir conflictos que generen confusión y sean difíciles de resolver..

Sincronizar el repositorio local con el repositorio remoto
```
-> git pull origin
```
Especifique el nombre de la rama a sincronizar
```
-> git pull origin <mybranch>
```
El comando `git pull` sin argumentos proporciona información útil sobre las ramas presentes en el repositorio remoto.
```
$ git pull
```

## Obtención de GIT

El comando `git pull` sobrescribe los archivos del repositorio local. <br>
Los cambios locales se pierden. <br>
Las confirmaciones locales que no se hayan enviado se perderán.
```
-> git fetch --all
-> git reset --hard origin/main
```
Luego, `git reset` restablece la rama principal a lo que acabas de obtener. La opción `--hard` cambia todos los archivos en tu árbol de trabajo para que coincidan con los archivos en `origin/main`.

El comando `git fetch` descarga todas las ramas del repositorio.
```
-> git fetch origin
```
La operacion de `fetch` descarga lo último del control remoto sin intentar fusionar o rebasar nada.

## Ramas de GIT

La gestión de ramas en GitHub es un aspecto clave del desarrollo colaborativo de software, ya que permite a los equipos trabajar en diferentes funciones, correcciones o experimentos simultáneamente sin conflictos. A continuación, se presentan algunos conceptos generales:

Puedes realizar las siguientes tareas:

- Crear, eliminar o renombrar una rama
- Crear y fusionar solicitudes de extracción
- Aplicar reglas de protección de ramas para evitar anulaciones accidentales
- Adoptar una estrategia de sucursales, como sucursales de larga o corta duración.

{: .highlight }
No es posible trabajar con el control de revisiones sin una comprensión básica de las ramas.

Una buena comprensión de las tareas de las ramas garantizará que los equipos colaboren eficazmente para mantener el código.

### Lista de ramas remotas

```
-> git branch
-> git branch -a
-> git branch -r
-> git branch -l
```

### Crear rama

Comprueba en qué rama estás.
```
-> git branch -v
```
Crear una nueva rama (mientras esté en la rama principal, por ejemplo)
```
-> git checkout -b new_branch
```
Enviar nueva rama al repositorio después de realizar cambios.
```
-> git add .
-> git commit -am “updates”
-> git push origin new_branch
```

### Extraer rama específica

Extrae la nueva rama a tu rama principal local.

[^6]: Ver en Stackoverflow este posteo acerca de [clonar rama especifica](http://stackoverflow.com/questions/1911109/clone-a-specific-git-branch)

 La rama nueva en el repositorio local tendrá el mismo nombre que en el repositorio remoto..
```
-> git pull origin new_branch
-> git checkout <lbranch>
```
O bien, baja la rama remota _rbranch_ y asígnele el nombre _lbranch_ localmente.
```
-> git fetch <remote> <rbranch>:<lbranch>
-> git checkout <lbranch>
```
Ejemplo: extraer la rama _dev1_ desde el origen
```
-> git fetch origin dev1:dev1test
-> git checkout dev1test
```

### Rama de clon único

Clonar una sola rama de un repositorio. <br>

> Se requiere Git **v1.7.10** para usar `--single-branch`.

Probé esto con SSH.
```
-> /usr/local/bin/git clone -b <my_branch> --single-branch git@git-servera:/home/git/project1
```
Este es otro ejemplo.
```
-> git clone -b <my_branch> --single-branch https://github.com/data/pets.git
```

### Eliminar rama

Eliminar una rama REMOTA (reemplace origin con el nombre que desee darle):
```
-> git push origin --delete <branch> #Git version 1.7.0 or newer 
-> git push origin :<branch>         #Git versions older than 1.7.0 
```
Eliminar una rama LOCAL:
```
-> git branch --delete <branch> 
-> git branch -d <branch>            #Shorter version 
-> git branch -D <branch>            #Force delete unmerged branches 
```
Eliminar una rama local de seguimiento en el repositorio git
```
-> git branch -a                     # get a branch listing 
-> git branch --delete --remotes <remote>/<branch> # use the output of the ‘git branch -a
-> git branch -dr <remote>/<branch>  #Shorter 
-> git fetch <remote> --prune        #Delete multiple obsolete tracking branches 
-> git fetch <remote> -p             #Shorter 
```
No es necesario realizar git commit ni nada después de eliminar una rama.


### Fusión de ramas

Secuencia para crear una nueva rama, agregar contenidos y fusionarla con la rama principal. <br>
(!) Los archivos en `newBranch` anularán los archivos en la rama principal.
```
-> git checkout -b newBranch	 # crea la rama
-> vi file1                      # edita un archivo
-> git add file1                 # añade el archivo
-> git commit -m”Edited file1’	 # confirma el archivo
-> git push origin newBranch	 # envía los cambios a la nueva rama
-> git checkout main             # cambia a la rama principal
-> git merge newBranch           # fusiona la nueva rama work con la rama principal
-> git branch -d newBranch       # elimina la nueva rama si ya no se necesita
```

### Establecer la Rama predeterminada Con `.gitconfig`

Establezca la rama predeterminada globalmente para el usuario actual.
```
-> git config --global init.defaultBranch main
```
Todos los repositorios recién creados tendrán la rama principal como predeterminada. 
El cambio se guarda en `~/.gitconfig`.
```
[init]
  defaultBranch = main
```
Además de la configuración global, cambie el repositorio local.
```
-> git branch -m main
```
Comprobar la rama predeterminada
```
-> git config --global init.defaultbranch
-> git symbolic-ref --short HEAD
-> grep defaultBranch ~/.gitconfig
```
Para cambiar otra rama predeterminada, en los comandos anteriores, simplemente cambie el nombre de la rama a cualquier otro. Elimine la configuración de la rama predeterminada. No habrá ninguna después de esto.
```
-> git config --global --unset init.defaultBranch
```

Este en un ejemplo completo de `.gitconfig`.
```
# Este es mi configuración para el cliente git
[user]
	name = DevEsp
	email = devesp@example.com
	username = devuser
[credential]
	helper = store
[filter "lfs"]
	clean = git-lfs clean -- %f
	smudge = git-lfs smudge --skip -- %f
	process = git-lfs filter-process --skip
	required = true
[core]
	editor = vim
	excludesfile = /Users/devuser/.gitignore_global
[merge]
	tool = vimdiff
[color]
	ui = true
[alias]
  co = checkout
  ci = commit
  st = status
  br = branch
  hist = log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short
  type = cat-file -t
  dump = cat-file -p
  glist = ls-tree -r master --name-only
[pull]
	rebase = false
[init]
	defaultBranch = main
```

## Acceso al repositorio SSH de GIT

Es posible acceder al repositorio git con clave SSH

Cualquiera que necesite acceder a los proyectos en el servidor git debe copiar su clave SSH pública al authorized_keysarchivo de usuario git en el servidor git..
```
[git@git-server]$ cat /home/devuser/.ssh/authorized_keys
```
Pruebe el acceso para asegurarse de que funciona..
```
[devuser@git-client] ssh git@git-server echo test
```

## Referencias

[Return to main page]({{site.baseurl}}/).