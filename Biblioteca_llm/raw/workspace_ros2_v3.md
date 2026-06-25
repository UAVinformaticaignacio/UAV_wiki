---
tags: [ros2, fundamentos, robotica, workspace]
fecha: 2026-06-19
---

# Workspace ROS2

## ¿Qué es?

Un **workspace** (espacio de trabajo) es una carpeta donde viven todos tus paquetes de ROS2. Es el lugar donde escribes, compilas y organizas tu código.

No es algo mágico de ROS2: es simplemente una carpeta con una estructura específica que el sistema espera, para poder compilar y encontrar tus paquetes (`colcon build` los reconoce ahí).

## ¿Para qué sirve?

- Mantiene tu código organizado y separado del código de sistema.
- Te permite tener varios proyectos/paquetes en un solo lugar y compilarlos juntos.
- Es el punto de partida obligatorio antes de crear cualquier paquete o nodo.

## Cómo crearlo

1. Crea la carpeta del workspace, con una subcarpeta `src/` adentro (ahí van los paquetes)
   ```bash
   mkdir -p ~/Workspace/Programacion/mi_workspace/src
   ```
2. Entra a la carpeta del workspace (no a `src/`)
   ```bash
   cd ~/Workspace/Programacion/mi_workspace
   ```
3. Instala las dependencias del sistema que necesiten tus paquetes (solo hace falta cuando ya tienes paquetes en `src/`, pero es buena costumbre dejarlo en el flujo)
   ```bash
   rosdep install --from-paths src --ignore-src -r -y
   ```
4. Compila el workspace (aunque esté vacío, esto crea las carpetas `build/`, `install/`, `log/`)
   ```bash
   colcon build
   ```
5. "Sourcea" el workspace para que el sistema sepa que existe
   ```bash
   source install/setup.bash
   ```
6. (Opcional pero recomendado) Agrega ese `source` a tu `.bashrc` para no escribirlo cada vez que abres una terminal nueva
   ```bash
   echo "source ~/Workspace/Programacion/mi_workspace/install/setup.bash" >> ~/.bashrc
   ```

> [!info] Añadido
> `rosdep` solo necesita inicializarse **una vez por máquina** (no por workspace):
> ```bash
> sudo rosdep init
> rosdep update
> ```
> Si ya lo hiciste antes en tu sistema (por ejemplo al instalar ROS2), no es necesario repetirlo.

## Ejemplo

```bash
mkdir -p ~/Workspace/Programacion/aprendizaje_ros/src
cd ~/Workspace/Programacion/aprendizaje_ros
rosdep install --from-paths src --ignore-src -r -y
colcon build
source install/setup.bash
```

Resultado: queda una carpeta así:

```
aprendizaje_ros/
├── build/      (archivos temporales de compilación)
├── install/    (acá queda lo "instalado" y listo para usar)
├── log/        (registros de compilación)
└── src/        (AQUÍ creas tus paquetes — esta es la única carpeta que tocas a mano)
```

> [!warning] Paso repetitivo
> **`colcon build` se ejecuta SIEMPRE** que:
> - Creas un paquete nuevo
> - Modificas un nodo en C++ (en Python normalmente no es obligatorio, pero es buena costumbre igual)
> - Agregas dependencias nuevas
>
> Y después de cada `colcon build`, hay que volver a hacer `source install/setup.bash` (o abrir una terminal nueva si ya lo agregaste al `.bashrc`) para que los cambios se reconozcan.
>
> Flujo típico que vas a repetir mil veces:
> ```bash
> cd ~/Workspace/Programacion/mi_workspace
> colcon build
> source install/setup.bash
> ```

## Relacionado

- [[Paquete ROS2 (package)]]
- [[Nodo (ROS2)]]
