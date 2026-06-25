# 🚁 Roadmap ROS 2 → Drones autónomos

> Meta: programar drones autónomos con misiones de waypoints (yo le indico a dónde ir).
> Estrategia: **software primero**, dominar todo en simulación antes de tocar hardware.

---

## ⚙️ Bases de ROS 2

- [ ] Workspaces y paquetes (colcon)
- [ ] Nodos, topics, publishers y subscribers
- [ ] Services
- [ ] Parámetros y launch files
- [ ] rclpy (todo en Python)

📚 Fuente: [docs.ros.org/en/jazzy](https://docs.ros.org/en/jazzy/) → Beginner: CLI tools + Client libraries

---

## 🔧 ROS 2 intermedio

- [ ] Mensajes / interfaces propias (msg, srv)
- [ ] Actions
- [ ] **QoS** ← clave para que PX4 te entregue datos
- [ ] **tf2 y transforms** (frames)
- [ ] Herramientas de debug: rqt, ros2 bag, RViz2

📚 Fuente: docs.ros.org/en/jazzy (How-to guides) + canal **Articulated Robotics** (YouTube)

---

## 🛩️ El mundo PX4

- [ ] Cómo funciona PX4 por dentro (commander, navigator, EKF2)
- [ ] El puente uXRCE-DDS y px4_msgs
- [ ] **Frames ENU (ROS) vs NED (PX4)** ← te va a morder si lo saltas
- [ ] Control offboard a fondo (regla de los 2 Hz / timeout 500 ms)

📚 Fuente: [docs.px4.io](https://docs.px4.io/main/en/ros2/user_guide) → ROS 2 User Guide + uXRCE-DDS Bridge

---

## 🚀 Subir de nivel

- [ ] PX4 ROS 2 Interface Library (modos de vuelo escritos en ROS 2)
- [ ] Conceptos de Nav2 (waypoints, GPS, behavior trees) — solo entenderlos
- [ ] **Aerostack2** (framework de misiones — instalar en Humble vía Docker)

📚 Fuente: docs.px4.io (Interface Library) + [aerostack2.github.io](https://aerostack2.github.io/)

---

## 🎯 El objetivo

- [ ] **Misión autónoma multi-waypoint completa en simulación** (takeoff → 4+ waypoints → RTL/land)
- [ ] Despliegue en Jetson + hardware (solo cuando la misión en SITL sea repetible)

---

## 📌 Notas para mí

- ✅ **Ya tengo avanzado:** bases 1–5 (parcial) y el control offboard (mi `nodo_offboard.py`).
- ⚠️ **Mis huecos reales:** QoS, tf2 y frames ENU/NED. Si domino esos 3, el salto a Aerostack2 se vuelve fácil.
- 🐳 **Versiones:** simulo en **Jazzy** (Ubuntu 24.04), pero PX4 recomienda y Aerostack2 solo tiene binarios para **Humble**. Crear un contenedor Docker Humble desde temprano (sirve también para el Jetson).
- 🔒 **Orden de abstracción:** offboard crudo → PX4 ROS 2 Interface Library → Aerostack2. No saltarse pasos.

---

### Progreso

- [ ] Bases de ROS 2
- [ ] ROS 2 intermedio
- [ ] El mundo PX4
- [ ] Subir de nivel
- [ ] Objetivo: misión autónoma en sim
- [ ] Hardware (Jetson)
