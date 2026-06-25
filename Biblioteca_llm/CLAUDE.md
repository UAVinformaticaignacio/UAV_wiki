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

## Jerarquía de la wiki: tema → subtema

Toda la wiki se organiza en tres niveles. Las tablas siempre llevan **temas**; los **subtemas** viven dentro de la página de su tema.

```
index.md (temas)
└── Página de tema            ← tecnología o módulo principal (ej. ROS2, Docker, Git y GitHub)
    └── Página de subtema     ← guía o detalle concreto (ej. Ejecutar un Nodo ROS2)
```

**Nivel 1 — `index.md`:** una sección por tema con una tabla `| Tema | Resumen |`. Solo páginas de tema; ningún subtema aparece aquí. Si un tema tiene subtemas, su resumen lo indica con "— subtemas dentro: ...".

**Nivel 2 — página de tema:** además del contenido propio, lleva una sección `## Subtemas` con tabla:

```markdown
## Subtemas

| Subtema | Resumen |
|---|---|
| [[Página hija]] | Qué cubre |
```

**Nivel 3 — página de subtema:** el contenido detallado. Enlaza de vuelta a su tema en `## Conexiones`.

Reglas para futuras adiciones:

- **Fuente nueva sobre un tema existente** → crear las páginas nuevas como subtemas, registrarlas en la tabla `## Subtemas` de su tema, **no** en `index.md`.
- **Tema nuevo** → crear su subcarpeta en `wiki/`, su página de tema con `## Subtemas`, y añadirlo a `index.md`.
- **Un apartado puede tener sub-apartados** (ej. Drones → Software): el sub-apartado es una página de tema dentro de una subcarpeta (`wiki/drones/software/`), y la página central del apartado (`Drones.md`) lo enlaza en sus tablas.
- Un subtema que crece hasta tener páginas hijas propias se promociona: gana su sección `## Subtemas` (ej. POO de Python dentro de Python).
- Las `## Conexiones` al final de cada página se mantienen siempre — son lo que genera el grafo de Obsidian.

Ejemplo vivo: [[Drones]] (apartado) → [[Software de Drones]] (sub-apartado) → [[ROS2]] (tecnología) → [[Ejecutar un Nodo ROS2]] (subtema: crear el environment y correr un nodo).

---

## Operaciones

### INGERIR una fuente nueva

Cuando el usuario añade un archivo a `raw/` o pega contenido:

1. Leer la fuente completa.
2. Discutir con el usuario los puntos clave si hace falta.
3. Crear una página `wiki/<título>.md` de tipo `fuente` con el resumen.
4. Actualizar las páginas de conceptos y entidades afectadas (crear si no existen), respetando la jerarquía tema → subtema: las páginas nuevas se registran en la tabla `## Subtemas` de su tema.
5. Actualizar `index.md` solo si aparece un tema nuevo (los subtemas no van al índice).
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

- **Jerarquía tema → subtema** en toda la wiki: las tablas de `index.md` listan solo **temas** (tecnologías y módulos principales); cada página de tema tiene una sección `## Subtemas` con una tabla `| Subtema | Resumen |` que lista sus páginas hijas. Los subtemas no aparecen en `index.md`.
- **Español** en todo el contenido wiki. Términos técnicos en su forma original (ej. `rclpy`, `colcon`).
- **Wiki-links** con sintaxis Obsidian: `[[Nombre de la página]]`.
- **Secciones** separadas con `---`.
- Actualizar siempre `index.md` y `log.md` tras cualquier cambio.
- Nunca modificar archivos en `raw/`.
- Nunca editar `.obsidian/`.

---

## Contexto actual

El vault cubre cuatro temas, cada uno en su subcarpeta de `wiki/`:
- `wiki/python/` — POO de Python (7 módulos)
- `wiki/git/` — Git y GitHub
- `wiki/docker/` — Docker básico
- `wiki/drones/` — apartado Drones, página central `Drones.md`. Incluye `wiki/drones/software/` (el stack: ROS2, PX4, Gazebo, RViz2, comunicación ROS2-PX4) y las páginas de pipeline y pruebas (SITL/HITL, SITL Quick Start).

ROS2 ya no es tema de nivel superior: vive dentro de `wiki/drones/software/`.
