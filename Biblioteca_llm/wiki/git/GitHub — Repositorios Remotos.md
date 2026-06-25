---
tipo: guía
tags: [github, remoto, push, pull, clone]
fuentes: [notion/GitHub — Repositorios Remotos]
actualizado: 2026-05-19
---

# GitHub — Repositorios Remotos

GitHub es una plataforma en la nube para almacenar y compartir repositorios. Git es la herramienta local; GitHub es el servidor remoto.

---

## Conectar tu Repo Local a GitHub

```bash
# 1. Crea el repositorio en github.com (sin README)
# 2. Conéctalo con tu repo local:
git remote add origin https://github.com/tu-usuario/tu-repo.git

# Verificar conexión:
git remote -v
```

---

## Subir Cambios (Push)

```bash
git push -u origin main   # Primera vez
git push                  # Las siguientes veces
```

---

## Bajar Cambios (Pull)

```bash
git pull
```

---

## Clonar un Repositorio Existente

```bash
git clone https://github.com/usuario/repositorio.git
```

---

## Flujo de Trabajo Típico

```
1. git pull              ← bajas los últimos cambios
2. (trabajas)
3. git add .
4. git commit -m "..."
5. git push              ← subes tus cambios
```

> Siempre haz `git pull` antes de empezar a trabajar para evitar conflictos.

---

## Conexiones

- [[Git — Flujo Básico]] — el flujo local antes del push
- [[Git — Ramas]] — sincronizar ramas remotas
- [[Git y GitHub]] — índice general
