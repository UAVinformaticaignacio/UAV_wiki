---
tipo: concepto
tags: [ROS2, conceptos]
fuentes: []
actualizado: 2026-06-18
---

# Conceptos Básicos de ROS2

Las ideas fundamentales para entender cómo está hecho ROS2 por dentro: cómo se pasan mensajes los nodos, y la capa de transporte que hace eso posible sin un servidor central.

---

## Subtemas

| Subtema | Resumen |
|---|---|
| [[Comunicación en ROS2]] | Las 4 primitivas: topic, service, action, parameter — cuándo usar cada una |
| [[DDS]] | La capa invisible que descubre nodos, serializa y entrega mensajes; cómo llega hasta PX4 vía microXRCE-DDS y uORB |
| [[QoS]] | Las políticas (reliability, history, durability) que definen cómo DDS entrega cada mensaje, y por qué un mismatch deja nodos sin conectarse |

---

## Conexiones
- [[ROS2]] — tema padre
- [[Comunicación ROS2-PX4]] — caso aplicado de estos conceptos en el stack del dron
