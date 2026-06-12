# Log de Operaciones

Registro cronológico append-only. Mantenido por el LLM — no editar manualmente.
Formato de cada entrada: `## [YYYY-MM-DD] tipo | descripción`

---

## [2026-06-12] ingest | Drones — PX4/ROS2 (7 páginas)

- Fuentes: `raw/pipeline-desarrollo-drones.md`, `raw/sitl-quick-start.md`
- Carpeta `wiki/drones/` creada.
- Páginas wiki creadas: `Pipeline de Desarrollo de Drones` (fuente, 7 fases del pipeline), `SITL Quick Start` (fuente, referencia 4 terminales), `PX4` (entidad), `Gazebo` (entidad), `RViz2` (entidad), `SITL y HITL` (concepto), `Comunicación ROS2-PX4` (concepto, DDS/uORB/MAVLink).
- `wiki/ros2/ROS2.md` desarrollado (estaba vacío): conceptos básicos de ROS2 y enlaces.
- `wiki/ros2/Ejecutar un Nodo ROS2.md`: añadida sección Conexiones enlazando al pipeline de drones.
- Todas las páginas nuevas interconectadas con wiki-links.
- `index.md` actualizado: nueva sección "Drones (PX4)" y sección ROS2 ampliada.

---

## [2026-05-29] ingest | Docker (2 páginas)

- Fuentes: `raw/Docker.md`, `raw/Docker - Imágenes y Contenedores .md`
- Carpeta `wiki/docker/` creada.
- Páginas wiki creadas: `Docker` (intro, conceptos, instalación), `Docker — Imágenes y Contenedores` (comandos de imágenes y contenedores)
- Ambas páginas interconectadas. `Docker.md` lista los módulos existentes.
- `index.md` actualizado con sección Docker.

---

## [2026-05-20] ingest | POO de Python (7 módulos)

- Fuentes: `raw/Modulo_01` a `raw/Modulo_07` (Clases y Objetos, Atributos y Métodos, Init y Self, Métodos Especiales, Herencia, Encapsulamiento, Polimorfismo)
- Carpeta `wiki/python/` creada.
- Páginas wiki creadas: `Python`, `POO de Python`, `POO — Clases y Objetos`, `POO — Atributos y Métodos`, `POO — Init y Self`, `POO — Métodos Especiales`, `POO — Herencia`, `POO — Encapsulamiento`, `POO — Polimorfismo`
- Todas interconectadas con wiki-links en cadena (módulo anterior / siguiente).
- `index.md` actualizado: lista de temas al tope sin submódulos, detalle por tema debajo.

---

## [2026-05-19] reorganización | Subcarpetas git/ y ros2/

- Movidas páginas de Git a `wiki/git/`
- Movidas páginas de ROS2 a `wiki/ros2/`
- `index.md` convertido en página principal con secciones por tema

---

## [2026-05-19] ingest | Notion — Git y GitHub (7 páginas)

- Fuente: biblioteca "Git y GitHub" en Notion (4 módulos, 7 páginas de contenido)
- Páginas wiki creadas: `Git y GitHub`, `Git — Cómo Iniciar`, `Git — Flujo Básico`, `Git — Commits`, `Git — Commits Avanzados`, `Git — Ramas`, `Git — Merge`, `GitHub — Repositorios Remotos`
- Todas las páginas interconectadas con wiki-links.
- Índice actualizado.

---

## [2026-05-19] setup | Inicialización de la wiki

- Creadas carpetas `wiki/`, `raw/`, `raw/assets/`.
- Movidas notas existentes a `wiki/`: `ROS2.md`, `Ejecutar un Nodo ROS2.md`.
- Creados `CLAUDE.md` (esquema), `index.md` (catálogo) y este `log.md`.
- Estado inicial: 2 páginas wiki, 0 fuentes ingeridas.
