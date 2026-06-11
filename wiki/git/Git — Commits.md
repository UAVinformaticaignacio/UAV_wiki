---
tipo: guía
tags: [git, commits, historial, tags]
fuentes: [notion/Commits — Cómo guardar versiones]
actualizado: 2026-05-19
---

# Git — Commits

Un commit es una **foto del estado de tu código** en un momento dado. Es la unidad básica del historial de Git.

---

## Flujo Completo

### 1. Ver qué archivos cambiaron

```bash
git status
# Rojo = no preparado | Verde = listo para commit
```

### 2. Preparar archivos (staging)

```bash
git add nombre_archivo.txt   # Un archivo específico
git add .                    # Todos los archivos modificados
```

### 3. Guardar el commit

```bash
git commit -m "Descripción clara de lo que hiciste"
```

### 4. Ver el historial

```bash
git log            # Vista completa
git log --oneline  # Vista resumida (recomendada)
```

---

## Tags — Marcar Versiones Importantes

```bash
git tag "v1.0"
```

Útil para etiquetar releases o hitos importantes.

> El mensaje del commit es una nota para humanos. Sé descriptivo — tu yo del futuro te lo agradecerá.

---

## Conexiones

- [[Git — Flujo Básico]] — el flujo simplificado
- [[Git — Commits Avanzados]] — reset, reflog, stash
- [[Git y GitHub]] — índice general
