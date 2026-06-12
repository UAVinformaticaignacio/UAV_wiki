---
tipo: fuente
tags: [drones, ROS2, PX4, pipeline, robotica]
fuentes: [raw/pipeline-desarrollo-drones.md]
actualizado: 2026-06-12
---

# Pipeline de Desarrollo de Drones

Resumen del pipeline completo para construir un dron profesional: del diseño del software a la operación en campo. Cubre el stack (ROS2, PX4, Gazebo, RViz2) y las 7 fases de desarrollo.

---

## Stack de software — quién hace qué

| Componente | Rol |
|---|---|
| **ROS2** (tu código) | Cerebro: misiones, percepción, decisiones. Publica setpoints, no toca motores. |
| **PX4** | Piloto automático: estabiliza, ejecuta setpoints, gestiona failsafes, fusiona sensores (EKF2), controla motores vía PWM. |
| **Gazebo** | Simula el mundo (física, sensores, ambiente) durante SITL. |
| **RViz2** | Visualiza datos de ROS2 (posición, paths, sensores). No simula nada. |

> [!info] Regla clave
> ROS2 le dice a PX4 **a dónde ir**; PX4 decide **cómo llegar** y es el único que controla los motores.

Detalle de cada componente: [[PX4]], [[Gazebo]], [[RViz2]].

---

## Comunicación entre componentes

ROS2 habla con PX4 vía **DDS** (`px4_msgs`) a través del Micro XRCE-DDS Agent; PX4 usa **uORB** internamente y **MAVLink** para hablar con QGroundControl. Ver [[Comunicación ROS2-PX4]] para el detalle de protocolos.

---

## Las 7 fases del pipeline

### Fase 1 — Diseño y requisitos

Antes de comprar piezas, definir: misión (ej. inspección de paneles solares), payload (ej. cámara térmica + RGB), autonomía (ej. 25 min de vuelo), rango (ej. 500 m de radio) y regulación (ej. DGAC Chile, zona no restringida).

> [!warning] Error común
> Saltar esta fase y empezar a comprar piezas sin saber qué se necesita.

### Fase 2 — Desarrollo software (ROS2)

Se escriben los nodos que controlan el dron, típicamente:

- `offboard_control` → enviar comandos de posición/velocidad
- `mission_planner` → secuencia de waypoints
- `obstacle_detector` → leer lidar y detectar obstáculos
- `camera_processor` → procesar imágenes con CV/YOLO

Estos nodos publican setpoints (ej. `TrajectorySetpoint` al topic `/fmu/in/trajectory_setpoint`) que PX4 ejecuta. Ver [[Ejecutar un Nodo ROS2]] para cómo crear y correr un nodo.

### Fase 3 — SITL (Software In The Loop)

Todo corre en el PC, sin hardware: PX4 + Gazebo, el Micro XRCE-DDS Agent, el nodo propio y RViz2, en 4 terminales (ver [[SITL Quick Start]]).

Qué se prueba: despegue/aterrizaje autónomo, navegación por waypoints, failsafes (pérdida de GPS, batería baja), obstacle avoidance y misiones multi-dron.

> [!info] Ventaja
> Si crasheas, reinicias. Costo = $0.

### Fase 4 — HITL (Hardware In The Loop)

PX4 corre en un Pixhawk real conectado por serial al PC, que sigue simulando los sensores con Gazebo. Valida que el firmware funcione en el chip real y que los tiempos de respuesta sean correctos. Ver [[SITL y HITL]].

### Fase 5 — Integración hardware

Se ensambla el dron físico: propelas → motores → ESCs → **Pixhawk** (FC + IMU + Baro, corre PX4) → companion computer (**Jetson Nano** o RPi, corre ROS2) → sensores (GPS, cámara, lidar).

Checklist de integración:

- Calibrar acelerómetro y giroscopio
- Calibrar magnetómetro (compass)
- Verificar orientación del Pixhawk
- Configurar RC (control remoto)
- Configurar failsafes en QGroundControl
- Verificar conexión serial Pixhawk ↔ Jetson
- Test de motores (dirección y orden correctos)

### Fase 6 — Pruebas de campo

Secuencia progresiva, **nunca saltar pasos**:

```
Manual (RC)  →  Position Hold  →  Misión simple  →  Misión completa
   ↓                ↓                  ↓                  ↓
Estabilidad    GPS funciona      Waypoints OK       Autonomía total
```

| Prueba | Qué valida |
|---|---|
| Hover manual | Estabilidad, vibraciones, PID básico |
| Position hold | GPS lock, EKF2, drift |
| RTL (Return to Launch) | Failsafe funciona |
| Misión con 3 waypoints | Navegación autónoma básica |
| Misión completa | Todo el pipeline end-to-end |

> [!warning] Seguridad
> Siempre tener **kill switch** en el RC. Siempre volar en zona despejada primero.

### Fase 7 — Despliegue y operación

El dron ya vuela misiones reales: pre-vuelo (checklist, clima, batería, GPS lock) → misión autónoma monitoreada → post-vuelo (descargar logs, analizar datos) → mantenimiento (motores, propelas, frame).

> [!tip] El mismo código
> El código ROS2 escrito en la Fase 2 es **exactamente** el que corre aquí. Solo cambió el hardware debajo.

---

## Regla de oro

> **Nunca vueles código que no hayas probado en SITL.** Un crash en simulación cuesta un `make clean`. Un crash real cuesta el dron.

---

## Conexiones
- [[PX4]] — piloto automático: EKF2, failsafes, control de motores
- [[Gazebo]] — simulador usado en SITL y HITL
- [[RViz2]] — visualización de datos ROS2
- [[Comunicación ROS2-PX4]] — protocolos DDS, uORB, MAVLink
- [[SITL y HITL]] — diferencias y cuándo usar cada uno
- [[SITL Quick Start]] — referencia rápida de comandos para SITL (Fase 3)
- [[Ejecutar un Nodo ROS2]] — cómo crear los nodos de la Fase 2
- [[ROS2]] — referencia general del framework
