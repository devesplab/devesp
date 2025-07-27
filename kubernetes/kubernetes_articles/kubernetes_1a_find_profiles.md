---
layout: default
title:  Find Kubernetes Profiles
permalink: /kubernetes-find-profiles/
parent: Artículos De Kubernetes
has_children: false
has_toc: false
nav_order: 1
---

# Finding Kubernetes Profiles

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

Para encontrar perfiles de Kubernetes, puedes utilizar el comando `kubectl config get-contexts` para listar todos los contextos configurados en tu entorno de Kubernetes. Cada contexto representa un perfil que incluye información sobre el clúster, el usuario y el espacio de nombres.
```bash
kubectl config get-contexts
```
Este comando te mostrará una lista de todos los contextos disponibles, junto con el contexto actual que estás utilizando. El contexto actual se marcará con un asterisco (*).
Si deseas cambiar al contexto de un perfil específico, puedes usar el siguiente comando:

```bash
kubectl config use-context <context-name>
```
Reemplaza `<context-name>` con el nombre del contexto que deseas utilizar. Esto cambiará tu configuración actual al perfil especificado.
Si necesitas crear un nuevo perfil, puedes hacerlo editando el archivo de configuración de Kubernetes (`~/.kube/config`) o utilizando el comando `kubectl config set-context` para definir un nuevo contexto.
```bash
kubectl config set-context <new-context-name> --cluster=<cluster-name> --user=<user-name> --namespace=<namespace>
```
Reemplaza `<new-context-name>`, `<cluster-name>`, `<user-name>` y `<namespace>` con los valores apropiados para tu nuevo perfil.
Si deseas eliminar un perfil, puedes usar el siguiente comando:

```bash
kubectl config delete-context <context-name>
```