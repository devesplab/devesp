---
layout: default
title: Ignorando Archivos en Linux
permalink: /git-ignore-linux/
parent: Artículos De Git  
has_children: false
has_toc: false
nav_order: 2
---

## Ignorar archivos de Git en Linux

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

Ignorar cierto tipo de recursos en una fuente de código puede ser tan impmortante como los recursos que deseamos rastrear. Ester articulo se concentra como manejamos esto en Linux.

## Usar un Patrón Para Ignorar Archivos en Git

En ocasiones deseamos omitir, o no rastrear cambios a archivos o carpetas que no son relevantes a la fuente de código en la que estamos trabajando. Github provee tal mecanismo via el archivo de configuración `.gitignore`.

En este ejemplo ignoremos las carpetas nombradas `miArchivoExcluido`.

> Vea el articulo relacionado con [Ignorar Arc hivos de Git](./git_articles/git_1a_ignore.md).

Este comando encuentra toda carpeta que tiene el patrón `miArchivoExcluido` como parte del nombre de la carpeta.

El comando se ejecuta dentro del repositorio local y borra todas las carpetas que encuentra que coincide con la expresion regular especificada por el argumento a `-name`.

```bash
-> find . -name miArchivoExcluido -print0 | xargs -0 git rm --ignore-unmatch??
```

Crear un archivo `.gitignore` global.

```bash
-> echo "miArchivoExcluido" > /Users/devuser/.gitignore
```

Configurar git para usar `.gitignore` globalmente.

```bash
-> git config --global core.excludesfile /Users/devuser/.gitignore
```

Como observamos, hemos creado `$HOME/.gitignore`. Eso hace que la configuración este disponible por defecto a todo repositorio de git que se encuentre en el directorio hogar del usuario.

Añade contenido a `.gitignore`.

> Ten en cuenta que puedes añadir comentarios usando `#`.

```bash
-> cat .gitignore
miArchivoExcluido
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

Lo configuración anterior indica que no queremos enviar archivos de tipo ISO, LOG, TAR o GZ. <br>
Podemos añadir mas patrones de expresiones regulares según sea necesario. <br>
La entrada `.*` ignora archivos ocultos como `.bashrc`.

[Regresar a la página principal]({{site.baseurl}}/).