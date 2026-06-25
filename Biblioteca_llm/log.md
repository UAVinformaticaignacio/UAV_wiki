# Log de Operaciones

Registro cronológico append-only. Mantenido por el LLM — no editar manualmente.
Formato de cada entrada: `## [YYYY-MM-DD] tipo | descripción`

---

## [2026-06-24] ingesta + reorganización | QoS y subtema "ROS2 — Inicio"

- Nuevas fuentes: `raw/qos-ros2.md`, `raw/workspace_ros2_v3.md`.
- Creada `wiki/drones/software/QoS.md` como página hija de [[Conceptos Básicos de ROS2]] (reliability, history/depth, durability; por qué un mismatch de QoS deja nodos sin conectarse sin error visible).
- Creado nuevo subtema `wiki/drones/software/ROS2 — Inicio.md` dentro de [[ROS2]], con su propia sección `## Subtemas`.
- Creada `wiki/drones/software/Workspace ROS2.md` (crear el workspace, `colcon build`, `source install/setup.bash`) como página hija de `ROS2 — Inicio`.
- Movida `Ejecutar un Nodo ROS2.md` para ser hija de `ROS2 — Inicio` en vez de hija directa de `ROS2` (Conexiones actualizadas).
- `ROS2.md`: tabla de Subtemas actualizada — `Ejecutar un Nodo ROS2` reemplazada por `ROS2 — Inicio`.
- `Conceptos Básicos de ROS2.md`: tabla de Subtemas ampliada con `QoS`.
- `DDS.md`: añadida conexión cruzada hacia `QoS` (detalle de las políticas que DDS expone).
- `index.md`: descripción de fila `[[ROS2]]` actualizada para mencionar QoS y el nuevo subtema de inicio.

---

## [2026-06-18] ingesta | Conceptos Básicos de ROS2 (comunicación y DDS)

- Nuevas fuentes: `raw/ros2-comunicacion-conceptos.md`, `raw/ros2-dds-concepto.md`.
- Creado subtema `wiki/drones/software/Conceptos Básicos de ROS2.md` dentro de [[ROS2]], con su propia sección `## Subtemas`.
- Creadas dos páginas hijas de ese subtema: `Comunicación en ROS2.md` (topic/service/action/parameter) y `DDS.md` (descubrimiento, serialización, QoS, microXRCE-DDS, uORB).
- `ROS2.md`: la sección inline "Conceptos básicos" se reemplazó por un enlace a la nueva página de subtema; tabla de Subtemas actualizada.
- `Comunicación ROS2-PX4.md`: añadidas conexiones cruzadas hacia `DDS` y `Comunicación en ROS2` (detalle de lo que esa página da por sentado).
- `index.md`: descripción de fila `[[ROS2]]` actualizada para mencionar el nuevo subtema.
- `ROS2.md`: ampliada la introducción para explicar qué es ROS2 (no es un OS, es middleware sobre Linux) y el modelo pub/sub sobre topics (publisher/subscriber desacoplados, callback automático), con remisión a [[Conceptos Básicos de ROS2]] para las otras 3 primitivas.

---

## [2026-06-17] reorganización | Python como tema, POO de Python como subtema

- `index.md`: eliminada la fila `[[POO de Python]]` de la tabla de Python; solo queda `[[Python]]` como tema de nivel superior, con nota de que POO de Python vive dentro.
- `Python.md` ya tenía la sección `## Subtemas` correcta apuntando a `[[POO de Python]]` — sin cambios.

---

## [2026-06-12] reorganización | Jerarquía tema → subtema en toda la wiki

- Nueva regla (registrada en `CLAUDE.md`): las tablas de `index.md` solo llevan temas/tecnologías; cada página de tema lista sus subtemas en una sección `## Subtemas` con tabla.
- `CLAUDE.md`: añadida sección "Jerarquía de la wiki: tema → subtema" con los tres niveles, formatos de tabla y reglas para futuras adiciones; pasos de INGERIR actualizados (subtemas se registran en su tema, no en `index.md`).
- `index.md` reescrito: subtemas eliminados de las tablas (páginas de Git, Ejecutar un Nodo ROS2, Comunicación ROS2-PX4, SITL y HITL, SITL Quick Start, Docker — Imágenes y Contenedores).
- Secciones `## Subtemas` añadidas: `ROS2` (→ Ejecutar un Nodo ROS2), `Software de Drones` (→ Comunicación ROS2-PX4, SITL Quick Start), `Pipeline de Desarrollo de Drones` (→ SITL y HITL, SITL Quick Start).
- Secciones existentes renombradas a `## Subtemas` y unificadas en formato tabla: `Python`, `Git y GitHub`, `Docker`, `POO de Python`.
- `Drones.md`: tabla de Software reducida a tecnologías (Ejecutar un Nodo ROS2 ahora vive dentro de ROS2).

---

## [2026-06-12] reorganización | Apartado Drones con sección Software

- Creada página central `wiki/drones/Drones.md` (mapa del apartado: Software / Pipeline / Pruebas).
- Creada `wiki/drones/software/Software de Drones.md` (síntesis): stack completo, cadena de comunicación y arranque con 4 terminales.
- Movidas a `wiki/drones/software/`: `ROS2`, `Ejecutar un Nodo ROS2` (desde `wiki/ros2/`, carpeta eliminada), `PX4`, `Gazebo`, `RViz2`, `Comunicación ROS2-PX4`.
- ROS2 deja de ser tema de nivel superior en el índice: ahora vive dentro de Drones → Software.
- Añadidos enlaces a [[Drones]] y [[Software de Drones]] en las Conexiones de las páginas afectadas.
- `index.md` reestructurado: sección Drones con subsecciones Software y Pipeline/Pruebas.

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
