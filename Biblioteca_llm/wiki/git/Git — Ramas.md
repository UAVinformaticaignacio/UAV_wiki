---
tipo: guía
tags: [git, ramas, branches]
fuentes: [notion/Ramas — Cómo crearlas y eliminarlas]
actualizado: 2026-05-19
---

# Git — Ramas

Una rama es una **línea de desarrollo independiente**. Permite trabajar en algo nuevo sin tocar el código principal.

La rama principal se llama `main` (antes `master` — se cambió por políticas de lenguaje inclusivo).

---

## Crear y Cambiar de Rama

```bash
git branch nombre-de-la-rama   # Crea la rama (no te mueve)
git switch nombre-de-la-rama   # Te mueve a esa rama

# Renombrar la rama actual:
git branch -m main
```

---

## Eliminar Ramas

```bash
git branch -d nombre   # Borrado seguro (solo si ya fue fusionada)
git branch -D nombre   # Borrado forzado

# Borrar también en GitHub:
git push origin --delete nombre
```

> No puedes borrar la rama donde estás parado. Primero muévete: `git switch main`.

---

## Conexiones

- [[Git — Merge]] — fusionar ramas cuando terminas el trabajo
- [[Git — Commits Avanzados]] — usar stash al cambiar de rama
- [[GitHub — Repositorios Remotos]] — sincronizar ramas con GitHub
- [[Git y GitHub]] — índice general
