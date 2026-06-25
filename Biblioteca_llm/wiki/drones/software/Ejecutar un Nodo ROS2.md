

## Requisitos

Para ejecutar un nodo en ROS2 necesitas dos cosas configuradas. La primera es un archivo Python que contiene tu código y será el nodo que se ejecuta. La segunda es una entrada en el archivo setup.py que registra ese nodo en ROS2 para que pueda ser ejecutado.

1. **Archivo Python del nodo** → tu código
2. **Entrada en setup.py** → le dice a ROS2 dónde está el nodo

---

## Archivo 1: El Nodo (Python)

Este archivo contiene tu código. Va en la carpeta interna del paquete (dentro de una carpeta con el mismo nombre que el paquete).

**Ubicación:**

```
~/workspace/src/nombre_paquete/nombre_paquete/nombre_nodo.py
```

El archivo define una clase que hereda de Node. Esta clase tiene un constructor que inicializa el nodo, crea un publicador para enviar mensajes y un temporizador que ejecuta una función cada cierto tiempo. La función publica un mensaje cada vez que se ejecuta.

---

## Archivo 2: Configuración en setup.py

Este archivo le dice a ROS2 cómo ejecutar tu nodo.

**Ubicación:**

```
~/workspace/src/nombre_paquete/setup.py
```

Dentro hay una sección llamada `entry_points` que es donde registras tu nodo. Debes agregar una línea que especifica qué función ejecutar cuando el usuario escriba un comando. El formato es:

```
'nombre_comando = nombre_paquete.nombre_archivo:nombre_funcion'
```

---

## Pasos para Ejecutar

Una vez que tienes los dos archivos listos, sigues estos pasos en orden.

### 1. Navega al Workspace

```bash
cd ~/workspace
```

Te posicionas en la carpeta raíz de tu proyecto.

### 2. Compila el Código

```bash
colcon build
```

ROS2 lee tu código y lo prepara para ejecutar. Procesa el setup.py, crea los ejecutables y los guarda en carpetas internas.

### 3. Carga el Workspace en la Terminal

```bash
source install/setup.bash
```

Este comando le dice a la terminal dónde está tu código compilado. Sin este paso, ROS2 no encuentra tu paquete. Debes hacerlo en cada terminal nueva.

### 4. Ejecuta tu Nodo

```bash
ros2 run nombre_paquete nombre_comando
```

Aquí lanzas el nodo. Debería empezar a ejecutarse.

### 5. Detén el Nodo

```
Ctrl + C
```

Presiona estas teclas para detener la ejecución.

---

## Ejemplo Completo: Paso a Paso

Este es el flujo exacto que seguiste hoy, con los detalles específicos.

### Paso 1: Crear el Paquete

```bash
cd ~/Workspace/Programacion/ros2_ws/src
ros2 pkg create primer --build-type ament_python --dependencies rclpy std_msgs
```

ROS2 crea una carpeta `primer/` con toda la estructura lista.

### Paso 2: Crear el Archivo del Nodo

Abre tu editor y crea un archivo en:

```
~/Workspace/Programacion/ros2_ws/src/primer/primer/hola_mundo.py
```

Copia este código:

```python
import rclpy
from rclpy.node import Node
from std_msgs.msg import String

class HolaMundoPublisher(Node):
    def __init__(self):
        super().__init__('hola_mundo_nodo')
        self.publisher_ = self.create_publisher(String, '/hola_mundo', 10)
        self.timer = self.create_timer(1.0, self.publicar)

    def publicar(self):
        msg = String()
        msg.data = 'Hola Mundo'
        self.publisher_.publish(msg)
        self.get_logger().info(f'Publicando: {msg.data}')

def main(args=None):
    rclpy.init(args=args)
    nodo = HolaMundoPublisher()
    rclpy.spin(nodo)
    nodo.destroy_node()
    rclpy.shutdown()

if __name__ == '__main__':
    main()
```

**Guarda el archivo.**

### Paso 3: Modificar setup.py

Abre:

```
~/Workspace/Programacion/ros2_ws/src/primer/setup.py
```

Busca la sección `entry_points`:

```python
entry_points={
    'console_scripts': [
    ],
},
```

Reemplázala con:

```python
entry_points={
    'console_scripts': [
        'hola_mundo = primer.hola_mundo:main',
    ],
},
```

**Guarda el archivo.**

### Paso 4: Compilar

Desde la raíz del workspace:

```bash
cd ~/Workspace/Programacion/ros2_ws
colcon build
```

Debería terminar con:

```
Finished <<< primer
Summary: 1 package finished
```

### Paso 5: Limpiar si Hay Errores

Si algo falla, elimina las compilaciones anteriores y recompila:

```bash
rm -rf build install log
colcon build
```

### Paso 6: Cargar el Workspace

```bash
source install/setup.bash
```

Ahora la terminal sabe dónde está tu código.

### Paso 7: Ejecutar el Nodo

```bash
ros2 run primer hola_mundo
```

Debería ver:

```
[INFO] [hola_mundo_nodo]: Publicando: Hola Mundo
[INFO] [hola_mundo_nodo]: Publicando: Hola Mundo
[INFO] [hola_mundo_nodo]: Publicando: Hola Mundo
...
```

El nodo está corriendo y publicando mensajes cada segundo.

### Paso 8: Ver los Mensajes Publicados (Otra Terminal)

Abre otra terminal y ejecuta:

```bash
cd ~/Workspace/Programacion/ros2_ws
source install/setup.bash
ros2 topic echo /hola_mundo
```

Verá:

```
data: Hola Mundo
---
data: Hola Mundo
---
data: Hola Mundo
---
```

Esto muestra cada mensaje que publica el nodo en el tema `/hola_mundo`.

### Paso 9: Detener

En cualquier terminal:

```
Ctrl + C
```

---

## Resumen Rápido

```bash
# Compilar
cd ~/workspace
colcon build

# Cargar
source install/setup.bash

# Ejecutar
ros2 run nombre_paquete nombre_comando
```

---

## Errores Comunes

| Error                   | Causa                                    | Solución                                              |
| ----------------------- | ---------------------------------------- | ----------------------------------------------------- |
| `Package not found`     | No cargaste el workspace                 | Ejecuta `source install/setup.bash`                   |
| `No executable found`   | El setup.py no tiene la entrada correcta | Revisa que `entry_points` sea correcto                |
| `ModuleNotFoundError`   | El archivo está en la carpeta equivocada | Verifica que está en `nombre_paquete/nombre_paquete/` |
| El nodo no muestra nada | No estás escuchando el tema              | Abre otra terminal y usa `ros2 topic echo /tema`      |

---

## Conexiones
- [[ROS2 — Inicio]] — subtema padre
- [[Workspace ROS2]] — paso previo: crear el workspace antes de poder crear el paquete
- [[ROS2]] — referencia general del framework
- [[Pipeline de Desarrollo de Drones]] — los nodos de la Fase 2 (offboard_control, mission_planner, etc.) se crean siguiendo este mismo proceso