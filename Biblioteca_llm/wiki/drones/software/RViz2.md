---
tipo: entidad
tags: [RViz2, ROS2, drones, visualizacion]
fuentes: [raw/pipeline-desarrollo-drones.md, raw/sitl-quick-start.md]
actualizado: 2026-06-12
---

# RViz2

**RViz2** es la herramienta de visualización de ROS2. No simula nada — solo muestra datos que ya existen como topics de ROS2.

---

## Qué muestra

- Posición del dron en 3D
- Trayectorias (path seguido)
- Nube de puntos del lidar
- Frames TF (orientación)
- Imagen de la cámara

> [!tip] Analogía
> [[Gazebo]] es el estadio donde se juega el partido; RViz2 es la tele donde se ve. Si apagas la tele, el partido sigue.

---

## Uso típico (SITL)

```bash
source /opt/ros/jazzy/setup.bash
rviz2 -d ~/drone_rviz.rviz
```

Ver [[SITL Quick Start]] (Terminal 4).

---

## Conexiones
- [[Software de Drones]] — rol de RViz2 dentro del stack
- [[Gazebo]] — simulador que genera los datos que RViz2 visualiza
- [[Pipeline de Desarrollo de Drones]] — rol de RViz2 en el stack
- [[Ejecutar un Nodo ROS2]] — los topics que publica un nodo pueden verse en RViz2
