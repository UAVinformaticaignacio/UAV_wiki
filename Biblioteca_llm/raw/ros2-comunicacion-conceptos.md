---
tags:
  - ros2
  - conceptos
  - topics
  - services
  - actions
  - parameters
  - a1
fecha: 2026-06-18
estado: 🌱
---

# ROS2 — Cómo se comunican los nodos

> **Idea central:** en ROS2 tu programa no es un bloque monolítico. Es un grafo de nodos independientes que se pasan mensajes. Hay 4 formas de hacerlo, cada una para un caso distinto.

---

## El modelo base: nodos y grafo

Un **nodo** es un proceso que hace una sola cosa bien.

```
[nodo_camara] → imagen → [nodo_yolo] → detección → [nodo_control]
```

Estos nodos no se llaman directamente entre sí. Se comunican por canales. Esos canales pueden ser de 4 tipos.

---

## Las 4 primitivas

### 1. Topic — flujo continuo

**Analogía:** una radio FM. La radio transmite todo el tiempo. Tú puedes sintonizarla o no — la radio no sabe cuántos la escuchan.

- El **publisher** transmite datos en un topic
- El **subscriber** se "sintoniza" a ese topic y recibe lo que llega
- El publisher no espera respuesta. El subscriber no pide nada. Los datos simplemente fluyen.

**Cuándo usarlo:** cuando los datos son continuos — sensor, imagen, posición, setpoint de vuelo.

**En el dron:**
```
nodo_offboard publica → /fmu/in/trajectory_setpoint → PX4 recibe y ejecuta
PX4 publica         → /fmu/out/vehicle_status       → nodo_offboard recibe y lee
```

> [!info] Añadido
> El callback del subscriber se ejecuta automáticamente cada vez que llega un mensaje. No tienes que preguntar "¿llegó algo?" en un loop — ROS2 lo llama solo.

---

### 2. Service — petición y respuesta

**Analogía:** llamar a un taxi. Tú llamas (request), el taxi confirma y viene (response). Hasta que no confirma, estás esperando.

- El **client** manda una petición y espera
- El **server** recibe, procesa, y responde
- La comunicación es **bloqueante** — el cliente no sigue hasta recibir respuesta

**Cuándo usarlo:** operaciones puntuales y rápidas — armar el dron, cambiar de modo, consultar si algo está listo.

**En el dron:**
```
tu nodo pide  → "armar el dron"  → PX4 responde → "armado OK" / "fallo"
```

> [!warning] Corrección
> Si el service tarda más de lo esperado (servidor lento o caído), tu nodo queda bloqueado esperando. Por eso **no** uses services para operaciones largas — para eso existen las actions.

---

### 3. Action — tarea larga con progreso

**Analogía:** pedir una pizza a domicilio. Mandas el pedido (goal), te van avisando el estado (feedback: "en preparación", "en camino"), y al final llega (result). Puedes cancelar en cualquier momento.

- Mandas un **goal** (objetivo)
- Recibes **feedback** mientras la tarea corre
- Al terminar recibes un **result**
- Puedes **cancelar** en cualquier momento

**Cuándo usarlo:** tareas largas que necesitan seguimiento — "ve al waypoint X", "inspecciona esta área", "ejecuta esta trayectoria".

**En el dron:**
```
goal     → "ve al waypoint A (10, 5, -3)"
feedback → "distancia restante: 8.2 m"
feedback → "distancia restante: 3.1 m"
result   → "waypoint alcanzado"
```

---

### 4. Parameter — configuración del nodo

**Analogía:** el control remoto del televisor. El televisor (nodo) es siempre el mismo. El control remoto te permite cambiar su comportamiento (volumen, canal, brillo) sin abrirlo y modificarlo por dentro.

- Un parámetro es una variable de configuración que vive **fuera del código**
- Se define en un archivo `.yaml`
- El nodo lo lee al arrancar, o puede actualizarse en vivo desde la terminal
- El parámetro no le habla a PX4 — le habla a **tu nodo**, y tu nodo es el que traduce eso en comandos

**Cuándo usarlo:** valores que dependen del contexto — velocidad máxima, thresholds, nombres de topics, frecuencias.

**En el dron — ejemplo concreto:**

Tienes el mismo `nodo_offboard.py` pero necesitas usarlo en tres contextos distintos:

| Contexto | velocidad_max | altura | lado_patron |
|---|---|---|---|
| Simulación | 5.0 m/s | 10 m | 20 m |
| Prueba en jaula | 0.3 m/s | 1.5 m | 2 m |
| Vuelo exterior | 2.0 m/s | 5 m | 10 m |

Con parámetros tienes **un solo código** y tres archivos yaml:

```
config/
  simulacion.yaml      ← velocidad_max: 5.0
  prueba_jaula.yaml    ← velocidad_max: 0.3
  vuelo_exterior.yaml  ← velocidad_max: 2.0
```

Al lanzar el nodo le pasas el archivo correspondiente. Sin tocar Python.

Además puedes cambiar un parámetro **en vivo** mientras el dron está volando:

```bash
ros2 param set /nodo_offboard velocidad_max 0.5
```

> [!info] Añadido
> El parámetro no le dice la velocidad al dron directamente. Le dice a tu nodo con qué valor operar. Tu nodo es el que usa ese valor para calcular los setpoints que manda a PX4.

---

## Resumen: cuándo usar cada una

| Situación | Primitiva |
|---|---|
| Publicar setpoints de velocidad a 20 Hz | Topic |
| Leer posición del dron continuamente | Topic (subscriber) |
| Armar / desarmar el dron | Service |
| "Ve al waypoint X y avísame el progreso" | Action |
| Configurar velocidad máxima según contexto | Parameter |

---

## Regla para elegir

1. ¿Necesito respuesta? → No: **Topic** / Sí: seguir
2. ¿La operación es rápida (< 1 segundo)? → Sí: **Service** / No: **Action**
3. ¿Es un valor de configuración, no un comando? → **Parameter**

---

## Preguntas abiertas

- ¿Cómo afecta el QoS (Quality of Service) a los topics en situaciones de pérdida de mensajes?
- ¿Cuándo conviene usar un lifecycle node en vez de un nodo normal?
- ¿Qué pasa si un subscriber no alcanza a procesar todos los mensajes que llegan?
- ¿Los parameters se pueden compartir entre nodos o cada nodo tiene los suyos?
