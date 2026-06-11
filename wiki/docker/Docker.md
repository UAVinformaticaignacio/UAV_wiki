---
tipo: entidad
tags: [docker, devops, contenedores]
fuentes: [raw/Docker.md]
actualizado: 2026-05-29
---

# Docker

## ¿Qué es?

Docker es una herramienta que permite **empaquetar una aplicación junto con todo lo que necesita para funcionar** (código, dependencias, versión del lenguaje, configuración) dentro de una unidad llamada **contenedor**.

Un contenedor es aislado, liviano y se comporta exactamente igual sin importar en qué máquina se ejecute: tu laptop, el servidor de un compañero o producción en la nube.

> **Analogía:** Es como llevar la comida en un tupper ya preparado, en lugar de llevar los ingredientes sueltos y esperar que en destino tengan los mismos utensilios y especias.

---

## ¿Qué problema soluciona?

El problema clásico del desarrollo de software:

> *"En mi máquina funciona..."*

Antes de Docker, era común que una aplicación funcionara perfectamente en la computadora del desarrollador pero fallara en otro entorno porque:

- La versión de Python/Node/Java era distinta
- Faltaba una librería del sistema operativo
- Una variable de entorno estaba configurada diferente
- El sistema operativo era otro (Mac vs Linux vs Windows)

Docker elimina este problema porque **el entorno viaja junto con la aplicación**.

---

## Conceptos clave

| Concepto | Qué es |
|---|---|
| **Dockerfile** | Archivo de texto con instrucciones para construir la imagen. Es la "receta". |
| **Image (imagen)** | El resultado de construir el Dockerfile. Es una plantilla estática e inmutable. |
| **Container (contenedor)** | Una instancia en ejecución de una imagen. Puedes tener N contenedores de la misma imagen. |
| **Docker Engine** | El servicio que corre en tu máquina y gestiona los contenedores. |
| **Docker Hub** | Repositorio público de imágenes listas para usar (como GitHub pero para imágenes). |

### Flujo básico

```
Dockerfile  →  (docker build)  →  Imagen  →  (docker run)  →  Contenedor
```

---

## Diferencia con una Máquina Virtual

| | Máquina Virtual | Contenedor Docker |
|---|---|---|
| **Incluye** | Sistema operativo completo | Solo la app y sus dependencias |
| **Peso** | Gigabytes | Megabytes |
| **Tiempo de inicio** | Minutos | Segundos |
| **Aislamiento** | Total (kernel propio) | Proceso aislado (comparte el kernel del host) |

---

## Ejemplo práctico

### El escenario

Tienes una app en Python 3.11 que usa la librería `requests`. Quieres que cualquier persona del equipo pueda levantarla sin instalar nada manualmente.

### 1. Estructura del proyecto

```
mi-app/
├── app.py
├── requirements.txt
└── Dockerfile
```

### 2. El código (`app.py`)

```python
import requests

respuesta = requests.get("https://jsonplaceholder.typicode.com/todos/1")
print(respuesta.json())
```

### 3. Las dependencias (`requirements.txt`)

```
requests==2.31.0
```

### 4. El Dockerfile

```dockerfile
# Imagen base: Python 3.11 ya instalado
FROM python:3.11-slim

# Directorio de trabajo dentro del contenedor
WORKDIR /app

# Copiar e instalar dependencias
COPY requirements.txt .
RUN pip install -r requirements.txt

# Copiar el código
COPY app.py .

# Comando que se ejecuta al iniciar el contenedor
CMD ["python", "app.py"]
```

### 5. Construir y ejecutar

```bash
# Construir la imagen (solo la primera vez o cuando cambie el Dockerfile)
docker build -t mi-app .

# Ejecutar el contenedor
docker run mi-app
```

### Resultado

```json
{'userId': 1, 'id': 1, 'title': 'delectus aut autem', 'completed': False}
```

Cualquier persona con Docker instalado puede clonar el repo y ejecutar los mismos dos comandos. No importa su sistema operativo ni qué versión de Python tengan instalada.

