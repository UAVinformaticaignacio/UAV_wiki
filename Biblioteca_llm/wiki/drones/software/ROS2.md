---
tipo: concepto
tags: [ROS2, rclpy, robotica]
fuentes: []
actualizado: 2026-06-18
---

# ROS2

**ROS2** (Robot Operating System 2) es un framework para construir software de robots: middleware de comunicación (DDS) entre nodos, herramientas de desarrollo y un ecosistema de paquetes. No es un sistema operativo — es una capa que corre sobre Linux (u otro OS) y resuelve el problema de hacer que muchos programas independientes cooperen como si fueran un solo robot.

Un **nodo** es un proceso que realiza una tarea (publicar sensores, controlar motores, planificar rutas). Los nodos no se llaman directamente entre sí: viven en un grafo y se comunican por canales. **rclpy** es la biblioteca cliente de ROS2 para Python usada para escribirlos.

La forma de comunicación más básica y más usada es el modelo **publicación/suscripción** sobre **topics**:

- Un nodo **publisher** publica mensajes en un topic (un canal con nombre, ej. `/scan`, `/cmd_vel`) sin saber ni importarle quién los recibe.
- Uno o varios nodos **subscriber** se suscriben a ese topic y reciben automáticamente cada mensaje que llega, vía un callback — no hay que preguntar "¿llegó algo?" en un loop.
- Publisher y subscriber están **desacoplados**: no se conocen entre sí, pueden arrancar en cualquier orden, y puede haber 0, 1 o varios subscribers para el mismo topic.

Es ideal para datos continuos (sensores, posición, video) donde nadie necesita una respuesta. Además del pub/sub existen otras tres primitivas — service, action y parameter — para los casos en que sí hace falta una respuesta o un seguimiento; el detalle completo de las 4 está en [[Conceptos Básicos de ROS2]].

---

## Subtemas

| Subtema                       | Resumen                                                                                                           |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| [[Conceptos Básicos de ROS2]] | Cómo se comunican los nodos (topic/service/action/parameter), QoS y la capa DDS que lo hace posible               |
| [[ROS2 — Inicio]]             | Crear el workspace, el paquete y el nodo, registrarlo en `setup.py`, compilar con `colcon` y ejecutarlo           |

---

## Conexiones
- [[Software de Drones]] — rol de ROS2 dentro del stack de un dron
- [[Pipeline de Desarrollo de Drones]] — caso de uso completo: ROS2 + PX4 + Gazebo para drones
