---
layout: default
title:  Find Minikube Profiles
permalink: /minikube-find-profiles/
parent: Artículos De Minikube
has_children: false
has_toc: false
nav_order: 1
---

# Finding Minikube Profiles

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

Para encontrar perfiles de Minikube, puedes utilizar el siguiente comando en tu terminal:

```bash
minikube profile list
```
Este comando te mostrará una lista de todos los perfiles de Minikube que tienes configurados en tu sistema. Cada perfil representa una instancia separada de Minikube, lo que te permite ejecutar múltiples clústeres de Kubernetes en tu máquina local.
Si deseas ver más detalles sobre un perfil específico, puedes usar:

```bash
minikube profile <profile-name>
```
Reemplaza `<profile-name>` con el nombre del perfil que deseas inspeccionar. Esto te proporcionará información adicional sobre ese perfil, como la versión de Kubernetes, el estado del clúster y más.
Si necesitas crear un nuevo perfil, puedes hacerlo con el siguiente comando:

```bash
minikube start -p <new-profile-name>
```
Reemplaza `<new-profile-name>` con el nombre que deseas asignar al nuevo perfil. Esto iniciará un nuevo clúster de Minikube con el perfil especificado.
Si deseas eliminar un perfil de Minikube, puedes usar el siguiente comando:

```bash
minikube delete -p <profile-name>
```
Reemplaza `<profile-name>` con el nombre del perfil que deseas eliminar. Esto eliminará el clúster asociado a ese perfil y liberará los recursos utilizados por él.
Para obtener más información sobre los perfiles de Minikube y cómo administrarlos, puedes consultar la [documentación oficial de Minikube](https://minikube.sigs.k8s.io/docs/commands/profile/).
Para más información sobre Minikube y sus características, puedes visitar la [documentación de Minikube](https://minikube.sigs.k8s.io/docs/) o explorar diversos recursos y tutoriales en línea que cubren conceptos, comandos y buenas prácticas de Minikube.
Para más información sobre Minikube y sus características, puedes visitar la [documentación de Minikube](https://minikube.sigs.k8s.io/docs/) o explorar diversos recursos y tutoriales en línea que cubren conceptos, comandos y buenas prácticas de Minikube.
Para más información sobre Minikube y sus características, puedes visitar la [documentación de Minikube](https://minikube.sigs.k8s.io/docs/) o explorar diversos recursos y tutoriales en línea que cubren conceptos, comandos y buenas prácticas de Minikube.