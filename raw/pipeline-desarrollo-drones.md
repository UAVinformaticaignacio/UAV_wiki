---
tags:
  - drones
  - ROS2
  - PX4
  - pipeline
  - robotica
fecha: 2026-06-12
estado: en-progreso
---

# Pipeline de desarrollo de drones

> Del código a los cielos — cómo se construye un dron profesional desde cero.

---

## Software stack — qué hace cada pieza

```
┌─────────────┐
│  Tu código  │  ← Python/C++ (ROS2 nodes)
│   (ROS2)    │     Control, misiones, percepción
└──────┬──────┘
       │ Topics DDS
       ▼
┌─────────────┐     ┌─────────────┐
│    PX4      │     │   RViz2     │  ← Visualiza datos de ROS2
│  Autopilot  │     │  (monitor)  │     Posición, sensores, paths
└──────┬──────┘     └─────────────┘
       │ PWM / actuadores
       ▼
┌─────────────┐     ┌─────────────┐
│  Hardware   │  ←→ │   Gazebo    │  ← Simula el mundo (en SITL)
│   (Dron)    │     │ (simulador) │     Física, sensores, ambiente
└─────────────┘     └─────────────┘
```

---

### ROS2 — el cerebro pensante

| Qué hace | Ejemplo |
|----------|---------|
| Publicar waypoints | `TrajectorySetpoint` → ir a x=10, y=5 |
| Leer sensores | Suscribirse a `/fmu/out/sensor_combined` |
| Procesar cámara | Detectar objetos con YOLO |
| Planificar rutas | Calcular path evitando obstáculos |
| Tomar decisiones | "Batería baja → volver a casa" |

> [!info] Clave
> ROS2 **no** controla los motores directamente. Le dice a PX4 **a dónde ir**, y PX4 se encarga de cómo llegar.

---

### PX4 — el piloto automático

| Qué hace | Cómo |
|----------|------|
| Estabilizar el dron | Controladores PID (roll, pitch, yaw) |
| Ejecutar comandos | Recibe setpoints de ROS2 |
| Gestionar failsafes | Pérdida de GPS → aterrizar |
| Fusionar sensores | EKF2 (GPS + IMU + barómetro) |
| Controlar motores | Señales PWM a los ESCs |

> [!warning] Importante
> PX4 es el **único** que toca los motores. Nunca envías PWM directo desde ROS2.

---

### Gazebo — el mundo virtual

Simula **todo** lo que existiría en la realidad:

- 🌍 Física → gravedad, colisiones, aerodinámica
- 📡 Sensores → GPS, IMU, cámara, lidar (con ruido)
- 💨 Ambiente → viento, terreno, obstáculos
- 🚁 Dron → modelo 3D que responde a los motores

> [!info] Dato
> Gazebo **reemplaza al mundo real** durante desarrollo. Cuando vuelas de verdad, Gazebo desaparece.

---

### RViz2 — la pantalla de monitoreo

**No simula nada.** Solo visualiza datos de ROS2:

- Posición del dron en 3D
- Trayectorias (path seguido)
- Nube de puntos del lidar
- Frames TF (orientación)
- Imagen de la cámara

> [!tip] Analogía
> Gazebo = **el estadio** donde se juega el partido.
> RViz2 = **la tele** donde lo ves.
> Si apagas la tele, el partido sigue.

---

### Comunicación entre componentes

```
Tu código (ROS2)
    │
    │  px4_msgs (topics DDS)
    │
    ▼
Micro XRCE-DDS Agent  ←── bridge entre ROS2 y PX4
    │
    │  uORB (protocolo interno)
    │
    ▼
PX4 Autopilot
    │
    │  MAVLink (protocolo estándar)
    │
    ▼
QGroundControl  ←── planificación visual de misiones
```

| Protocolo | Entre qué | Para qué |
|-----------|-----------|----------|
| **DDS** (topics) | ROS2 ↔ PX4 | Comandos y telemetría en tiempo real |
| **uORB** | Interno PX4 | Comunicación entre módulos del firmware |
| **MAVLink** | PX4 ↔ QGC | Monitoreo, misiones, configuración |

---

## Pipeline de desarrollo — las 7 fases

### Fase 1 → Diseño y requisitos

Antes de todo, defines **qué va a hacer el dron**:

| Pregunta | Ejemplo |
|----------|---------|
| ¿Misión? | Inspección de paneles solares |
| ¿Payload? | Cámara térmica + RGB |
| ¿Autonomía? | 25 min de vuelo |
| ¿Rango? | 500m radio |
| ¿Regulación? | DGAC Chile, zona no restringida |

> [!warning] Error común
> Saltar esta fase y empezar a comprar piezas sin saber qué necesitas.

---

### Fase 2 → Desarrollo software (ROS2)

Escribes los nodos que controlan el dron:

```python
# Nodo básico de control offboard
class DroneControl(Node):
    def __init__(self):
        super().__init__('drone_control')
        # Publicar setpoints de posición
        self.pub = self.create_publisher(
            TrajectorySetpoint,
            '/fmu/in/trajectory_setpoint', 10
        )
        self.timer = self.create_timer(0.1, self.loop)

    def loop(self):
        msg = TrajectorySetpoint()
        msg.position = [0.0, 0.0, -5.0]  # 5m arriba (NED)
        self.pub.publish(msg)
```

