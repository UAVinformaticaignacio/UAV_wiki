---
tipo: guía
tags: [git, terminal, configuración]
fuentes: [notion/Cómo Iniciar Git]
actualizado: 2026-05-19
---

# Git — Cómo Iniciar

Pasos para instalar, configurar y crear tu primer repositorio.

---

## Prerrequisitos

- Cuenta en **GitHub**
- **Git** instalado (descarga desde [git-scm.com](https://git-scm.com))
- Una carpeta/proyecto donde trabajar

> Al instalar Git se descarga **Git Bash**, la terminal principal. También funciona desde cualquier terminal del sistema.

---

## Configuración Inicial

Solo se hace una vez:

```bash
git config --global user.name "Tu_Nombre"
git config --global user.email "tu_correo@gmail.com"

# Verificar:
git config --list
```

> En Git Bash: copiar = `Ctrl + Insert` | pegar = `Shift + Insert`

---

## Comandos de Terminal Básicos

| Comando     | Qué hace                                       | Significado             |
| ----------- | ---------------------------------------------- | ----------------------- |
| `ls`        | Muestra archivos de la carpeta actual          | list                    |
| `ls -a`     | Muestra también archivos ocultos (como `.git`) | list all                |
| `cd nombre` | Entra a una carpeta                            | Change Directory        |
| `cd ..`     | Vuelve a la carpeta anterior                   |                         |
| `pwd`       | Muestra en qué carpeta estás                   | Print Working Directory |
| `clear`     | Limpia el texto de la terminal                 |                         |

> Usa **TAB** para autocompletar nombres de carpetas.

---

## Iniciar un Repositorio

```bash
# Dentro de tu carpeta de proyecto:
git init

# Ver el estado actual:
git status
```

`git init` crea una carpeta oculta `.git` donde Git guarda todo el historial.

> Usa `git status` constantemente — es tu mejor herramienta para saber qué está pasando.

---

## Conexiones

- [[Git — Flujo Básico]] — siguiente paso tras iniciar el repo
- [[Git y GitHub]] — índice general
