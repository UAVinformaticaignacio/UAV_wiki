---
tags:
  - ros2
  - dds
  - conceptos
  - arquitectura
  - a1
fecha: 2026-06-18
estado: 🌱
---

# ROS2 — Cómo viajan los datos (DDS)

> **Idea central:** cuando dos nodos ROS2 se comunican, no lo hacen directamente. Hay una capa invisible llamada DDS que maneja todo — descubrimiento, conexión, serialización y entrega. Entender DDS es entender por qué ROS2 funciona como funciona.

---

## El problema que DDS resuelve

Imagina 10 nodos corriendo al mismo tiempo — uno lee la cámara, otro procesa imágenes, otro controla el dron, otro graba datos, otro visualiza. Todos necesitan hablar entre sí.

La solución naive es tener un **servidor central** que coordine todo — todos le preguntan a él quién existe y dónde está. ROS1 hacía exactamente esto con un proceso llamado `roscore`.

El problema: si ese servidor cae, todo el sistema cae con él. Para un robot de laboratorio es tolerable. Para un dron volando, es inaceptable.

**DDS resuelve esto eliminando el servidor central.**

---

## Qué es DDS

**DDS** (Data Distribution Service) es un estándar internacional de comunicación para sistemas distribuidos. No es exclusivo de ROS2 — se usa en aviación, medicina, defensa y vehículos autónomos.

ROS2 lo adoptó como su capa de comunicación porque resuelve exactamente los problemas que tenía ROS1.

La característica clave de DDS se llama **descubrimiento automático**: cuando un nodo arranca, anuncia su existencia a la red. Los demás nodos escuchan ese anuncio y se conectan solos. Sin servidor intermediario, sin configuración manual, sin IPs hardcodeadas.

Es como entrar a una sala donde todos se presentan solos al llegar — nadie necesita al organizador para saber quién es quién.

---

## Qué hace DDS por debajo

Cuando tú creas un publisher o subscriber en ROS2, DDS se encarga de tres cosas que nunca ves:

**Descubrimiento** — encuentra automáticamente qué nodos existen en la red y qué topics publican o escuchan. Si arrancas un nuevo nodo, DDS lo detecta y lo conecta solo con quien corresponde.

**Serialización** — tus mensajes son objetos Python o C++ con campos y tipos. DDS los convierte en bytes para mandarlos por la red, y los reconstruye exactamente igual al otro lado. Tú nunca tocas este proceso.

**Entrega con garantías** — DDS puede configurarse para garantizar distintas cosas: entregar todos los mensajes aunque lleguen tarde, entregar solo el último y descartar los viejos, guardar mensajes para subscribers que se conecten después. Esto se llama QoS y es un tema propio.

> [!info] Añadido
> En ROS2 Jazzy, el DDS que viene por defecto es **Fast-DDS** (de eProsima). Existen otros como Cyclone DDS. Son distintas implementaciones del mismo estándar — intercambiables, funcionan igual desde el punto de vista de tu código.

---

## Dónde vive DDS en tu código

Tú no interactúas con DDS directamente. La capa entre tu código y DDS se llama **rclpy** (para Python) o **rclcpp** (para C++). Es la API de ROS2.

```
tu código Python
      ↓
   rclpy          ← lo que importas y usas
      ↓
   DDS            ← trabaja invisible por debajo
      ↓
   red / cable
```

Cuando escribes `self.create_publisher(...)`, rclpy le dice a DDS "quiero publicar en este topic". DDS hace el resto — busca quién escucha, establece la conexión, y entrega los mensajes. Todo automático.

---

## El caso especial: microXRCE-DDS

DDS es potente pero pesado. Necesita un sistema operativo completo y memoria suficiente. Corre bien en Linux o Windows, pero no puede correr en un microcontrolador con kilobytes de RAM.

Este es el problema con PX4, que corre en un Pixhawk (microcontrolador sin sistema operativo completo).

La solución tiene dos piezas:

**Micro XRCE-DDS Client** — una versión ultraliviana del protocolo que sí puede correr en microcontroladores. Vive dentro de PX4.

**Micro XRCE-DDS Agent** — un proceso que corre en tu computador Linux y hace de puente entre el DDS normal y el cliente liviano del microcontrolador.

```
tus nodos ROS2
      ↓
    DDS (Fast-DDS, Ubuntu)
      ↓
  XRCE Agent          ← puente traductor, corre en tu Ubuntu
      ↓  UART / USB / UDP
  XRCE Client         ← dentro de PX4, en el Pixhawk
      ↓
   uORB               ← bus interno de mensajes de PX4
      ↓
  Flight stack        ← controladores, estimador, mixer
      ↓
  ESC + motores
```

> [!warning] Corrección
> El XRCE Agent no es opcional. Sin él, tus nodos ROS2 y PX4 son mundos completamente aislados aunque estén conectados físicamente por cable. Si el agente se cae, el dron pierde offboard aunque tu código siga corriendo perfecto.

---

## uORB: el bus interno de PX4

Dentro de PX4 existe otro sistema de mensajes independiente llamado **uORB**. Es el equivalente a DDS pero solo para uso interno — los módulos de PX4 (estimador EKF, controlador de actitud, mixer de motores) se comunican entre sí por uORB.

Cuando el XRCE Client recibe un setpoint desde el agente, lo convierte a un mensaje uORB y lo publica internamente. Al revés, cuando PX4 quiere enviarte el estado del vehículo, el cliente toma el mensaje uORB y lo pasa al agente para que llegue a tus nodos.

---

## El viaje completo de un mensaje

Cuando tu nodo publica un setpoint de velocidad, esto es lo que ocurre en milisegundos:

```
1. tu código llama publish(setpoint)
2. rclpy pasa el mensaje a DDS
3. DDS serializa el objeto a bytes
4. DDS lo manda por UDP al XRCE Agent
5. El agente traduce y lo manda por UART/USB al Pixhawk
6. El XRCE Client lo recibe y publica en uORB
7. El controlador de posición lee el setpoint
8. El mixer calcula las señales para cada motor
9. Los ESC mueven los motores
```

---

## Por qué esto explica comportamientos concretos

Con este modelo mental, ciertos comportamientos que parecen raros tienen sentido inmediato:

Si matas el XRCE Agent, el dron pierde offboard aunque tu nodo siga corriendo — el puente entre los dos mundos desapareció.

Si publicas setpoints más lento que 2 Hz, PX4 sale de OFFBOARD automáticamente — uORB tiene un timeout y detecta que dejaron de llegar datos.

Si hay latencia alta en el cable UART, los comandos llegan tarde aunque tu código sea perfecto — el cuello de botella está en la capa física, no en ROS2 ni en tu lógica.

---

## Resumen

| Componente | Dónde vive | Qué hace |
|---|---|---|
| rclpy / rclcpp | Ubuntu | API de ROS2, lo que tú usas |
| DDS (Fast-DDS) | Ubuntu | comunicación, descubrimiento, serialización |
| XRCE Agent | Ubuntu | puente entre DDS y el microcontrolador |
| XRCE Client | Pixhawk | versión liviana de DDS para microcontrolador |
| uORB | Pixhawk | bus interno de mensajes de PX4 |

---

## Preguntas abiertas

- ¿Qué pasa exactamente cuando dos nodos están en máquinas distintas en la misma red? ¿DDS los conecta igual?
- ¿Qué es QoS y cómo afecta la entrega de mensajes entre nodos?
- ¿Por qué PX4 usa uORB en vez de DDS directamente para su comunicación interna?
- ¿Se puede reemplazar Fast-DDS por Cyclone DDS y qué cambia en la práctica?
