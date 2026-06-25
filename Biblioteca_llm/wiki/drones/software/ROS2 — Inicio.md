---
tipo: concepto
tags: [ROS2, fundamentos]
fuentes: []
actualizado: 2026-06-24
---

# ROS2 — Inicio

La ruta práctica para empezar a trabajar en ROS2: primero crear el **workspace** donde van a vivir tus paquetes, y después crear un **paquete con un nodo** y ejecutarlo. Estos dos pasos son la base sobre la que se construye todo lo demás (publishers, subscribers, services, actions, parameters).

---

## Subtemas

| Subtema | Resumen |
|---|---|
| [[Workspace ROS2]] | Crear la carpeta de trabajo (`src/`, `colcon build`, `source install/setup.bash`) antes de poder crear cualquier paquete |
| [[Ejecutar un Nodo ROS2]] | Crear el paquete, el archivo del nodo, registrarlo en `setup.py`, compilar y ejecutarlo con `ros2 run` |

---

## Conexiones
- [[ROS2]] — tema padre
- [[Conceptos Básicos de ROS2]] — una vez que el nodo corre, estas son las primitivas que usa para comunicarse
