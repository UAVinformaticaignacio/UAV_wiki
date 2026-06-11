---
tipo: guía
tags: [git, reset, reflog, stash, HEAD]
fuentes: [notion/Commits Avanzados — Viajes en el tiempo]
actualizado: 2026-05-19
---

# Git — Commits Avanzados

Navegar el historial, deshacer commits y pausar trabajo en progreso.

---

## El Hash: ID Único de cada Commit

Cada commit tiene un código único llamado **hash** (ej. `a3f8c12`). Lo ves con `git log`. Solo necesitas los primeros 7 caracteres.

---

## HEAD: ¿Dónde estoy parado?

**HEAD** es un puntero que indica en qué commit estás trabajando. Normalmente apunta al último commit de tu rama.

---

## Volver a un Commit Anterior

```bash
git checkout a3f8c12
```

Pone tus archivos en ese estado pasado (**Detached HEAD**). Para volver al presente:

```bash
git checkout main   # usa el nombre de la rama, no un hash
```

---

## Eliminar Commits con git reset

```bash
git reset --hard [hash]
```

Mueve la rama al commit indicado. Los commits posteriores quedan "huérfanos" e invisibles en `git log`, pero **no se borran realmente**.

---

## Recuperar con git reflog

`git reflog` guarda **todos tus movimientos**, incluso después de un reset:

```bash
git reflog
# Ver el hash del commit que "eliminaste" y volver a él:
git reset --hard [hash_recuperado]
```

Registra: commits, cambios de rama, merges, resets y pulls.

---

## Git Stash — El Botón de Pausa

Guarda cambios en una pila temporal y deja tu carpeta limpia. Ideal para cambiar de rama sin hacer un commit incompleto.

| Comando | Qué hace |
|---|---|
| `git stash` | Guarda los cambios y limpia la carpeta |
| `git stash pop` | Recupera los cambios más recientes |
| `git stash list` | Muestra todo lo que tienes en pausa |
| `git stash drop` | Elimina el último stash sin aplicarlo |
| `git stash drop stash@{1}` | Elimina uno específico |

---

## Conexiones

- [[Git — Commits]] — flujo básico de commits
- [[Git — Ramas]] — cambiar de rama con stash
- [[Git y GitHub]] — índice general
