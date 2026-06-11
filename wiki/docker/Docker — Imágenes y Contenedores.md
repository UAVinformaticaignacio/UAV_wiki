---
tipo: concepto
tags: [docker, devops, imágenes, contenedores]
fuentes: [raw/Docker - Imágenes y Contenedores .md]
actualizado: 2026-05-29
---

# Docker — Imágenes y Contenedores

## Imágenes

### ¿Qué es una imagen?

Una imagen es una **plantilla estática e inmutable** que contiene todo lo necesario para correr una aplicación: el sistema operativo base, las dependencias, el código y la configuración.

Es el punto de partida. Por sí sola no hace nada, no corre ni consume recursos. Es simplemente un molde guardado en tu disco.

> **Analogía:** Una imagen es como el archivo `.iso` de un sistema operativo. Lo tienes guardado, pero hasta que no lo "instalas" o ejecutas, no pasa nada.

### ¿De dónde vienen las imágenes?

Las imágenes se descargan desde **Docker Hub** (`hub.docker.com`), que es el repositorio público oficial. Ahí hay imágenes listas para usar de casi cualquier cosa: Ubuntu, Python, Node, MySQL, Nginx, etc.

También puedes crear tus propias imágenes a partir de un `Dockerfile`.

### ¿Cómo se visualiza una imagen?

Cada imagen tiene:

| Campo | Descripción |
|---|---|
| `REPOSITORY` | Nombre de la imagen (ej. `ubuntu`, `python`) |
| `TAG` | Versión de la imagen (ej. `latest`, `3.11`, `slim`) |
| `IMAGE ID` | Identificador único de la imagen |
| `SIZE` | Cuánto pesa en disco |

### Comandos de imágenes

#### `docker pull`

Descarga una imagen desde Docker Hub a tu máquina.

```bash
docker pull <imagen>:<tag>
```

- `<imagen>` — nombre de la imagen que quieres descargar
- `:<tag>` — versión específica (opcional, si no se pone usa `latest`)

**Ejemplo:**

```bash
docker pull ubuntu
docker pull python:3.11
```

Docker busca la imagen en Docker Hub y la descarga. Solo se descarga una vez. La próxima vez que la uses, la toma directo del disco.

---

#### `docker images`

Muestra todas las imágenes descargadas en tu máquina.

```bash
docker images
```

No recibe parámetros obligatorios.

**Ejemplo de salida:**

```
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
ubuntu       latest    5a81c4b8502e   2 weeks ago    78.1MB
python       3.11      4c1e997385b8   3 weeks ago    1.01GB
```

Cada fila es una imagen disponible lista para usarse.

---

## Contenedores

### ¿Qué es un contenedor?

Un contenedor es una **imagen en ejecución**. Es la instancia viva y activa de una imagen. Cuando corres una imagen, Docker crea un contenedor a partir de ella.

A diferencia de la imagen (que es estática), el contenedor tiene su propio proceso, su propio sistema de archivos temporal y su propio estado. Cuando el contenedor se detiene, la imagen sigue intacta y sin cambios.

> **Analogía:** Si la imagen es el molde de una galleta, el contenedor es la galleta ya horneada. Puedes hacer 10 galletas del mismo molde, cada una independiente. Si una se rompe, el molde sigue ahí.

### ¿Cómo se visualiza un contenedor?

Cada contenedor tiene:

| Campo | Descripción |
|---|---|
| `CONTAINER ID` | Identificador único del contenedor |
| `IMAGE` | A partir de qué imagen fue creado |
| `COMMAND` | El comando que está ejecutando |
| `STATUS` | Si está corriendo, detenido, etc. |
| `NAMES` | Nombre asignado (Docker lo genera automáticamente si no se especifica) |

### Estados de un contenedor

Un contenedor puede estar en tres estados:

- **Running** — está corriendo activamente
- **Exited** — terminó su ejecución o fue detenido
- **Created** — fue creado pero nunca iniciado

### Comandos de contenedores

#### `docker run`

Crea y arranca un contenedor a partir de una imagen.

```bash
docker run <opciones> <imagen> <comando>
```

- `<opciones>` — flags que modifican el comportamiento (ver abajo)
- `<imagen>` — la imagen base para crear el contenedor
- `<comando>` — el comando a ejecutar dentro del contenedor (opcional)

Opciones más comunes:

| Opción | Descripción |
|---|---|
| `-it` | Modo interactivo, te mete dentro del contenedor |
| `-d` | Modo background, corre sin ocupar la terminal |
| `--name` | Asigna un nombre personalizado al contenedor |

**Ejemplo:**

```bash
docker run -it ubuntu bash
```

El prompt cambia a algo como `root@3f2a1b4c8d9e:/#`, eso indica que estás dentro del contenedor. Es un Ubuntu completamente aislado de tu sistema.

Desde adentro puedes explorar:

```bash
ls
cat /etc/os-release
```

Para salir:

```bash
exit
```

---

#### `docker ps`

Muestra los contenedores que están corriendo en este momento.

```bash
docker ps
```

**Ejemplo de salida:**

```
CONTAINER ID   IMAGE     COMMAND   CREATED         STATUS         NAMES
3f2a1b4c8d9e   ubuntu    "bash"    1 minute ago    Up 1 minute    eager_morse
```

Si no hay ningún contenedor activo, la lista aparece vacía.

---

#### `docker ps -a`

Muestra **todos** los contenedores, incluyendo los que están detenidos.

```bash
docker ps -a
```

- `-a` — flag que significa "all" (todos)

**Ejemplo de salida:**

```
CONTAINER ID   IMAGE     COMMAND   CREATED         STATUS                     NAMES
3f2a1b4c8d9e   ubuntu    "bash"    2 minutes ago   Exited (0) 1 minute ago    eager_morse
```

El `STATUS` muestra `Exited` porque saliste con `exit`. El contenedor sigue existiendo pero ya no está corriendo.

---

## Relación entre imagen y contenedor

```
Docker Hub
    │
    │  docker pull
    ▼
  Imagen  ──────────────────────────────────────────┐
  (plantilla,                                        │
   estática)                                         │ docker run
                                                     ▼
                                            Contenedor A (corriendo)
                                            Contenedor B (corriendo)
                                            Contenedor C (detenido)
```

Una sola imagen puede dar origen a múltiples contenedores. Cada contenedor es independiente: lo que pase en uno no afecta a los demás ni a la imagen original.

---

## Conexiones

- [[Docker]] — página principal: qué es Docker, conceptos clave, instalación
