---
tipo: síntesis
tags: [drones, ROS2, PX4, Gazebo, RViz2, stack, SITL]
fuentes: [raw/pipeline-desarrollo-drones.md, raw/sitl-quick-start.md]
actualizado: 2026-06-12
---

# Software de Drones

El stack de software de un dron autónomo: qué hace cada componente, cómo se comunican entre sí y cómo se arranca todo en simulación con 4 terminales. Es la sección de software del apartado [[Drones]].

---

## El stack — quién hace qué

| Componente           | Rol                                                                                                     | Página     |
| -------------------- | ------------------------------------------------------------------------------------------------------- | ---------- |
| **ROS2** (tu código) | Cerebro: misiones, percepción, decisiones. Publica setpoints, no toca motores.                          | [[ROS2]]   |
| **PX4**              | Piloto automático: estabiliza, ejecuta setpoints, failsafes, fusiona sensores (EKF2), controla motores. | [[PX4]]    |
| **Gazebo**           | Simula el mundo: física, sensores, ambiente. Solo existe durante SITL/HITL.                             | [[Gazebo]] |
| **RViz2**            | Visualiza los datos de ROS2. No simula nada.                                                            | [[RViz2]]  |

> [!info] Regla clave
> ROS2 le dice a PX4 **a dónde ir**; PX4 decide **cómo llegar** y es el único que controla los motores. Nunca se envía PWM directo desde ROS2.

> [!tip] Analogía
> [[Gazebo]] es el estadio donde se juega el partido; [[RViz2]] es la tele donde se ve. Si apagas la tele, el partido sigue.

---

## Cómo funciona — la cadena de comunicación

```
Tu código (ROS2)                ← nodos: offboard_control, mission_planner...
    │  px4_msgs (topics DDS)
    ▼
Micro XRCE-DDS Agent            ← puente ROS2 ↔ PX4
    │  uORB (protocolo interno)
    ▼
PX4 Autopilot                   ← estabiliza y controla motores
    │  MAVLink
    ▼
QGroundControl                  ← estación de tierra
```

Tu nodo publica setpoints (ej. `TrajectorySetpoint` a `/fmu/in/trajectory_setpoint`), el Agent los traduce a uORB, y PX4 los ejecuta. Detalle de protocolos en [[Comunicación ROS2-PX4]].

---

## Cómo arranca — las 4 terminales (SITL)

Para ver el stack funcionando completo en el PC, sin hardware:

| Terminal | Qué levanta                       | Comando clave                          |
| -------- | --------------------------------- | -------------------------------------- |
| **T1**   | [[PX4]] + [[Gazebo]] (simulación) | `make px4_sitl gz_x500`                |
| **T2**   | Micro XRCE-DDS Agent (puente)     | `MicroXRCEAgent udp4 -p 8888`          |
| **T3**   | Tu nodo [[ROS2]]                  | `ros2 run drone_control offboard_node` |
| **T4**   | [[RViz2]] (visualización)         | `rviz2 -d ~/drone_rviz.rviz`           |

El orden importa: primero la simulación (T1), luego el puente (T2), luego tu código (T3); RViz2 (T4) puede arrancar en cualquier momento.

Comandos completos, verificación de topics y despegue manual en [[SITL Quick Start]].

---

## Escribir tu propio código

Los nodos típicos de un dron (Fase 2 del [[Pipeline de Desarrollo de Drones]]):

- `offboard_control` → enviar comandos de posición/velocidad
- `mission_planner` → secuencia de waypoints
- `obstacle_detector` → leer lidar y detectar obstáculos
- `camera_processor` → procesar imágenes con CV/YOLO

Cómo crear y compilar un nodo desde cero: [[Ejecutar un Nodo ROS2]].

---

## Subtemas

| Subtema | Resumen |
|---|---|
| [[Comunicación ROS2-PX4]] | Detalle de los protocolos: DDS, uORB, MAVLink y el Micro XRCE-DDS Agent |
| [[SITL Quick Start]] | Comandos completos de las 4 terminales, verificación de topics y despegue |

---

## Conexiones
- [[Drones]] — apartado al que pertenece esta sección
- [[ROS2]] · [[PX4]] · [[Gazebo]] · [[RViz2]] — los componentes del stack
- [[Comunicación ROS2-PX4]] — detalle de DDS, uORB y MAVLink
- [[SITL Quick Start]] — los comandos de las 4 terminales
- [[Pipeline de Desarrollo de Drones]] — el stack en el contexto de las 7 fases
- [[Ejecutar un Nodo ROS2]] — primer paso para escribir tu código
