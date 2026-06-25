---
tipo: concepto
tags: [ROS2, topics, services, actions, parameters]
fuentes: [raw/ros2-comunicacion-conceptos.md]
actualizado: 2026-06-18
---

# Comunicación en ROS2

Un programa ROS2 es un grafo de nodos independientes que se pasan mensajes — nunca se llaman directamente entre sí. Hay 4 primitivas de comunicación, cada una para un caso distinto.

---

## Las 4 primitivas

### Topic — flujo continuo

**Analogía:** una radio FM. El **publisher** transmite sin esperar respuesta; el **subscriber** se sintoniza y recibe lo que llega. El callback del subscriber se ejecuta automáticamente cada vez que llega un mensaje — ROS2 lo llama solo, no hay que preguntar "¿llegó algo?" en un loop.

**Cuándo usarlo:** datos continuos — sensor, imagen, posición, setpoint de vuelo.

**En el dron:**
```
nodo_offboard publica → /fmu/in/trajectory_setpoint → PX4 recibe y ejecuta
PX4 publica         → /fmu/out/vehicle_status       → nodo_offboard recibe y lee
```

---

### Service — petición y respuesta

**Analogía:** llamar a un taxi. El **client** manda una petición y espera; el **server** recibe, procesa y responde. La comunicación es **bloqueante**.

**Cuándo usarlo:** operaciones puntuales y rápidas — armar el dron, cambiar de modo, consultar si algo está listo.

> Si el service tarda más de lo esperado (servidor lento o caído), el nodo queda bloqueado esperando. Por eso no se usan services para operaciones largas — para eso existen las actions.

---

### Action — tarea larga con progreso

**Analogía:** pedir una pizza a domicilio. Se manda un **goal**, se recibe **feedback** mientras la tarea corre, y al final llega el **result**. Se puede **cancelar** en cualquier momento.

**Cuándo usarlo:** tareas largas que necesitan seguimiento — "ve al waypoint X", "inspecciona esta área", "ejecuta esta trayectoria".

**En el dron:**
```
goal     → "ve al waypoint A (10, 5, -3)"
feedback → "distancia restante: 8.2 m"
result   → "waypoint alcanzado"
```

---

### Parameter — configuración del nodo

**Analogía:** el control remoto del televisor. El nodo es siempre el mismo código; el parámetro cambia su comportamiento sin tocarlo por dentro.

- Vive **fuera del código**, en un archivo `.yaml`.
- El nodo lo lee al arrancar, o se puede actualizar en vivo desde la terminal (`ros2 param set ...`).
- El parámetro no le habla a PX4 — le habla a **tu nodo**, y tu nodo traduce ese valor en comandos.

**Cuándo usarlo:** valores que dependen del contexto — velocidad máxima, thresholds, nombres de topics, frecuencias. Permite un solo código fuente con distintos `.yaml` por contexto (simulación, prueba en jaula, vuelo exterior).

---

## Resumen: cuándo usar cada una

| Situación | Primitiva |
|---|---|
| Publicar setpoints de velocidad a 20 Hz | Topic |
| Leer posición del dron continuamente | Topic (subscriber) |
| Armar / desarmar el dron | Service |
| "Ve al waypoint X y avísame el progreso" | Action |
| Configurar velocidad máxima según contexto | Parameter |

## Regla para elegir

1. ¿Necesito respuesta? → No: **Topic** / Sí: seguir
2. ¿La operación es rápida (< 1 segundo)? → Sí: **Service** / No: **Action**
3. ¿Es un valor de configuración, no un comando? → **Parameter**

---

## Conexiones
- [[Conceptos Básicos de ROS2]] — subtema padre
- [[DDS]] — la capa que transporta estos mensajes por debajo, sin que el código la vea
- [[Comunicación ROS2-PX4]] — cómo estas primitivas (sobre todo topics) cruzan hacia PX4
- [[Ejecutar un Nodo ROS2]] — dónde se declaran publishers, subscribers y parameters en código
