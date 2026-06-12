---
tags:
  - drones
  - ROS2
  - PX4
  - SITL
  - quick-ref
fecha: 2026-06-12
estado: activo
---

# SITL Quick Start — 4 terminales

> Referencia rápida. Para detalles ver [[activacion-stack-sitl]].

---

## T1 — PX4 + Gazebo

```bash
cd ~/PX4-Autopilot
make px4_sitl gz_x500
```

---

## T2 — DDS Agent

```bash
MicroXRCEAgent udp4 -p 8888
```

---

## T3 — Nodo offboard

```bash
source /opt/ros/jazzy/setup.bash
source ~/ros2_ws/install/setup.bash
ros2 run drone_control offboard_node
```

---

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
