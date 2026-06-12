---
tipo: fuente
tags: [drones, ROS2, PX4, SITL, quick-ref]
fuentes: [raw/sitl-quick-start.md]
actualizado: 2026-06-12
---

# SITL Quick Start — 4 terminales

Referencia rápida para levantar el stack SITL (PX4 + Gazebo + ROS2) en 4 terminales. Para el contexto completo ver [[Pipeline de Desarrollo de Drones]] (Fase 3) y [[SITL y HITL]].

---

## T1 — PX4 + Gazebo

```bash
cd ~/PX4-Autopilot
make px4_sitl gz_x500
```

## T2 — DDS Agent

```bash
MicroXRCEAgent udp4 -p 8888
```

## T3 — Nodo offboard

```bash
source /opt/ros/jazzy/setup.bash
source ~/ros2_ws/install/setup.bash
ros2 run drone_control offboard_node
```

## T4 — RViz2

```bash
source /opt/ros/jazzy/setup.bash
rviz2 -d ~/drone_rviz.rviz
```

---

## Verificación rápida

```bash
# Ver si los topics llegan
ros2 topic list | grep fmu

# Ver posición en vivo
ros2 topic echo /fmu/out/vehicle_odometry

# Despegar manual desde consola PX4 (T1)
commander takeoff
```

---

## Conexiones
- [[Pipeline de Desarrollo de Drones]] — Fase 3 (SITL) en contexto
- [[PX4]] — qué hace PX4 en T1 y T3
- [[Gazebo]] — simulador levantado en T1
- [[Comunicación ROS2-PX4]] — qué es el Micro XRCE-DDS Agent (T2)
- [[RViz2]] — qué se visualiza en T4
- [[Ejecutar un Nodo ROS2]] — cómo está hecho el nodo de T3