**Nodos típicos que desarrollas:**

- `offboard_control` → enviar comandos de posición/velocidad
- `mission_planner` → secuencia de waypoints
- `obstacle_detector` → leer lidar y detectar obstáculos
- `camera_processor` → procesar imágenes con CV/YOLO

---

### Fase 3 → SITL (Software In The Loop)

> **Todo** corre en tu PC. Sin hardware.

```
Terminal 1:  make px4_sitl gz_x500      ← PX4 + Gazebo
Terminal 2:  MicroXRCEAgent udp4 -p 8888  ← Bridge
Terminal 3:  ros2 run mi_pkg mi_nodo      ← Tu código
Terminal 4:  rviz2                         ← Monitoreo
```

**Qué pruebas aquí:**

- ✅ Despegue y aterrizaje autónomo
- ✅ Navegación por waypoints
- ✅ Failsafes (pérdida de GPS, batería baja)
- ✅ Obstacle avoidance
- ✅ Misiones multi-dron

> [!info] Ventaja
> Si crasheas, reinicias. Costo = $0.

---

### Fase 4 → HITL (Hardware In The Loop)

```
Pixhawk real ←──serial──→ PC con Gazebo
   (firmware)              (sensores simulados)
```

**¿Qué cambia vs SITL?**

| SITL | HITL |
|------|------|
| PX4 corre como proceso en tu PC | PX4 corre en el **Pixhawk real** |
| Todo simulado | Hardware real + sensores simulados |
| Rápido de iterar | Valida tiempos de respuesta reales |

> [!tip] Para qué sirve
> Verificar que el firmware corre bien en el chip real, que la comunicación serial funciona, y que los tiempos de respuesta son correctos.

---

### Fase 5 → Integración hardware

Armas el dron físico:

```
         Propelas
            │
        ┌───┴───┐
        │ Motor │ ×4
        └───┬───┘
            │
        ┌───┴───┐
        │  ESC  │ ×4  ← Controlador de velocidad
        └───┬───┘
            │
     ┌──────┴──────┐
     │   Pixhawk   │  ← Controlador de vuelo (PX4)
     │  (FC + IMU  │
     │  + Baro)    │
     └──────┬──────┘
            │ serial (UART/USB)
     ┌──────┴──────┐
     │ Jetson Nano │  ← Companion computer (ROS2)
     │ (o RPi)     │
     └──────┬──────┘
            │
     ┌──────┴──────┐
     │  Sensores   │  ← GPS, cámara, lidar
     └─────────────┘
```

**Checklist de integración:**

- [ ] Calibrar acelerómetro y giroscopio
- [ ] Calibrar magnetómetro (compass)
- [ ] Verificar orientación del Pixhawk
- [ ] Configurar RC (control remoto)
- [ ] Configurar failsafes en QGroundControl
- [ ] Verificar conexión serial Pixhawk ↔ Jetson
- [ ] Test de motores (dirección y orden correctos)

---

### Fase 6 → Pruebas de campo

**Secuencia progresiva — nunca saltar pasos:**

```
Manual (RC)  →  Position Hold  →  Misión simple  →  Misión completa
   ↓                ↓                  ↓                  ↓
Estabilidad    GPS funciona      Waypoints OK       Autonomía total
```

| Prueba | Qué validas |
|--------|-------------|
| Hover manual | Estabilidad, vibraciones, PID básico |
| Position hold | GPS lock, EKF2, drift |
| RTL (Return to Launch) | Failsafe funciona |
| Misión con 3 waypoints | Navegación autónoma básica |
| Misión completa | Todo el pipeline end-to-end |

> [!warning] Seguridad
> Siempre tener **kill switch** en el RC. Siempre volar en zona despejada primero.

---

### Fase 7 → Despliegue y operación

El dron ya vuela misiones reales:

- **Pre-vuelo** → checklist, clima, batería, GPS lock
- **Misión** → ejecución autónoma monitoreada
- **Post-vuelo** → descargar logs, analizar datos
- **Mantenimiento** → revisar motores, propelas, frame

> [!tip] El mismo código
> El código ROS2 que escribiste en la Fase 2 es **exactamente** el que corre aquí. Solo cambió el hardware debajo.

---

## Resumen visual del pipeline

```
  DISEÑO          SOFTWARE         SIMULACIÓN
 ┌────────┐     ┌──────────┐     ┌───────────┐
 │Misión  │ ──→ │ ROS2     │ ──→ │ SITL      │
 │Payload │     │ Nodes    │     │ PX4+Gazebo│
 │Reqs    │     │ Python   │     │ $0 riesgo │
 └────────┘     └──────────┘     └─────┬─────┘
                                       │
  DEPLOY          CAMPO            HARDWARE
 ┌────────┐     ┌──────────┐     ┌───────────┐
 │Misiones│ ←── │ Pruebas  │ ←── │ Integrar  │
 │Reales  │     │ de vuelo │     │ Pixhawk   │
 │Logs    │     │ PID tune │     │ + Jetson  │
 └────────┘     └──────────┘     └───────────┘
```

---

> [!quote] Regla de oro
> **Nunca vueles código que no hayas probado en SITL.**
> Un crash en simulación cuesta un `make clean`. Un crash real cuesta el dron.
