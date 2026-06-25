---
tipo: guía
tags: [git, merge, conflictos, ramas]
fuentes: [notion/Merge — Fusionar ramas y resolver conflictos]
actualizado: 2026-05-19
---

# Git — Merge

Fusionar una rama en otra cuando el trabajo está listo.

---

## Fusionar Ramas

```bash
# 1. Muévete a la rama que va a recibir los cambios:
git switch main

# 2. Fusiona:
git merge nombre-de-la-rama
```

---

## Resolución de Conflictos

Si dos ramas modificaron las **mismas líneas** del mismo archivo, Git no puede decidir solo → genera un **conflicto**.

Pasos para resolverlo:

1. Git avisa del conflicto al hacer `merge`
2. Abre el archivo en tu editor — verás marcas con los dos cambios
3. Elige qué versión conservar (o combínalas manualmente)
4. Guarda el archivo
5. Finaliza la fusión:

```bash
git add nombre_del_archivo
git commit -m "Resuelve conflicto en nombre_del_archivo"
```

---

## Conexiones

- [[Git — Ramas]] — cómo crear las ramas antes de hacer merge
- [[Git — Commits]] — el commit que cierra la resolución del conflicto
- [[Git y GitHub]] — índice general