---

## ¿Qué necesito para usar Docker?

Solo necesitas instalar **Docker Desktop**, que incluye todo: el motor de Docker, la interfaz gráfica y las herramientas de línea de comandos.

| Sistema operativo | Requisito previo |
|---|---|
| **Windows** | Windows 10/11 de 64 bits. Requiere activar WSL 2 (subsistema de Linux). |
| **Mac** | macOS 12 o superior. Hay versión para chip Intel y para Apple Silicon (M1/M2/M3). |
| **Linux** | Se instala Docker Engine directamente, sin interfaz gráfica. |

---

## Guía de instalación paso a paso

### Windows

**Paso 1 — Activar WSL 2**

Abrir PowerShell como administrador y ejecutar:

```powershell
wsl --install
```

Reiniciar la computadora cuando lo pida.

**Paso 2 — Instalar Docker Desktop**

Descargar el instalador desde docker.com, ejecutarlo y seguir el asistente. Asegurarse de que la opción "Use WSL 2 instead of Hyper-V" esté marcada.

**Paso 3 — Verificar la instalación**

```bash
docker --version
```

Si responde algo como `Docker version 26.x.x` la instalación fue exitosa.

---

### Mac

**Paso 1 — Descargar Docker Desktop**

En la página de descarga, elegir la versión correcta según el chip:
- "Mac with Apple Silicon" → para M1, M2, M3
- "Mac with Intel chip" → para procesadores Intel

**Paso 2 — Instalar**

Abrir el archivo `.dmg` descargado y arrastrar Docker a la carpeta Aplicaciones.

**Paso 3 — Iniciar Docker Desktop**

Abrir Docker desde Aplicaciones. La primera vez pedirá permisos del sistema, aceptar. En la barra de menú aparecerá el ícono de la ballena 🐳 cuando esté listo.

**Paso 4 — Verificar**

```bash
docker --version
```

---

### Linux (Ubuntu/Debian)

**Paso 1 — Instalar Docker Engine**

```bash
sudo apt-get update
sudo apt-get install docker.io -y
```

**Paso 2 — Iniciar el servicio**

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

Este último comando es para iniciar Docker al encender la PC.

**Paso 3 — Permitir usar Docker sin `sudo`**

```bash
sudo usermod -aG docker $USER
```

Cerrar sesión y volver a entrar para que el cambio tome efecto.

**Paso 4 — Verificar**

```bash
docker --version
```

---

## Probar que funciona

```bash
docker run hello-world
```

Docker descargará una imagen de prueba y mostrará un mensaje de bienvenida. Si aparece, Docker está instalado y funcionando correctamente.

---

## ¿Por qué se usa tanto?

- **Reproducibilidad:** El entorno es idéntico en desarrollo, testing y producción
- **Aislamiento:** Varias apps con versiones distintas de Python/Node coexisten sin conflictos
- **Escalabilidad:** Para tener más capacidad, se corren más contenedores de la misma imagen
- **Portabilidad:** La imagen corre en cualquier máquina con Docker, sin configuración adicional
- **Colaboración:** Compartir una imagen es compartir el entorno completo

---

## Inicio Rápido — Usar Docker sin que inicie con el PC

Por defecto Docker arranca solo al encender la computadora. Si no lo usas a diario, conviene desactivarlo y levantarlo solo cuando lo necesites.

### Desactivar el inicio automático

```bash
sudo systemctl disable docker
```

### Iniciar Docker

```bash
sudo systemctl start docker
```

### Detener Docker

```bash
sudo systemctl stop docker
```

---

## Módulos

| Página                               | Resumen                                                          |
| ------------------------------------ | ---------------------------------------------------------------- |
| [[Docker — Imágenes y Contenedores]] | Comandos `pull`, `images`, `run`, `ps`; estados de un contenedor |

---

## Conexiones

- [[Python]] — Docker se usa frecuentemente para empaquetar aplicaciones Python
