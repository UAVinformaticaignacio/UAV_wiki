# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Qué es esto

Este vault de Obsidian funciona como una **wiki personal mantenida por IA**. Tú aportas fuentes y preguntas; yo escribo y mantengo la wiki. Nunca escribas en `wiki/` ni en `index.md`/`log.md` manualmente — esos archivos son míos. Las fuentes en `raw/` son tuyas y son inmutables.

---

## Estructura de directorios

```
/
├── CLAUDE.md          ← este archivo (esquema y reglas)
├── index.md           ← catálogo de todas las páginas wiki
├── log.md             ← registro cronológico de operaciones
├── wiki/              ← páginas generadas y mantenidas por el LLM
└── raw/               ← fuentes originales (no modificar)
    └── assets/        ← imágenes descargadas localmente
```

---

## Formato de páginas wiki

Cada página en `wiki/` lleva frontmatter YAML y cuerpo en Markdown:

```markdown
---
tipo: [concepto | entidad | fuente | síntesis | consulta]
tags: [lista de etiquetas]
fuentes: [lista de archivos raw/ que alimentaron esta página]
actualizado: YYYY-MM-DD
---

# Título

Resumen de 1-3 oraciones.

---

## Secciones de contenido
...

## Conexiones
- [[Página relacionada]] — por qué está relacionada
```

Los tipos de página:
- **concepto** — idea, técnica, patrón (ej. "Publicador ROS2")
- **entidad** — persona, proyecto, herramienta, paquete (ej. "rclpy")
- **fuente** — resumen de un documento raw/
- **síntesis** — análisis o comparación elaborado al responder una consulta
- **consulta** — respuesta archivada a una pregunta importante

---

## Operaciones

### INGERIR una fuente nueva

Cuando el usuario añade un archivo a `raw/` o pega contenido:

1. Leer la fuente completa.
2. Discutir con el usuario los puntos clave si hace falta.
3. Crear una página `wiki/<título>.md` de tipo `fuente` con el resumen.
4. Actualizar las páginas de conceptos y entidades afectadas (crear si no existen).
5. Actualizar `index.md`.
6. Añadir entrada al `log.md`.

Una sola fuente puede tocar 5-15 páginas wiki.

### RESPONDER una consulta

1. Leer `index.md` para identificar páginas relevantes.
2. Leer esas páginas.
3. Sintetizar la respuesta con citas a páginas wiki.
4. Si la respuesta es suficientemente valiosa, archivarla como página `wiki/<pregunta-resumida>.md` de tipo `consulta` o `síntesis`.
5. Actualizar `index.md` y `log.md` si se crearon páginas nuevas.

### LINT (revisión de salud)

Buscar y reportar:
- Contradicciones entre páginas.
- Páginas huérfanas (sin enlaces entrantes).
- Conceptos mencionados sin página propia.
- Referencias cruzadas faltantes.
- Afirmaciones desactualizadas por fuentes más recientes.

---

## Reglas de mantenimiento

- **Español** en todo el contenido wiki. Términos técnicos en su forma original (ej. `rclpy`, `colcon`).
- **Wiki-links** con sintaxis Obsidian: `[[Nombre de la página]]`.
- **Secciones** separadas con `---`.
- Actualizar siempre `index.md` y `log.md` tras cualquier cambio.
- Nunca modificar archivos en `raw/`.
- Nunca editar `.obsidian/`.

---

## Contexto actual

El vault cubre **ROS2 con Python (rclpy)**. Páginas existentes:
- `wiki/ROS2.md` — referencia general (cuerpo pendiente de desarrollar)
- `wiki/Ejecutar un Nodo ROS2.md` — guía paso a paso: crear, compilar y ejecutar un nodo Python
