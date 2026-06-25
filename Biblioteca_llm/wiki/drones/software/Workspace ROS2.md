---
tipo: concepto
tags: [ROS2, workspace, fundamentos]
fuentes: [raw/workspace_ros2_v3.md]
actualizado: 2026-06-24
---

# Workspace ROS2

Un **workspace** es la carpeta donde viven todos tus paquetes de ROS2: el lugar donde escribes, compilas y organizas tu código. No es nada mágico — es una carpeta con una estructura específica (`src/` adentro) que `colcon build` espera para poder encontrar y compilar tus paquetes. Crearlo es el paso obligatorio antes de crear cualquier paquete o nodo.

---

## Cómo crearlo

1. Crear la carpeta del workspace con una subcarpeta `src/` adentro (ahí van los paquetes):
   ```bash
   mkdir -p ~/Workspace/Programacion/mi_workspace/src
   ```
2. Entrar a la carpeta del workspace (no a `src/`):
   ```bash
   cd ~/Workspace/Programacion/mi_workspace
   ```
3. Instalar las dependencias del sistema que necesiten tus paquetes (hace falta cuando ya hay paquetes en `src/`, pero es buena costumbre dejarlo en el flujo):
   ```bash
   rosdep install --from-paths src --ignore-src -r -y
   ```
4. Compilar el workspace — aunque esté vacío, esto crea `build/`, `install/` y `log/`:
   ```bash
   colcon build
   ```
5. "Sourcear" el workspace para que el sistema sepa que existe:
   ```bash
   source install/setup.bash
   ```
6. (Opcional, recomendado) agregar ese `source` al `.bashrc` para no repetirlo en cada terminal nueva:
   ```bash
   echo "source ~/Workspace/Programacion/mi_workspace/install/setup.bash" >> ~/.bashrc
   ```

> [!info]
> `rosdep init` / `rosdep update` solo hace falta correrlo **una vez por máquina**, no por workspace. Si ya se hizo al instalar ROS2, no hay que repetirlo.

---

## Estructura resultante

```
mi_workspace/
├── build/      (archivos temporales de compilación)
├── install/    (lo "instalado" y listo para usar)
├── log/        (registros de compilación)
└── src/        (única carpeta que se toca a mano — aquí van los paquetes)
```

---

## El ciclo que se repite siempre

`colcon build` se ejecuta cada vez que:
- se crea un paquete nuevo,
- se modifica un nodo en C++ (en Python no es obligatorio, pero es buena costumbre igual),
- se agregan dependencias nuevas.

Y después de cada `colcon build` hay que volver a `source install/setup.bash` (o abrir una terminal nueva si ya está en el `.bashrc`) para que los cambios se reconozcan:

```bash
cd ~/Workspace/Programacion/mi_workspace
colcon build
source install/setup.bash
```

---

## Conexiones
- [[ROS2 — Inicio]] — subtema padre
- [[Ejecutar un Nodo ROS2]] — una vez creado el workspace, el siguiente paso es crear el paquete y el nodo dentro de él
- [[ROS2]] — tema general
