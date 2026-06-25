---
tags:
  - ros2
  - dds
  - fundamentos
  - dron
fecha: 2026-06-24
---

# QoS (Quality of Service) en ROS2

## Definición

**QoS = Quality of Service** (Calidad de Servicio). Es el conjunto de políticas que definen **cómo se entrega un mensaje** entre un [[publisher]] y un [[subscriber]] en [[ROS2]] — no *qué* dato se envía, sino *con qué garantías* se envía.

Estas políticas vienen de [[DDS]] (Data Distribution Service), el protocolo de comunicación sobre el que está construido ROS2. Es una diferencia clave frente a ROS1, que no exponía este nivel de control: en ROS1 la comunicación era básicamente TCP, sin opción de elegir el trade-off.

> [!info] Añadido
> La regla más importante de todo el sistema: **publisher y subscriber solo logran comunicarse (hacer "discovery") si sus políticas de QoS son compatibles entre sí.** Si no lo son, no hay error visible — simplemente nunca se conectan. Esto es la causa más común de "mi nodo no recibe nada de PX4 y no sé por qué".

## Qué problema resuelve

Pensar en ROS2 como un sistema postal entre nodos: la pregunta que QoS responde es *qué tipo de servicio postal quieres*. ¿Carta certificada que garantiza entrega aunque tarde, o un boletín que se reparte rápido y, si se pierde una copia, no importa porque viene otra en segundos?

Cada caso de uso necesita un trade-off distinto entre **confiabilidad** y **latencia/recursos**. QoS te da control fino sobre eso, tópico por tópico.

## Los 3 niveles principales

### 1. Reliability (confiabilidad)

- `RELIABLE`: el publisher reintenta hasta confirmar que el subscriber recibió el mensaje. Como un acuse de recibo.
- `BEST_EFFORT`: se envía y no se espera confirmación. Si se pierde, se pierde.

No siempre conviene `RELIABLE`. Un stream de cámara a 30 FPS no necesita retransmitir un frame perdido — para cuando llegara, ya sería viejo, y reintentar consume ancho de banda que se necesita para el frame *siguiente*. Por eso:
- Sensores de alta frecuencia (cámaras, a veces IMU) → suelen ir en `BEST_EFFORT`.
- Comandos críticos (armar el dron, cambiar de modo, abortar misión) → deben ir en `RELIABLE`.

### 2. History + Depth (cuántos mensajes guardar)

- `KEEP_LAST` con profundidad N: el publisher guarda solo los últimos N mensajes en su buffer.
- `KEEP_ALL`: guarda todo (hasta el límite de recursos).

Para algo como `VehicleLocalPosition` (la posición del dron actualizándose constantemente), conviene `KEEP_LAST` con depth bajo (1 o 5): solo importa el dato *más reciente*. Guardar mensajes viejos de posición no solo es inútil — es peligroso si un nodo de control llega a leer un dato desactualizado por error.

### 3. Durability (persistencia)

- `VOLATILE`: si no hay subscriber activo en el momento de publicar, el mensaje se pierde para siempre.
- `TRANSIENT_LOCAL`: el publisher guarda mensajes recientes para nodos que se suscriban *después* de que se publicaron.

Útil para mensajes de configuración o estado que un nodo nuevo necesita conocer aunque se haya conectado tarde (ej. un parámetro fijo, un mapa estático).

## Cómo encaja en el rompecabezas

QoS no es un detalle de implementación menor — es la capa que decide **qué tan honesto es el sistema sobre la realidad física** del dron. Decisiones mal tomadas aquí pueden:

- Hacer que un nodo nunca se conecte con PX4 (mismatch de QoS, sin error explícito — solo silencio).
- Entregar un setpoint de velocidad "viejo" por guardar historial innecesario.
- Perder un comando crítico de abort porque iba en `BEST_EFFORT` cuando debía ir en `RELIABLE`.

Conecta directo con [[failsafes]] y [[kill switch]]: esas medidas de seguridad solo funcionan si el mensaje que las dispara realmente llega a tiempo. QoS es la pieza silenciosa que garantiza (o rompe) esa llegada.

También es la explicación formal de un síntoma ya vivido: el offboard "perdiendo mensajes" con PX4 a través del puente [[microXRCE-DDS]] — muy probablemente un mismatch entre la política `BEST_EFFORT` que usa PX4 para telemetría de alta frecuencia y una expectativa `RELIABLE` del lado del nodo ROS2.
