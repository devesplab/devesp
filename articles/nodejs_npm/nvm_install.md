---
layout: default
title: Usar NVM para Instalar NodeJS
permalink: /nvn_nodejs/
parent: Articulos
has_children: true
has_toc: false
nav_order: 1
---

# Propósito de NVM

Node Version Manager (NVM) es una herramienta que se usa para manejar varias versiones de Node.js en un mismo sistema. La idea es de poder instalar varias versiones y poder cambiar de una a otra con facilidad. Podemos instaler the version mas actualizada o una version en particular por razones determinadas por el desarrollador.

Uno de los aspectos más importantes para hacer esto el poder manejar dependencias de una version específica sin afectar otras versiones presentes en el sistema.

{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

## Intallar NVM

Esta operacion puede hacerse como un usuario regular since necesitar permisos elevados de SUDO or ROOT.

Podemos usar BASH para usar el programa de instalación directamente en linea.

{: .note }
Ver las [Instrucciones de instalacion de NodeJS](https://nodejs.org/en/download/package-manager/all#installing-nodejs-via-package-managers) en linea.
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

Notese que el comando de instalación arriba agrega este bloque al archivo `~/.bashrc` del usuario que ejecutó la operación.
```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

Tambien es de notar que el comando nvm no se ve en la terminal cuando lo buscamos con el comando `which`.
```bash
Sat 2025Feb15 19:14:38 PST
orion@devesp
~
hist:412 -> which nvm
```

## Instalar NodeJS

Veamos ahora e instalemos la version ` 23.8.0` the Nodejs.
```bash
Sat 2025Feb15 19:14:52 PST
orion@devesp
~
hist:412 -> nvm install 23.8.0
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
Sat 2025Feb15 19:37:13 PST
orion@devesp
~
hist:424 -> nvm ls
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

## Referencias 

- [Documentos Officials de NVM](https://github.com/nvm-sh/nvm)
- [Lista de distribuciones de NodeJS](https://nodejs.org/dist/)
- [Instrucciones de instalacion de NodeJS](https://nodejs.org/en/download/package-manager/all#installing-nodejs-via-package-managers)

[Return to main page]({{site.baseurl}}/).
