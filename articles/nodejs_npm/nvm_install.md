---
layout: default
title: Usar NVM para Instalar NodeJS
permalink: /nvn_nodejs/
parent: Articulos
has_children: true
has_toc: false
nav_order: 1
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

# Propósito de NVM

Node Version Manager (NVM) es una herramienta que se usa para manejar varias versiones de Node.js en un mismo sistema. La idea es de poder instalar varias versiones y poder cambiar de una a otra con facilidad. Podemos instalar la version mas actualizada o una version en particular por razones que el desarrollador decida.

Uno de los aspectos más importantes para usar NVM es la abilidad de manejar dependencias de una version específica sin afectar otras versiones presentes en el sistema.

## Intallar NVM

Las operaciones de NVM pueden hacerse como un usuario regular sin necesitar permisos elevados de SUDO o ROOT.

Podemos usar BASH para usar el programa de instalación directamente en linea.

{: .note }
Ver las [Instrucciones de instalacion de NodeJS](https://nodejs.org/en/download/package-manager/all#installing-nodejs-via-package-managers) en linea.<br>
Si el comando `curl` no esta disponible ver [Manejar Paquetes en Linux](https://docs.devesp.com/manejar-paquetes-linux/).
```bash
Sat 2025Feb15 19:12:53 PST
orion@devesp
~
hist:408 -> curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 15916  100 15916    0     0   187k      0 --:--:-- --:--:-- --:--:--  187k
=> Downloading nvm from git to '/Users/orion/.nvm'
=> Cloning into '/Users/orion/.nvm'...
remote: Enumerating objects: 381, done.
remote: Counting objects: 100% (381/381), done.
remote: Compressing objects: 100% (324/324), done.
remote: Total 381 (delta 43), reused 176 (delta 29), pack-reused 0 (from 0)
Receiving objects: 100% (381/381), 383.82 KiB | 1.98 MiB/s, done.
Resolving deltas: 100% (43/43), done.
* (HEAD detached at FETCH_HEAD)
  master
=> Compressing and cleaning up git repository

=> Appending nvm source string to /Users/orion/.bashrc
=> Appending bash_completion source string to /Users/orion/.bashrc
env: node: No such file or directory
=> Close and reopen your terminal to start using nvm or run the following to use it now:

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

Al bajar la distribución se crea el directorio `~/.nvm` en tu computadora.
```bash
-> ls -l ~/.nvm
total 536
-rw-r--r--   1 orion  staff    7560 Feb 15 19:13 CODE_OF_CONDUCT.md
-rw-r--r--   1 orion  staff    4743 Feb 15 19:13 CONTRIBUTING.md
-rw-r--r--   1 orion  staff    3691 Feb 15 19:13 Dockerfile
-rw-r--r--   1 orion  staff     467 Feb 15 19:13 GOVERNANCE.md
-rw-r--r--   1 orion  staff    1113 Feb 15 19:13 LICENSE.md
-rw-r--r--   1 orion  staff    5361 Feb 15 19:13 Makefile
-rw-r--r--   1 orion  staff    2935 Feb 15 19:13 PROJECT_CHARTER.md
-rw-r--r--   1 orion  staff   46120 Feb 15 19:13 README.md
-rw-r--r--   1 orion  staff     882 Feb 15 19:13 ROADMAP.md
drwxr-xr-x   4 orion  staff     128 Feb 15 19:15 alias/
-rw-r--r--   1 orion  staff    2299 Feb 15 19:13 bash_completion
-rwxr-xr-x   1 orion  staff   15916 Feb 15 19:13 install.sh*
-rwxr-xr-x   1 orion  staff     371 Feb 15 19:13 nvm-exec*
-rw-r--r--   1 orion  staff  143144 Feb 15 19:13 nvm.sh
-rw-r--r--   1 orion  staff    2372 Feb 15 19:13 package.json
-rwxr-xr-x   1 orion  staff    1235 Feb 15 19:13 rename_test.sh*
drwxr-xr-x  11 orion  staff     352 Feb 15 19:13 test/
-rwxr-xr-x   1 orion  staff    2478 Feb 15 19:13 update_test_mocks.sh*
drwxr-xr-x   3 orion  staff      96 Feb 15 19:15 versions/
```

Para hacer disponible el comando `nvm` en el ambiente del usuario que ejecutó la operación, la instalación arriba agrega este bloque al archivo `~/.bashrc`.
```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

Tal como la salida del comando `curl` lo sugirió, salgamos y re-entremos a la terminal, o simplemente releamos el archivo de inicio:
```sh
-> source ~/.bashrc
```
Si hacer lo anterior, el comando `nvm` no estará disponible para uso.

Tambien es de notar que el comando nvm no se ve en la terminal cuando lo buscamos con el comando `which`.
```bash
-> which nvm
```

## Instalar NodeJS

Veamos ahora e instalemos la version `23.8.0` the Nodejs.
```bash
-> nvm install 23.8.0
Downloading and installing node v23.8.0...
Downloading https://nodejs.org/dist/v23.8.0/node-v23.8.0-darwin-x64.tar.xz...
####################################################################################### 100.0%
Computing checksum with sha256sum
Checksums matched!
Now using node v23.8.0 (npm v10.9.2)
Creating default alias: default -> 23.8.0 (-> v23.8.0)
```
Tomar nota del mensaje indicando "Now using node..." or "Ahora usando node...".

Entremos el comando para seleccionar la version a usar.
```bash
-> nvm use 23.8.0
Now using node v23.8.0 (npm v10.9.2)
```
Desde este momento, La version activa de NodeJS es ` 23.8.0`.

## Verificar la version de node y npm

{: .highlight }
NodeJS provee el comando `node` y `npm`.

Verificar la version de NODE.
```bash
-> node -v
v23.8.0
```

Verificar la version the NPM.
```bash
-> npm --version
10.9.2
```

Opcionalmente, usa este comando para agregar el binario `node` a `/usr/local/bin/`.
```
sudo install -m 755  $(which node) /usr/local/bin/
```

## Instalar version adicional de nodejs

Ahora, instalemos otra version de nodejs.
```bash
-> nvm install 18
Downloading and installing node v18.20.6...
Downloading https://nodejs.org/dist/v18.20.6/node-v18.20.6-darwin-x64.tar.xz...
############################################################################### 100.0%
Computing checksum with sha256sum
Checksums matched!
Now using node v18.20.6 (npm v10.8.2)
```

Usemos `nvm` para seleccionar la nueva version que hemos instalado.
```bash
-> nvm use 18
Now using node v18.20.6 (npm v10.8.2)
```

## Listar versiones de nodejs

Usemos el comando `nvm` para ver todas las instalaciones de nodejs en nuestro sistema.
La versión activa esta marcada con `->`.
```bash
-> nvm ls
       v18.20.6
->      v23.8.0
default -> 23.8.0 (-> v23.8.0)
iojs -> N/A (default)
unstable -> N/A (default)
node -> stable (-> v23.8.0) (default)
stable -> 23.8 (-> v23.8.0) (default)
lts/* -> lts/jod (-> N/A)
lts/argon -> v4.9.1 (-> N/A)
lts/boron -> v6.17.1 (-> N/A)
lts/carbon -> v8.17.0 (-> N/A)
lts/dubnium -> v10.24.1 (-> N/A)
lts/erbium -> v12.22.12 (-> N/A)
lts/fermium -> v14.21.3 (-> N/A)
lts/gallium -> v16.20.2 (-> N/A)
lts/hydrogen -> v18.20.6
lts/iron -> v20.18.3 (-> N/A)
lts/jod -> v22.14.0 (-> N/A)
```

## Actualizando NPM

En algun tiempo, NPM indicara que debe actualizarse. Esto se nota cuando ejecutamos comandos y en la salida nos recomienda que actualizemos NPM

Per ejemplo, aqui tenemos version `10.x`. 
```
-> npm -v
10.9.2
```

En este ejemplo hacemos un simple lista de modulos que contienen "file" en el nombre.
```
-> npm search file
file
Higher level path and file manipulation functions.
Version 0.2.2 published 2014-02-21 by aconbere
Maintainers: aconbere
https://npm.im/file
...
metro-file-map
[Experimental] - 🚇 File crawling, watching and mapping for Metro
Version 0.84.4 published 2026-04-30 by GitHub Actions
Maintainers: fb metro-bot
https://npm.im/metro-file-map
...
npm notice
npm notice New major version of npm available! 10.9.2 -> 11.14.1
npm notice Changelog: https://github.com/npm/cli/releases/tag/v11.14.1
npm notice To update run: npm install -g npm@11.14.1
npm notice
```
En la ultima seccion nos indica lo que debemos hacer para actualizar NPM. 
```
devuser@devesp
~/workdir
hist:192 -> npm install -g npm@11.14.1

removed 84 packages, and changed 105 packages in 6s

15 packages are looking for funding
  run `npm fund` for details
```

{: .note }
No se requiere permisos elevados de SUDO para instalar modulos de NPM como se describe en este documento puesto que los modulos se instalan en el directorio hogar del usuario.

Confirmamos que NPM fue actuzlizado a version `11.x`.
```
Fri 2026May15 02:43:19 UTC
devuser@devesp
~/workdir
hist:193 -> npm -v
11.14.1
```

## Ubicacion de Modulos the NPM

Los comandos a seguir muestran el directorio base donde NPM reside en el hogar del usuario. 

{: .warning }
> El directorio base de NPM es /home/[id-de-usuario]/.nvm/versions/node/[version].
> Modulos instalados solo para el usuario no son disponibles globalmente en el sistema. Discutiremos este tópico en otro articulo.

Los comandos siguientes son ejecutados con tu cuenta de usuario en tu directory hogar `$HOME`.

```
devuser@devesp
~
hist:202 -> npm prefix -g
/home/devuser/.nvm/versions/node/v23.8.0

-> npm root -g
/home/devuser/.nvm/versions/node/v23.8.0/lib/node_modules
```
Lo anterior indica que si instalamos un modulo, digamos `cowsay`:
```
-> npm install -g cowsay
```
entonces el modulo se instala en la raiz de NMP.
```
-> ls -ld ~/.nvm/versions/node/v23.8.0/lib/node_modules/cowsay
drwxrwxr-x 6 devuser devuser 4096 May 15 02:58 /home/devuser/.nvm/versions/node/v23.8.0/lib/node_modules/cowsay/
```
Y luego, probamos a imprimir un mensaje para asegurarnos que el module trabaja.
```
-> npx cowsay "hola devesp!"
 ______________
< hola devesp! >
 --------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```


## Glossario De Commandos

`npm`
: utilidad para manejar paquetes de Javascript.

`npx` 
: correr un comando de un modulo de npm local o remoto.

## Referencias Utiles

- [Documentos Officials de NVM](https://github.com/nvm-sh/nvm)
- [Lista de distribuciones de NodeJS](https://nodejs.org/dist/)
- [Instrucciones de instalacion de NodeJS](https://nodejs.org/en/download/package-manager/all#installing-nodejs-via-package-managers)
- [Acerca de NPM](https://docs.npmjs.com/about-npm)

Paginas Manuales

- [npm](https://docs.npmjs.com/cli/v11/commands/npm)
- [npx](https://docs.npmjs.com/cli/v11/commands/npx?utm_source=chatgpt.com)

[Return to main page]({{site.baseurl}}/).
