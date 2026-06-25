---
tipo: concepto
tags: [ROS2, QoS, DDS]
fuentes: [raw/qos-ros2.md]
actualizado: 2026-06-24
---

# QoS (Quality of Service) en ROS2

**QoS** es el conjunto de políticas que define **cómo se entrega un mensaje** entre un publisher y un subscriber en ROS2 — no *qué* dato se envía, sino *con qué garantías*. Estas políticas vienen de [[DDS]], la capa de transporte sobre la que está construido ROS2; es una diferencia clave frente a ROS1, que solo ofrecía TCP plano sin ese control.

> [!info] Regla más importante
> Publisher y subscriber solo logran conectarse ("discovery") si sus políticas de QoS son **compatibles entre sí**. Si no lo son, no hay error visible: simplemente nunca se conectan. Es la causa más común de "mi nodo no recibe nada y no sé por qué".

---

## Qué problema resuelve

Cada caso de uso necesita un trade-off distinto entre **confiabilidad** y **latencia/recursos**. QoS da control fino sobre eso, tópico por tópico — como elegir entre carta certificada (garantiza entrega aunque tarde) o un boletín que se reparte rápido y, si se pierde una copia, no importa porque viene otra en segundos.

---

## Los 3 niveles principales

### 1. Reliability (confiabilidad)

- `RELIABLE`: el publisher reintenta hasta confirmar que el subscriber recibió el mensaje.
- `BEST_EFFORT`: se envía sin esperar confirmación; si se pierde, se pierde.

No siempre conviene `RELIABLE`. Un stream de cámara a 30 FPS no necesita retransmitir un frame perdido — para cuando llegara ya sería viejo, y reintentar consume ancho de banda que se necesita para el frame siguiente.

- Sensores de alta frecuencia (cámaras, a veces IMU) → suelen ir en `BEST_EFFORT`.
- Comandos críticos (armar el dron, cambiar de modo, abortar misión) → deben ir en `RELIABLE`.

### 2. History + Depth (cuántos mensajes guardar)

- `KEEP_LAST` con profundidad N: el publisher guarda solo los últimos N mensajes.
- `KEEP_ALL`: guarda todo (hasta el límite de recursos).

Para algo como la posición del dron actualizándose constantemente conviene `KEEP_LAST` con depth bajo (1 o 5): solo importa el dato más reciente. Guardar mensajes viejos de posición no solo es inútil — es peligroso si un nodo de control llega a leer un dato desactualizado por error.

### 3. Durability (persistencia)

- `VOLATILE`: si no hay subscriber activo al momento de publicar, el mensaje se pierde para siempre.
- `TRANSIENT_LOCAL`: el publisher guarda mensajes recientes para nodos que se suscriban después de que se publicaron.

Útil para mensajes de configuración o estado que un nodo nuevo necesita conocer aunque se haya conectado tarde (un parámetro fijo, un mapa estático).

---

## Por qué no es un detalle menor

QoS es la capa que decide qué tan honesto es el sistema sobre la realidad física del dron. Decisiones mal tomadas aquí pueden:

- hacer que un nodo nunca se conecte con PX4 (mismatch de QoS, sin error explícito — solo silencio),
- entregar un setpoint de velocidad "viejo" por guardar historial innecesario,
- perder un comando crítico de abort porque iba en `BEST_EFFORT` cuando debía ir en `RELIABLE`.

Conecta directo con las medidas de seguridad tipo failsafe/kill switch: esas solo funcionan si el mensaje que las dispara realmente llega a tiempo. QoS es la pieza silenciosa que garantiza (o rompe) esa llegada.

También explica un síntoma típico de offboard "perdiendo mensajes" con PX4 a través de microXRCE-DDS: muy probablemente un mismatch entre la política `BEST_EFFORT` que usa PX4 para telemetría de alta frecuencia y una expectativa `RELIABLE` del lado del nodo ROS2.

---

## Conexiones
- [[Conceptos Básicos de ROS2]] — subtema padre
- [[DDS]] — la capa de transporte de donde vienen estas políticas
- [[Comunicación en ROS2]] — las primitivas (topics, sobre todo) cuyo comportamiento configura QoS
- [[Comunicación ROS2-PX4]] — caso aplicado: mismatches de QoS entre ROS2 y PX4 vía microXRCE-DDS
