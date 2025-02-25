---
layout: default
title: Ruby y rbenv
permalink: /ruby_rbev/
parent: Articulos
has_children: false
has_toc: false
nav_order: 2
---

{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

# Propósito de RBENV

rbenv es una herramienta para gestionar diferentes versiones de Ruby en un único sistema. Su objetivo principal es permitir a los desarrolladores cambiar fácilmente entre múltiples versiones de Ruby y garantizar que se utilice la versión correcta para un proyecto específico.

## Instalar RBENV

Este documento explica lo siguiente:
- Obtener rbenv
- Instalar rbenv
- Usar rbenv para instalar una version de ruby
-Usar rbenv para establecer la version de ruby en forma local o global 

Para el proceso entero detallado aquí usamos Ubuntu 22.04.
Usamos el commando `hostnamectl` para mostrar los detalles del sistema donde estamos trabajando.

```bash
Fri 2025Feb21 05:21:47 UTC
devuser@ubuntu2204-1-devesp
~
hist:233 -> hostnamectl
 Static hostname: ubuntu2204-1-devesp
       Icon name: computer-container
         Chassis: container
      Machine ID: 68646fed809a4c54ab836048028d6896
         Boot ID: 34f42aa3a9b34e4bae91c3b50b3864a5
  Virtualization: docker
Operating System: Ubuntu 22.04.4 LTS
          Kernel: Linux 6.3.13-linuxkit
    Architecture: x86-64
```

## Obtener RBENV

Hagamos lo siguiente como pre-requisito
- Obtener la distribucion de rbenv
- Inicializar el ambiente de rbenv

Para empezar, estamos en la carpeta de inicio del usuario.

{: .highlight }
Durante este proceso, todos los comandos son ejecutados estando en el directorio de inicio del usuario.

```bash
Fri 2025Feb21 04:15:09 UTC
devuser@ubuntu2204-1-devesp
/home/devuser
hist:188 -> pwd
/home/devuser
```

Clonemos el repositorio de git.
El argumento `~/.rbenv` indica el destino local donde deseamos clonar el repositorio.
```bash
Fri 2025Feb21 04:15:09 UTC
devuser@ubuntu2204-1-devesp
/home/devuser
hist:187 -> git clone https://github.com/rbenv/rbenv.git ~/.rbenv
Cloning into '/home/devuser/.rbenv'...
remote: Enumerating objects: 3379, done.
remote: Counting objects: 100% (280/280), done.
remote: Compressing objects: 100% (114/114), done.
remote: Total 3379 (delta 224), reused 166 (delta 166), pack-reused 3099 (from 3)
Receiving objects: 100% (3379/3379), 714.84 KiB | 1.80 MiB/s, done.
Resolving deltas: 100% (2082/2082), done.
```

El comando anterior crea la carpeta `/home/devuser/.rbenv`.

Enseguida, initicializemos rbenv.
```bash
devuser@ubuntu2204-1-devesp
/home/devuser
hist:189 -> ~/.rbenv/bin/rbenv init
writing ~/.bash_profile: now configured for rbenv.

devuser@ubuntu2204-1-devesp
/home/devuser
hist:191 -> source ~/.bash_profile
```

Mostremos el contenido de la carpeta bin.

```bash
devuser@ubuntu2204-1-devesp
~
hist:193 -> ls -l ~/.rbenv/bin/
total 0
lrwxrwxrwx 1 devuser devuser 16 Feb 21 04:15 rbenv -> ../libexec/rbenv*
```

## Instalar ruby-build

Antes de que podemos usar rbenv, necesitamos instalar el paquete **ruby-build**.

{: .note }
El comando de instalacion de rbenv mencionado en el paso anterior no provee el binario de `rbenv`. Este es proveido por el plugin **ruby-build**.<br>
El plugin de ruby-build es una utilidad que simplifica la instalacion de cualquier version de ruby en sistemas de variantes de Unix.

Clonemos el repositorio de git y sigamos los pasos para abilitar el paquete.

```bash
devuser@ubuntu2204-1-devesp
~
hist:194 -> git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
Cloning into '/home/devuser/.rbenv/plugins/ruby-build'...
remote: Enumerating objects: 16810, done.
remote: Counting objects: 100% (4554/4554), done.
remote: Compressing objects: 100% (412/412), done.
remote: Total 16810 (delta 4422), reused 4177 (delta 4137), pack-reused 12256 (from 3)
Receiving objects: 100% (16810/16810), 3.33 MiB | 1.54 MiB/s, done.
Resolving deltas: 100% (11927/11927), done.

devuser@ubuntu2204-1-devesp
~
hist:195 -> git -C "$(rbenv root)"/plugins/ruby-build pull
Already up to date.

devuser@ubuntu2204-1-devesp
~
hist:197 ->  rbenv root
/home/devuser/.rbenv
```

Verifiquemos que rbenv esta en el paso del usuario.
```
devuser@ubuntu2204-1-devesp
~
hist:192 ->  which rbenv
/home/devuser/.rbenv/bin/rbenv
```

Verifiquemos la version de rbenv.
```
devuser@ubuntu2204-1-devesp
~
hist:192 -> rbenv
rbenv 1.3.2
Usage: rbenv <command> [<args>...]

Commands to manage available Ruby versions:
   versions    List installed Ruby versions
   rehash      Regenerate rbenv shims

Commands to view or change the current Ruby version:
   version     Show the current Ruby version and its origin
   local       Set or show the local application-specific Ruby version
   global      Set or show the global Ruby version
   shell       Set or show the shell-specific Ruby version

See `rbenv help <command>' for information on a specific command.
For full documentation, see: https://github.com/rbenv/rbenv#readme
```

## Instalar una version de ruby (primera instalacion)

El ambiente esta ahora listo y podemos proceder a instalar nuestra primera version de ruby.

Para el propósito de este ejemplo, instalemos ruby  version `3.3.5`

La sintaxis simplemente requiere que pasemos la version a instalar de esta manera:

```bash
rbenv install <version>
```

{: .important }
Una instalacion de ruby toma alrededor de **5 minutos**!<br>
Puede tomar un poco mas de tiempo en sistemas con bajos recursos.<br>
Este proceso instala algunas dependencias destras de escena.

```
devuser@ubuntu2204-1-devesp
~
hist:215 -> rbenv install 3.3.5
==> Downloading openssl-3.0.15.tar.gz...
-> curl -q -fL -o openssl-3.0.15.tar.gz https://dqw8nmjcqpjn7.cloudfront.net/23c666d0edf20f14249b3d8f0368acaee9ab585b09e1de82107c66e1f3ec9533
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 14.6M  100 14.6M    0     0  6177k      0  0:00:02  0:00:02 --:--:-- 6176k
==> Installing openssl-3.0.15...
-> ./config "--prefix=$HOME/.rbenv/versions/3.3.5/openssl" "--openssldir=$HOME/.rbenv/versions/3.3.5/openssl/ssl" --libdir=lib zlib-dynamic no-ssl3 shared "-Wl,-rpath,$HOME/.rbenv/versions/3.3.5/openssl/lib"
-> make -j 6
-> make install_sw install_ssldirs
==> Installed openssl-3.0.15 to /home/devuser/.rbenv/versions/3.3.5
==> Downloading ruby-3.3.5.tar.gz...
-> curl -q -fL -o ruby-3.3.5.tar.gz https://cache.ruby-lang.org/pub/ruby/3.3/ruby-3.3.5.tar.gz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 21.1M  100 21.1M    0     0  6935k      0  0:00:03  0:00:03 --:--:-- 6937k
==> Installing ruby-3.3.5...
-> ./configure "--prefix=$HOME/.rbenv/versions/3.3.5" "--with-openssl-dir=$HOME/.rbenv/versions/3.3.5/openssl" --enable-shared --with-ext=openssl,psych,+
-> make -j 6
-> make install
==> Installed ruby-3.3.5 to /home/devuser/.rbenv/versions/3.3.5

NOTE: to activate this Ruby version as the new default, run: `rbenv global 3.3.5`
```

Notemos que el paso de las instalacion es `/home/devuser/.rbenv/versions/3.3.5` 
Como lo recomienda el comando anterior, establescamos la version global.

```
devuser@ubuntu2204-1-devesp
~
hist:215 -> rbenv global 3.3.5
```

Esta acción crea el archivo `~/.rbenv/version` el cual contiene la version activa.

```
devuser@ubuntu2204-1-devesp
~
hist:216 -> cat ~/.rbenv/version
3.3.5
```

El comando `rbenv global <version>` establece la version de ruby. En cualquier momento, podemos anular la version global con el comando `rbenv local`, o al establecer la variable de ambient **RBENV_VERSION**. Es de notar que `<version>` deberia ser una cadena que coincida con una version conocida por rbenv. 

Usemos el comando `rbenv versions` para listar las versiones de ruby disponibles al momento.

```
devuser@ubuntu2204-1-devesp
~
hist:218 -> rbenv versions
* 3.3.5 (set by /home/devuser/.rbenv/version)

devuser@ubuntu2204-1-devesp
~
hist:219 -> ruby --version
ruby 3.3.5 (2024-09-03 revision ef084cc8f4) [x86_64-linux]

devuser@ubuntu2204-1-devesp
~
hist:220 -> which ruby
/home/devuser/.rbenv/shims/ruby
```

La instalacion de ruby agrega el comando `gem`, el cual se usa para instalar paquetes adicionales de ruby conocidos come **gemas** que extienden la funcionalidad de ruby.

```
devuser@ubuntu2204-1-devesp
~
hist:221 -> which gem
/home/devuser/.rbenv/shims/gem
```

La instalacion entera de rbenv va en la carpeta `~/.rbenv/shims/`.<br>
Notese la presencia de los binarios `bundler`, `gem`, `rake` y `ruby`.

```bash
devuser@ubuntu2204-1-devesp
~
hist:222 -> ls -l ~/.rbenv/shims/
total 104
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 bundle*
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 bundle.lock*
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 bundler*
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 bundler.lock*
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 erb*
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 erb.lock*
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 gem*
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 irb*
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 irb.lock*
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 racc*
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 racc.lock*
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 rake*
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 rake.lock*
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 rbs*
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 rbs.lock*
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 rdbg*
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 rdbg.lock*
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 rdoc*
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 rdoc.lock*
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 ri*
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 ri.lock*
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 ruby*
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 syntax_suggest*
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 syntax_suggest.lock*
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 typeprof*
-rwxrwxr-x 1 devuser devuser 397 Feb 21 04:53 typeprof.lock*
```

El comando `gem` es parte del sistema de manejo de paquetes de Ruby. Se usa para mostrar información acerca del ambiente de ruby. Esto es útil cuando deseamos saber la versión activa de ruby, la localidad donde se instalarían gemas y otras partes importantes.

```bash
devuser@ubuntu2204-1-devesp
~
hist:223 -> gem env
RubyGems Environment:
  - RUBYGEMS VERSION: 3.5.16
  - RUBY VERSION: 3.3.5 (2024-09-03 patchlevel 100) [x86_64-linux]
  - INSTALLATION DIRECTORY: /home/devuser/.rbenv/versions/3.3.5/lib/ruby/gems/3.3.0
  - USER INSTALLATION DIRECTORY: /home/devuser/.local/share/gem/ruby/3.3.0
  - RUBY EXECUTABLE: /home/devuser/.rbenv/versions/3.3.5/bin/ruby
  - GIT EXECUTABLE: /usr/bin/git
  - EXECUTABLE DIRECTORY: /home/devuser/.rbenv/versions/3.3.5/bin
  - SPEC CACHE DIRECTORY: /home/devuser/.cache/gem/specs
  - SYSTEM CONFIGURATION DIRECTORY: /home/devuser/.rbenv/versions/3.3.5/etc
  - RUBYGEMS PLATFORMS:
     - ruby
     - x86_64-linux
  - GEM PATHS:
     - /home/devuser/.rbenv/versions/3.3.5/lib/ruby/gems/3.3.0
     - /home/devuser/.local/share/gem/ruby/3.3.0
  - GEM CONFIGURATION:
     - :update_sources => true
     - :verbose => true
     - :backtrace => true
     - :bulk_threshold => 1000
  - REMOTE SOURCES:
     - https://rubygems.org/
  - SHELL PATH:
     - /home/devuser/.rbenv/versions/3.3.5/bin
     - /home/devuser/.rbenv/libexec
     - /home/devuser/.rbenv/plugins/ruby-build/bin
     - /home/devuser/.rbenv/shims
     - /home/devuser/.rbenv/bin
     - /home/devuser/bin
     - /home/devuser/Library/Python/3.9/bin
     - /home/devuser/bin
     - /home/devuser/Library/Python/3.9/bin
     - /usr/local/sbin
     - /usr/local/bin
     - /usr/sbin
     - /usr/bin
     - /sbin
     - /bin
     - /usr/games
     - /usr/local/games
     - /snap/bin
```

Al instalar nuevas gemas, van en el paso de la version activa de ruby.
En este ejemplo el paso de la version activa es ` $HOME/.rbenv/versions/3.3.5`.

```bash
devuser@ubuntu2204-1-devesp
~
hist:227 -> ls -l $HOME/.rbenv/versions/3.3.5/lib/ruby/gems/3.3.0/gems/
total 344
drwxr-xr-x  2 devuser devuser 4096 Feb 21 04:53 abbrev-0.1.2/
drwxr-xr-x  2 devuser devuser 4096 Feb 21 04:53 base64-0.2.0/
drwxr-xr-x  2 devuser devuser 4096 Feb 21 04:53 benchmark-0.3.0/
drwxr-xr-x  2 devuser devuser 4096 Feb 21 04:53 bigdecimal-3.1.5/
drwxr-xr-x  3 devuser devuser 4096 Feb 21 04:53 bundler-2.5.16/
drwxr-xr-x  2 devuser devuser 4096 Feb 21 04:53 cgi-0.4.1/
drwxr-xr-x  2 devuser devuser 4096 Feb 21 04:53 csv-3.2.8/
drwxr-xr-x  2 devuser devuser 4096 Feb 21 04:53 date-3.3.4/
drwxr-xr-x  6 devuser devuser 4096 Feb 21 04:53 debug-1.9.1/
drwxr-xr-x  2 devuser devuser 4096 Feb 21 04:53 delegate-0.3.1/
drwxr-xr-x  2 devuser devuser 4096 Feb 21 04:53 did_you_mean-1.6.3/
drwxr-xr-x  2 devuser devuser 4096 Feb 21 04:53 digest-3.1.1/
(…snip…)
```

Podemos listar las gemas que son instaladas por defecto. La lista abajo es parcial.<br>
Al inspeccionar la lista podemos encontrar si tenemos las dependecias requeridas para desarrollar un proyecto

```
devuser@ubuntu2204-1-devesp
~
hist:228 -> gem list | head
abbrev (default: 0.1.2)
base64 (default: 0.2.0)
benchmark (default: 0.3.0)
bigdecimal (default: 3.1.5)
bundler (default: 2.5.16)
cgi (default: 0.4.1)
csv (default: 3.2.8)
date (default: 3.3.4)
debug (1.9.1)
delegate (default: 0.3.1)
(…snip…)
```

## Instalar versiones adicionales de ruby 

You can now proceed to install new ruby versions.
You can override existing settings that have set a previous version.

Ahora podemos proceder a instalar versiones de ruby adicionales.
Podemos anular ajustes existentes de versiones que han sido instaladas anteriormente.
La [lista entera de lanzamientos](https://www.ruby-lang.org/en/downloads/releases/) de Ruby esta disponible en linea.

```bash
-> rbenv install 3.4.2

-> rbenv install 2.7.2
```

Consultemos las versiones instaladas corrientemente.

```bash
devuser@ubuntu2204-1-devesp
~
hist:230 -> ls -l $HOME/.rbenv/versions
total 12
drwxrwxr-x 7 devuser devuser 4096 Feb 21 05:13 2.7.2/
drwxrwxr-x 7 devuser devuser 4096 Feb 21 04:53 3.3.5/
drwxrwxr-x 7 devuser devuser 4096 Feb 21 05:06 3.4.2/
```

En esta salida, la version señalada con un asterisco '*' indica que es la version activa.

```
devuser@ubuntu2204-1-devesp
~
hist:231 -> rbenv versions
  2.7.2
* 3.3.5 (set by /home/devuser/.rbenv/version)
  3.4.2
```

Ahora simplemente podemos editar el achivo `~/.rbenv/version` con la version que queramos establecer como GLOBAL y ruby lo recogerá.

A seguir, editemos el archivo escribiendo `3.4.2` para hacer esa la version global.

```
devuser@ubuntu2204-1-devesp
~
hist:231 ->  vi  ~/.rbenv/version
```

Verificar la nueva version global.

```
devuser@ubuntu2204-1-devesp
~
hist:231 -> rbenv versions
  2.7.2
  3.3.5
* 3.4.2 (set by /home/devuser/.rbenv/version)

devuser@ubuntu2204-1-devesp
~
hist:231 -> ruby --version
ruby 3.4.2 (2025-02-15 revision d2930f8e7a) +PRISM [x86_64-linux]
```

Ahora podemos anular la version GOGAL al establecer la version LOCAL.

{: .highlight }
La versión global de Ruby es la versión de Ruby que está configurada para usarse en todo el sistema para todos los proyectos, a menos que la anule una versión local. La versión local de Ruby es específica de un proyecto en particular y se puede configurar para anular la versión global. Esto le permite mantener dependencias y entornos específicos del proyecto sin afectar otros proyectos.

```
devuser@ubuntu2204-1-devesp
~
hist:231 -> rbenv local 2.7.2

devuser@ubuntu2204-1-devesp
~
hist:232 -> rbenv versions
* 2.7.2 (set by /home/devuser/.ruby-version)
  3.3.5
  3.4.2
```

EL comando gem env muestra la nueva version activa y las carpetas relevantes al ambiente.

```
devuser@ubuntu2204-1-devesp
~
hist:232 -> gem env
RubyGems Environment:
  - RUBYGEMS VERSION: 3.1.4
  - RUBY VERSION: 2.7.2 (2020-10-01 patchlevel 137) [x86_64-linux]
  - INSTALLATION DIRECTORY: /home/devuser/.rbenv/versions/2.7.2/lib/ruby/gems/2.7.0
  - USER INSTALLATION DIRECTORY: /home/devuser/.gem/ruby/2.7.0
  - RUBY EXECUTABLE: /home/devuser/.rbenv/versions/2.7.2/bin/ruby
  - GIT EXECUTABLE: /usr/bin/git
  - EXECUTABLE DIRECTORY: /home/devuser/.rbenv/versions/2.7.2/bin
  - SPEC CACHE DIRECTORY: /home/devuser/.gem/specs
  - SYSTEM CONFIGURATION DIRECTORY: /home/devuser/.rbenv/versions/2.7.2/etc
  - RUBYGEMS PLATFORMS:
    - ruby
    - x86_64-linux
  - GEM PATHS:
     - /home/devuser/.rbenv/versions/2.7.2/lib/ruby/gems/2.7.0
     - /home/devuser/.gem/ruby/2.7.0
  - GEM CONFIGURATION:
     - :update_sources => true
     - :verbose => true
     - :backtrace => false
     - :bulk_threshold => 1000
  - REMOTE SOURCES:
     - https://rubygems.org/
  - SHELL PATH:
     - /home/devuser/.rbenv/versions/2.7.2/bin
     - /home/devuser/.rbenv/libexec
     - /home/devuser/.rbenv/plugins/ruby-build/bin
     - /home/devuser/.rbenv/shims
     - /home/devuser/.rbenv/bin
     - /home/devuser/bin
     - /home/devuser/Library/Python/3.9/bin
     - /home/devuser/bin
     - /home/devuser/Library/Python/3.9/bin
     - /usr/local/sbin
     - /usr/local/bin
     - /usr/sbin
     - /usr/bin
     - /sbin
     - /bin
     - /usr/games
     - /usr/local/games
     - /snap/bin
```

A partir de este momento, tenemos un ambiente de ruby listo para usar!

## Instalando Gemas

Pongamos nuestrao nuevo ambiente de ruby a la prueba.

Deseamos instalar una gema llamada abbrev.

Usemos el comando gem para saber si la gema abbrev existe.

```
-> gem search abbrev

*** REMOTE GEMS ***

abbrev (0.1.2)
abbreviato (3.1.4)
cisco_abbrev (0.1.0)
ransack_abbreviator (0.0.8)
redcarpet-abbreviations (0.1.1)
rubysl-abbrev (2.0.4)
```

La salida muestra que tenemos disponible la gema abbrev (0.1.2).
Instalemos la gema.

```
-> gem install abbrev
Fetching abbrev-0.1.2.gem
Successfully installed abbrev-0.1.2
Parsing documentation for abbrev-0.1.2
Installing ri documentation for abbrev-0.1.2
Done installing documentation for abbrev after 0 seconds
1 gem installed
```

Verifiquemos que la gema fue instalada.

```
-> gem list abbrev

*** LOCAL GEMS ***

abbrev (0.1.2)
```

De donde viene esa gema? Viene del recurso remoto que vimos al correr el comando gem env. El recurso remote se denota con la variable de ambiente REMOTE SOURCE dentro del ambiente de ruby.

```
-> gem env
RubyGems Environment:
…
  - REMOTE SOURCES:
     - https://rubygems.org/
…
```

Discutiremos mas detalles acerca de ruby y gemas on otros articulos.
