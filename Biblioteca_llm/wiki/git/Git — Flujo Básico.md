---
tipo: guía
tags: [git, commits, staging, gitignore]
fuentes: [notion/Flujo Básico de Git]
actualizado: 2026-05-19
---

# Git — Flujo Básico

El ciclo de trabajo central de Git: editar, preparar y guardar.

---

## Conceptos Clave

- **Repositorio:** carpeta del proyecto con todo el historial de Git
- **Working directory:** archivos en los que estás trabajando ahora
- **Staging area:** zona de preparación antes de guardar un commit
- **Commit:** foto del estado de tu código en un momento dado

---

## El Flujo

```
Trabajas  →  git add  →  git commit
(editas)     (preparas)   (guardas)
```

---

## Comandos Esenciales

```bash
git status          # ¿Qué está pasando ahora?
git add archivo     # Preparar un archivo específico
git add .           # Preparar todos los archivos modificados
git commit -m "msg" # Guardar la versión con un mensaje
git log             # Ver el historial completo
git log --oneline   # Ver el historial resumido (recomendado)
```

> El mensaje del commit debe describir **qué hiciste**. Ej: `"Agrega validación de formulario de login"`.

---

## Ignorar Archivos con .gitignore

Crea un archivo `.gitignore` y lista lo que Git debe ignorar:

```
node_modules/
.env
*.log
```

---

## Deshacer Cambios

```bash
# Revertir un archivo al estado del último commit:
git restore nombre_archivo.txt

# Ver exactamente qué líneas cambiaron (antes del add):
git diff
```

---

## Conexiones

- [[Git — Cómo Iniciar]] — cómo crear el repo primero
- [[Git — Commits]] — detalle del ciclo de commits
- [[Git y GitHub]] — índice general
