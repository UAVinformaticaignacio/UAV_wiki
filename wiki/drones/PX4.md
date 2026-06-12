---
tipo: entidad
tags: [PX4, drones, autopilot]
fuentes: [raw/pipeline-desarrollo-drones.md]
actualizado: 2026-06-12
---

# PX4

**PX4** es el firmware de piloto automático (autopilot) que corre en el controlador de vuelo (ej. Pixhawk) y es el único componente del stack que controla los motores del dron.

---

## Qué hace

| Qué hace | Cómo |
|---|---|
| Estabilizar el dron | Controladores PID (roll, pitch, yaw) |
| Ejecutar comandos | Recibe setpoints desde ROS2 |
| Gestionar failsafes | Ej. pérdida de GPS → aterrizar |
| Fusionar sensores | EKF2 (GPS + IMU + barómetro) |
| Controlar motores | Señales PWM a los ESCs |

> [!warning] Importante
> PX4 es el **único** que toca los motores. Nunca se envía PWM directo desde ROS2 — ROS2 solo envía setpoints (posición/velocidad) y PX4 decide cómo ejecutarlos.

---

## Modos de ejecución

- **SITL** — PX4 corre como proceso en el PC, todo simulado (con [[Gazebo]]).
- **HITL** — PX4 corre en el Pixhawk real, sensores simulados desde el PC.
- **Real** — PX4 corre en el Pixhawk, con hardware real completo.

Ver [[SITL y HITL]] para más detalle.

---

## Comunicación

- Recibe setpoints de ROS2 vía **DDS** (`px4_msgs`) a través del Micro XRCE-DDS Agent.
- Usa **uORB** internamente entre sus módulos de firmware.
- Habla **MAVLink** con QGroundControl para monitoreo, misiones y configuración.

Ver [[Comunicación ROS2-PX4]].

---

## Conexiones
- [[Pipeline de Desarrollo de Drones]] — rol de PX4 en el pipeline completo
- [[Gazebo]] — simulador con el que PX4 corre en SITL/HITL
- [[Comunicación ROS2-PX4]] — protocolos DDS/uORB/MAVLink
- [[SITL y HITL]] — modos de ejecución de PX4
- [[SITL Quick Start]] — cómo levantar PX4 para pruebas
