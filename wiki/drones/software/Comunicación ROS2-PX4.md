---
tipo: concepto
tags: [ROS2, PX4, DDS, MAVLink, uORB, drones]
fuentes: [raw/pipeline-desarrollo-drones.md]
actualizado: 2026-06-12
---

# Comunicación ROS2 ↔ PX4

Cómo se conectan los componentes del stack de un dron, desde el código ROS2 hasta el suelo (QGroundControl).

---

## Cadena de comunicación

```
Tu código (ROS2)
    │  px4_msgs (topics DDS)
    ▼
Micro XRCE-DDS Agent  ←── bridge entre ROS2 y PX4
    │  uORB (protocolo interno)
    ▼
PX4 Autopilot
    │  MAVLink (protocolo estándar)
    ▼
QGroundControl  ←── planificación visual de misiones
```

---

## Protocolos

| Protocolo | Entre qué | Para qué |
|---|---|---|
| **DDS** (topics) | ROS2 ↔ PX4 | Comandos y telemetría en tiempo real |
| **uORB** | Interno PX4 | Comunicación entre módulos del firmware |
| **MAVLink** | PX4 ↔ QGroundControl | Monitoreo, misiones, configuración |

---

## Micro XRCE-DDS Agent

Es el puente entre los topics DDS de ROS2 (`px4_msgs`) y el bus interno **uORB** de [[PX4]]. Se levanta como proceso aparte:

```bash
MicroXRCEAgent udp4 -p 8888
```

Ver [[SITL Quick Start]] (Terminal 2).

---

## Conexiones
- [[PX4]] — lado autopilot de la comunicación
- [[Pipeline de Desarrollo de Drones]] — diagrama completo del stack
- [[SITL Quick Start]] — cómo levantar el Micro XRCE-DDS Agent
- [[Ejecutar un Nodo ROS2]] — el nodo ROS2 que publica `px4_msgs`
