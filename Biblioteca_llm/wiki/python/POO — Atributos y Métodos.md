---
tipo: concepto
tags: [python, POO, atributos, métodos]
fuentes: [raw/Modulo_02_Atributos_y_Metodos.md]
actualizado: 2026-05-20
---

# Módulo 2 — Atributos y Métodos

---

## ¿Qué son los Atributos?

Los atributos son los **datos** que tiene un objeto. Sus características.
Son variables que viven **dentro del objeto** y se acceden con el punto `.`.

```python
persona.nombre   # atributo nombre
persona.edad     # atributo edad
```

---

## ¿Qué son los Métodos?

Los métodos son las **acciones** que puede hacer un objeto.
Son funciones que viven **dentro de la clase** y también se acceden con el punto `.`.

```python
persona.saludar()      # método saludar
persona.cumpleanos()   # método cumpleanos
```

> **Diferencia visual:** Los métodos llevan paréntesis `()`, los atributos no.

---

## Atributos vs Métodos

|              | Atributo        | Método             |
| ------------ | --------------- | ------------------ |
| ¿Qué es?     | Un dato         | Una acción         |
| ¿Cómo se ve? | `objeto.nombre` | `objeto.saludar()` |
| ¿Lleva `()`? | No              | Sí                 |
| Ejemplo      | `auto.color`    | `auto.acelerar()`  |

---

## Ejemplo — Atributos

```python
class Persona:
    def __init__(self, nombre, edad):
        self.nombre = nombre    # atributo
        self.edad   = edad      # atributo


persona1 = Persona("Juan", 30)
persona2 = Persona("Maria", 25)

# Cada objeto tiene sus propios atributos
print(persona1.nombre)   # Juan
print(persona2.nombre)   # Maria
print(persona1.edad)     # 30
```

Cada objeto tiene **sus propios valores**, aunque comparten la misma clase.

---

## Ejemplo — Métodos

```python
class Persona:
    def __init__(self, nombre, edad):
        self.nombre = nombre
        self.edad   = edad

    def saludar(self):
        print(f"Hola, me llamo {self.nombre}")

    def cumpleanos(self):
        self.edad += 1
        print(f"{self.nombre} ahora tiene {self.edad} años")


persona1 = Persona("Juan", 30)

persona1.saludar()       # Hola, me llamo Juan
persona1.cumpleanos()    # Juan ahora tiene 31 años
print(persona1.edad)     # 31
```

El método `cumpleanos` **modifica el atributo** `edad` del propio objeto.

---

## Métodos con parámetros

Un método puede recibir datos externos además de `self`:

```python
class Persona:
    def __init__(self, nombre, edad):
        self.nombre = nombre
        self.edad   = edad

    def presentarse(self, otro_nombre):
        print(f"Hola {otro_nombre}, yo soy {self.nombre}")

    def sumar_años(self, cantidad):
        self.edad += cantidad
        print(f"{self.nombre} ahora tiene {self.edad} años")


p = Persona("Juan", 30)
p.presentarse("Maria")   # Hola Maria, yo soy Juan
p.sumar_años(5)          # Juan ahora tiene 35 años
```

---

## Ejemplo completo — todo junto

```python
class Vehiculo:
    def __init__(self, marca, color):
        self.marca      = marca     # atributo
        self.color      = color     # atributo
        self.velocidad  = 0         # atributo (siempre empieza en 0)

    def acelerar(self, cantidad):   # método con parámetro
        self.velocidad += cantidad
        print(f"{self.marca} acelera a {self.velocidad} km/h")

    def frenar(self, cantidad):     # método con parámetro
        self.velocidad -= cantidad
        print(f"{self.marca} reduce a {self.velocidad} km/h")

    def estado(self):               # método sin parámetro
        print(f"--- {self.marca} ({self.color}) | Velocidad: {self.velocidad} km/h ---")


auto = Vehiculo("Toyota", "Rojo")
auto.estado()        # --- Toyota (Rojo) | Velocidad: 0 km/h ---
auto.acelerar(60)    # Toyota acelera a 60 km/h
auto.acelerar(40)    # Toyota acelera a 100 km/h
auto.frenar(30)      # Toyota reduce a 70 km/h
auto.estado()        # --- Toyota (Rojo) | Velocidad: 70 km/h ---
```

---

## Resumen

```
Atributo  →  dato del objeto         →  self.nombre, self.edad
Método    →  acción del objeto       →  def saludar(self):
self      →  el objeto mismo         →  self.nombre = "Juan"
Punto .   →  accede a ambos         →  persona.nombre / persona.saludar()
```

---

## Mini ejercicio

Crea una clase `Jugador` con:
- Atributos: `nombre`, `puntaje` (inicia en 0), `vidas` (inicia en 3)
- Método `ganar_puntos(cantidad)` → suma al puntaje
- Método `perder_vida()` → resta una vida e imprime cuántas quedan
- Método `estado()` → imprime nombre, puntaje y vidas actuales

Crea un jugador y prueba todos los métodos.

---

## Conexiones

- [[POO — Clases y Objetos]] — módulo anterior: qué es una clase y un objeto
- [[POO de Python]] — índice del módulo POO
- [[POO — Init y Self]] — módulo siguiente: `__init__` y `self`
