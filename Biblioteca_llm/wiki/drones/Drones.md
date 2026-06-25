---
tipo: concepto
tags: [drones, ROS2, PX4, robotica, indice]
fuentes: [raw/pipeline-desarrollo-drones.md, raw/sitl-quick-start.md]
actualizado: 2026-06-12
---

# Drones

Apartado central sobre desarrollo de drones autónomos: el stack de software (ROS2 + PX4 + Gazebo), cómo se comunican sus componentes y el camino completo de la simulación al vuelo real.

---

## Mapa del apartado

```
Drones
├── Software ──────── el stack: quién hace qué y cómo arranca
│   ├── ROS2 (tu código)
│   ├── PX4 (autopilot)
│   ├── Gazebo (simulador)
│   ├── RViz2 (visualización)
│   └── Comunicación ROS2-PX4
├── Pipeline ──────── las 7 fases: del diseño al despliegue
└── Pruebas ───────── SITL y HITL antes de volar
```

---

## Software

Todo el stack está desarrollado en [[Software de Drones]]: qué hace cada componente, cómo funciona la cadena de comunicación y **cómo se arranca todo con las 4 terminales**.

| Tecnología | Qué es |
|---|---|
| [[Software de Drones]] | **El apartado de software**: stack completo, arquitectura y arranque |
| [[ROS2]] | El framework donde escribes tu código — subtema dentro: crear el environment y ejecutar un nodo |
| [[PX4]] | El piloto automático: estabiliza, ejecuta setpoints, controla motores |
| [[Gazebo]] | El simulador: física, sensores y ambiente |
| [[RViz2]] | Visualización de los datos de ROS2 |

---

## Pipeline de desarrollo

[[Pipeline de Desarrollo de Drones]] cubre las 7 fases: diseño → desarrollo software → SITL → HITL → integración hardware → pruebas de campo → despliegue.

> [!tip] El mismo código
> El código ROS2 que escribes en la Fase 2 es exactamente el que vuela en la Fase 7. Solo cambia el hardware debajo.

---

## Pruebas antes de volar

- [[SITL y HITL]] — las dos etapas de simulación, en orden creciente de realismo.
- [[SITL Quick Start]] — referencia rápida: las 4 terminales para levantar SITL.

> **Regla de oro:** nunca vueles código que no hayas probado en SITL.

---

## Conexiones
- [[Software de Drones]] — el stack y su arranque
- [[Pipeline de Desarrollo de Drones]] — las 7 fases completas
- [[SITL y HITL]] — etapas de prueba
- [[SITL Quick Start]] — arranque con 4 terminales
