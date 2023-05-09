---
layout: default
title: Conceptos
permalink: /minikube-basics/
parent: Minikube
nav_order: 1
---

This page: Minikube Basics

# Minikube

En este document hablaremos the Minikube.
_El objectivo es enternder la manera de usar esta herramienta para crear un ambiente para probar varios aspectos de Kubernetes._

## Listar Minikube 

Ejecutemos el comando para listar la version the minikube instalada localmente.

```bash
-> minikube version
minikube version: v1.28.0
commit: 986b1ebd987211ed16f8cc10aed7d2c42fc8392f
```

## Verificar Instancias de Kubernetes

Verificar las instancias the kubernetes installadas corrientemente.
This is el resultado en forma de **comando**.

```bash
-> minikube profile list
🎉  minikube 1.29.0 is available! Download it: https://github.com/kubernetes/minikube/releases/tag/v1.29.0
💡  To disable this notice, run: 'minikube config set WantUpdateNotification false'

|-------------------|-----------|---------|--------------|------|---------|---------|-------|--------|
|      Profile      | VM Driver | Runtime |      IP      | Port | Version | Status  | Nodes | Active |
|-------------------|-----------|---------|--------------|------|---------|---------|-------|--------|
| minikube-v1.23.4a | hyperkit  | docker  | 172.16.156.9 | 8443 | v1.23.4 | Stopped |     3 |        |
|-------------------|-----------|---------|--------------|------|---------|---------|-------|--------|
```

Este es el resultado en forma the **tabla**.

|-------------------|-----------|---------|--------------|------|---------|---------|-------|--------|
|      Profile      | VM Driver | Runtime |      IP      | Port | Version | Status  | Nodes | Active |
|-------------------|-----------|---------|--------------|------|---------|---------|-------|--------|
| minikube-v1.23.4a | hyperkit  | docker  | 172.16.156.9 | 8443 | v1.23.4 | Stopped |     3 |        |
|-------------------|-----------|---------|--------------|------|---------|---------|-------|--------|

> NOTA: la lista es dinámica y puede cambiar

[Return to main page]({{site.baseurl}}/).