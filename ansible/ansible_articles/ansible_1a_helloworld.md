---
layout: default
title:  Ansible Hello World
permalink: /ansible-hello-world/
parent: Artículos De Ansible
has_children: false
has_toc: false
nav_order: 1
---

# Ansible Hello World

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

## Ejemplo Simple De Ansible Playbook

Ansible Hello World es un playbook sencillo que demuestra la estructura básica y la funcionalidad de un playbook de Ansible. A menudo se utiliza como punto de partida para aprender o para probar instalaciones de Ansible.
```yaml
- name: Ansible Hello World
  hosts: localhost
  tasks:
    - name: Print Hello World
      ansible.builtin.debug:
        msg: "Hello, World!"
```
Este playbook se ejecuta en localhost e imprime "Hello, World!" en la consola usando el módulo `ansible.builtin.debug`. Es una forma simple pero efectiva de comenzar con Ansible y entender cómo funcionan los playbooks.
