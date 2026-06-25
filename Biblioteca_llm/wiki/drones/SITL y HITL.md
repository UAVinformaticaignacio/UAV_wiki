---
tipo: concepto
tags: [SITL, HITL, drones, PX4, pruebas]
fuentes: [raw/pipeline-desarrollo-drones.md, raw/sitl-quick-start.md]
actualizado: 2026-06-12
---

# SITL y HITL

Dos etapas de prueba antes de volar un dron real, en orden creciente de realismo. Forman las Fases 3 y 4 del [[Pipeline de Desarrollo de Drones]].

---

## SITL — Software In The Loop

Todo corre en el PC, sin hardware: [[PX4]] + [[Gazebo]] + Micro XRCE-DDS Agent + nodo ROS2 propio + [[RViz2]] (ver [[SITL Quick Start]]).

Qué se prueba:

- Despegue y aterrizaje autónomo
- Navegación por waypoints
- Failsafes (pérdida de GPS, batería baja)
- Obstacle avoidance
- Misiones multi-dron

> [!info] Ventaja
> Si crasheas, reinicias. Costo = $0.

---

## HITL — Hardware In The Loop

```
Pixhawk real ←──serial──→ PC con Gazebo
   (firmware)              (sensores simulados)
```

PX4 corre en el **Pixhawk real**, pero los sensores siguen siendo simulados desde el PC.

> [!tip] Para qué sirve
> Verificar que el firmware corre bien en el chip real, que la comunicación serial funciona, y que los tiempos de respuesta son correctos.

---

## Comparación

| | SITL | HITL |
|---|---|---|
| Dónde corre PX4 | Como proceso en el PC | En el Pixhawk real |
| Simulación | Todo simulado | Hardware real + sensores simulados |
| Velocidad de iteración | Rápida | Más lenta, pero más realista |

---

## Regla de oro

> Nunca vueles código que no hayas probado en SITL. Después de SITL, HITL valida el hardware antes de pasar a la Fase 5 (integración hardware) del [[Pipeline de Desarrollo de Drones]].

---

## Conexiones
- [[Drones]] — apartado central del tema
- [[Pipeline de Desarrollo de Drones]] — Fases 3 y 4
- [[PX4]] — modos de ejecución
- [[Gazebo]] — simulador usado en ambas etapas
- [[SITL Quick Start]] — comandos para levantar SITL
