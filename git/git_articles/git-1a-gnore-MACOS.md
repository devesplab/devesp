---
layout: default
title: Ignorando Archivos en MacOS
permalink: /git-ignore-macos/
parent: Artículos De Git  
has_children: false
has_toc: false
nav_order: 1
---

## Ignorar archivos de Git en MACOS

{: .no_toc }

<details open markdown="block">
  <summary>
    Tabla de contenidos
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

---

Ignorar cierto tipo de recursos en una fuente de código puede ser tan importante como los recursos que deseamos rastrear. Ester articulo se concentra como manejamos esto en MACOS X.

## Ignorando Archivos .DStore en Git (MacOS)

{: .note }
Este sección es especifico a MacOS, pero el concepto de ignorar archivos es aplicable a cualquier sistema operativo y a cualquier tipo de archivo que use Git.

Es una buena práctica ignorar ciertos archivos en tu repositorio de Git que no son necesarios para tu proyecto. Un archivo común que debe ser ignorado es `.DS_Store`. Estos archivos son creados por el Finder de macOS para guardar atributos personalizados de una carpeta, como posiciones de iconos y opciones de visualización. No son necesarios para tu proyecto y pueden saturar tu repositorio Git. Para evitar que estos archivos sean rastreados por Git, puedes agregarlos a tu archivo `.gitignore`.

> En **MACOS X** ignoremos las carpetas ocultas nombradas `.DS_Store` [^1].<br>

[^1]: Vea esta publicación de stackoverflow sobre [ignoring .DS_Store]( http://stackoverflow.com/questions/18393498/gitignore-all-the-ds-store-files-in-every-folder-and-subfolder) activado en cada carpeta primaria y secundaria.

Para excluir un archivo tal como `.DS_Store` de toda actividad de git, agregamos el nombre del archivo a `.gitignore`.

```bash
-> cat .gitignore
.DS_Store
```

Esto le indica a Git que ignore cualquier archivo `.DS_Store` en tu repositorio, evitando que sean rastreados o añadidos.

En algunas ocasiones, después de agregar `.DS_Store` a tu `.gitignore`, podrías seguir viéndolo en la salida de `git status`. Veamos cómo resolver ese problema.

Para revisar el estado de tu repositorio Git, puedes ejecutar el comando:

```bash
-> git status
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   .DS_Store
        deleted:    scripts/update_U2004.sh
        modified:   scripts/install_docker.sh

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        chrony_install/install_chrony_Ubuntu_U2204.sh
```

La salida del comando anterior muestra la línea `modified: .DS_Store`. No queremos eso.

Si git status aún muestra `.DS_Store` después de haberlo agregado a tu `.gitignore`, probablemente se deba a que ya estaba siendo rastreado por git antes de actualizar `.gitignore`. El archivo `.gitignore` solo previene que _nuevos_ archivos sean rastreados; no afecta archivos que ya estaban presentes en el repositorio al momento que creamos el archivo `.gitignore`.

Para dejar de rastrear `.DS_Store` y asegurarte de que sea ignorado en el futuro, puedes seguir estos pasos:

1. Asegúrate de que `.DS_Store` esté listado en tu archivo `.gitignore`.
2. Elimina `.DS_Store` del índice (área de preparación) manteniéndolo en tu sistema de archivos local.
3. Haz commit de los cambios para actualizar el repositorio.

```bash
git rm --cached .DS_Store
git add .gitignore
git commit -m "Dejar de rastrear .DS_Store y actualizar .gitignore"
```

Esto eliminará `.DS_Store` del repositorio (pero no de tu sistema de archivos local) y asegurará que sea ignorado en el futuro.

[Regresar a la página principal]({{site.baseurl}}/).