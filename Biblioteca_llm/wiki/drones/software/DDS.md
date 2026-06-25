---
tipo: concepto
tags: [ROS2, DDS, arquitectura, uORB, microXRCE-DDS]
fuentes: [raw/ros2-dds-concepto.md]
actualizado: 2026-06-18
---

# DDS

Cuando dos nodos ROS2 se comunican, no lo hacen directamente: hay una capa invisible llamada **DDS** que maneja descubrimiento, conexión, serialización y entrega de mensajes.

---

## El problema que resuelve

ROS1 usaba un servidor central (`roscore`) que coordinaba a todos los nodos. Si ese servidor caía, todo el sistema caía con él — inaceptable para un dron volando.

**DDS** (Data Distribution Service) es un estándar internacional de comunicación distribuida (también usado en aviación, medicina, defensa) que ROS2 adoptó porque **elimina el servidor central**.

Su característica clave es el **descubrimiento automático**: cuando un nodo arranca, anuncia su existencia a la red y los demás nodos se conectan solos — sin servidor intermediario, sin IPs hardcodeadas.

---

## Qué hace DDS por debajo

- **Descubrimiento** — detecta qué nodos existen y qué topics publican o escuchan, y los conecta automáticamente.
- **Serialización** — convierte los mensajes (objetos Python/C++) en bytes para la red, y los reconstruye igual al otro lado.
- **Entrega con garantías (QoS)** — configurable: entregar todos los mensajes aunque lleguen tarde, entregar solo el último, o guardar mensajes para subscribers que se conecten después.

> En ROS2 Jazzy, el DDS por defecto es **Fast-DDS** (eProsima). Existen otras implementaciones como Cyclone DDS — intercambiables, funcionan igual desde el código.

---

## Dónde vive en el código

No se interactúa con DDS directamente. La capa intermedia es **rclpy** (Python) o **rclcpp** (C++):

```
tu código Python
      ↓
   rclpy          ← lo que importas y usas
      ↓
   DDS            ← trabaja invisible por debajo
      ↓
   red / cable
```

`self.create_publisher(...)` le dice a rclpy, que le dice a DDS "quiero publicar en este topic". DDS hace el resto.

---

## El caso especial: microXRCE-DDS

DDS necesita un sistema operativo completo — no corre en un microcontrolador como el Pixhawk de [[PX4]]. La solución:

- **Micro XRCE-DDS Client** — versión ultraliviana del protocolo, vive dentro de PX4.
- **Micro XRCE-DDS Agent** — proceso en Ubuntu que hace de puente entre el DDS normal y el cliente liviano.

```
tus nodos ROS2 → DDS (Fast-DDS, Ubuntu) → XRCE Agent (Ubuntu) → XRCE Client (Pixhawk) → uORB → Flight stack → motores
```

> El XRCE Agent no es opcional: sin él, ROS2 y PX4 son mundos aislados aunque estén conectados por cable. Si el agente se cae, el dron pierde offboard aunque el código siga corriendo perfecto.

---

## uORB: el bus interno de PX4

Dentro de PX4 existe otro sistema de mensajes independiente, **uORB** — el equivalente a DDS pero solo interno, usado entre módulos de PX4 (EKF, controlador de actitud, mixer). El XRCE Client traduce entre DDS (afuera) y uORB (adentro).

---

## Por qué esto explica comportamientos concretos

- Si matas el XRCE Agent, el dron pierde offboard aunque el nodo siga corriendo — el puente entre los dos mundos desapareció.
- Si publicas setpoints más lento que 2 Hz, PX4 sale de OFFBOARD automáticamente — uORB tiene un timeout y detecta que dejaron de llegar datos.
- Si hay latencia alta en el cable UART, los comandos llegan tarde aunque el código sea perfecto — el cuello de botella está en la capa física, no en ROS2.

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

## Conexiones
- [[Conceptos Básicos de ROS2]] — subtema padre
- [[Comunicación en ROS2]] — las primitivas (topic/service/action/parameter) que DDS transporta por debajo
- [[QoS]] — el detalle de las políticas de entrega que DDS expone y hace cumplir
- [[Comunicación ROS2-PX4]] — la cadena completa DDS → uORB → MAVLink en el stack del dron
- [[PX4]] — lado autopilot donde corren uORB y el XRCE Client
