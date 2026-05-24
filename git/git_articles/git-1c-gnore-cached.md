---
layout: default
title: Ignorando Recursos en Cache 
permalink: /git-ignore-cached/
parent: Artículos De Git  
has_children: false
has_toc: false
nav_order: 3
---

# Ignorar archivos de Git en CACHE

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

En algun momento encontramos que git esta rastreando recursos que no son necesarios empujar a la fuente de código que distribuimos. Por esa razón, tomamos las acciones descritas en este documento y asi mantener una fuente de código limpia.

## Ignorando Carpetas Rastreadas en Git

En este ejemplo, dejamos de rastrear una carpeta llamada `_site/`.

> Este es un ejemplo real usando Jekyll

Aqui estamos en una rama de git llamada `myRamaDeTrabajo`. Cuando inspeccionamos los cambios, vemos varias entradas con `_site/`. 

Pero no deseamos que Git rastree esa carpeta.
```
-> git status
On branch myRamaDeTrabajo
Your branch is up to date with 'origin/myRamaDeTrabajo'.

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	deleted:    .jekyll-metadata
	modified:   _site/404.html
	modified:   _site/about/index.html
	modified:   _site/ansible-hello-world/index.html
	modified:   _site/ansible/index.html
...
```

Para evitar que Git rastree la carpeta `_site/`, hacemos lo siguiente:

> Usamos la bandera `-r` porque estamos operando en una carpeta.

```
git rm -r --cached _site/
echo "_site/" >> .gitignore
git add .gitignore
git commit -m "Dejar de rastrear _site/ y actualizar .gitignore"
```

Lo anterior indica lo siguiente: 
- `--cached` elimina archivos solo del seguimiento de Git 
- `-r` es requerida para traversar la carpeta
- `echo` agrega el nombre de la carpeta a `.gitignore`
- la carpeta `_site/` real permanece en el disco 
- `git commit` es para que los futuros cambios bajo `_site/` serán ignorados 

Puedes comprobarlo después con:
```
git status
```

Esto eliminará `_site/` del repositorio (pero no de tu sistema de archivos local) y asegurará que sea ignorado en el futuro.

## Ignorando Archivos Rastreadas en Git

Podemos dejar de rastrear archivos de la misma manera que lo hicimos con carpetas.

```
git rm --cached nombreDeArchivo
echo "nombreDeArchivo" >> .gitignore
git commit -m "Stop tracking nombreDeArchivo"
git status
```

Esto eliminará `nombreDeArchivo` del repositorio (pero no de tu sistema de archivos local) y asegurará que sea ignorado en el futuro.


[Regresar a la página principal]({{site.baseurl}}/).