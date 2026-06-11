---
tipo: concepto
tags: [python, POO, dunder, métodos-especiales]
fuentes: [raw/Modulo_04_Metodos_Especiales.md]
actualizado: 2026-05-20
---

# Módulo 4 — Métodos Especiales (Dunder Methods)

---

## ¿Qué son?

Son métodos que Python reconoce y ejecuta **automáticamente** en situaciones específicas. Siempre tienen **doble guión bajo** al inicio y al final:

```
__nombre__
```

Se les llama **dunder methods** (de *double underscore* — doble guión bajo). Ya conoces uno: `__init__`. Pero hay varios más muy útiles.

---

## ¿Por qué existen?

Permiten que tus objetos se comporten como tipos nativos de Python. Por ejemplo, que se puedan imprimir con `print()`, comparar con `==`, medir con `len()`, sumar con `+`, y más.

---

## `__str__` — cómo se imprime tu objeto

Sin `__str__`, imprimir un objeto da algo inútil:

```python
class Auto:
    def __init__(self, marca, color):
        self.marca = marca
        self.color = color


auto = Auto("Toyota", "Rojo")
print(auto)   # <__main__.Auto object at 0x000001>  😕
```

Con `__str__` defines tú mismo qué se muestra:

```python
class Auto:
    def __init__(self, marca, color):
        self.marca = marca
        self.color = color

    def __str__(self):
        return f"Auto {self.marca} de color {self.color}"


auto = Auto("Toyota", "Rojo")
print(auto)   # Auto Toyota de color Rojo  ✅
```

> `__str__` debe siempre **retornar un string** con `return`, no usar `print`.

---

## `__len__` — darle un largo a tu objeto

Permite usar `len()` sobre tu propio objeto:

```python
class Equipo:
    def __init__(self, nombre, jugadores):
        self.nombre     = nombre
        self.jugadores  = jugadores

    def __len__(self):
        return len(self.jugadores)


equipo = Equipo("Los Tigres", ["Juan", "Maria", "Pedro", "Ana"])
print(len(equipo))   # 4
```

---

## `__eq__` — comparar dos objetos

Sin `__eq__`, comparar objetos siempre da `False` aunque tengan los mismos datos:

```python
auto1 = Auto("Toyota", "Rojo")
auto2 = Auto("Toyota", "Rojo")

print(auto1 == auto2)   # False 😕  (son distintos objetos en memoria)
```

Con `__eq__` defines **tú** cuándo dos objetos son iguales:

```python
class Auto:
    def __init__(self, marca, color):
        self.marca = marca
        self.color = color

    def __eq__(self, otro):
        return self.marca == otro.marca and self.color == otro.color


auto1 = Auto("Toyota", "Rojo")
auto2 = Auto("Toyota", "Rojo")
auto3 = Auto("Ford", "Azul")

print(auto1 == auto2)   # True   ✅ misma marca y color
print(auto1 == auto3)   # False  ✅ distintos
```

---

## `__add__` — sumar dos objetos con `+`

```python
class Dinero:
    def __init__(self, monto):
        self.monto = monto

    def __add__(self, otro):
        return Dinero(self.monto + otro.monto)

    def __str__(self):
        return f"${self.monto}"


billetera1 = Dinero(5000)
billetera2 = Dinero(3000)
total = billetera1 + billetera2   # usa __add__ automáticamente

print(total)   # $8000
```

---

## Todos juntos en un ejemplo real

```python
class Producto:
    def __init__(self, nombre, precio):
        self.nombre = nombre
        self.precio = precio

    def __str__(self):
        return f"{self.nombre} — ${self.precio}"

    def __eq__(self, otro):
        return self.nombre == otro.nombre and self.precio == otro.precio

    def __add__(self, otro):
        total = self.precio + otro.precio
        return f"Total de '{self.nombre}' y '{otro.nombre}': ${total}"


p1 = Producto("Café", 1500)
p2 = Producto("Té", 1500)
p3 = Producto("Jugo", 2000)

print(p1)             # Café — $1500
print(p2)             # Té — $1500
print(p1 == p2)       # False (distinto nombre aunque mismo precio)
print(p1 + p3)        # Total de 'Café' y 'Jugo': $3500
```

---

## Tabla de los más usados

| Método     | Se ejecuta cuando...              | Ejemplo de uso           |
| ---------- | --------------------------------- | ------------------------ |
| `__init__` | Creas el objeto                   | `Auto("Toyota", "Rojo")` |
| `__str__`  | Haces `print(objeto)`             | `print(auto)`            |
| `__len__`  | Haces `len(objeto)`               | `len(equipo)`            |
| `__eq__`   | Comparas `objeto1 == objeto2`     | `auto1 == auto2`         |
| `__add__`  | Sumas `objeto1 + objeto2`         | `billetera1 + billetera2`|
| `__lt__`   | Comparas `objeto1 < objeto2`      | `precio1 < precio2`      |

---

## Resumen

```
__init__   →  al crear el objeto
__str__    →  al imprimir con print()
__len__    →  al medir con len()
__eq__     →  al comparar con ==
__add__    →  al sumar con +
```

Todos se ejecutan **solos** cuando Python los necesita — tú solo los defines.

---

## Mini ejercicio

Toma tu clase `Persona` y agrégale:
- `__str__` que imprima `"Persona: Juan, 30 años, Santiago"`
- `__eq__` que considere iguales a dos personas si tienen el mismo nombre Y edad

Prueba creando dos personas iguales y dos distintas.

---

## Conexiones

- [[POO — Init y Self]] — módulo anterior: `__init__` y `self`
- [[POO de Python]] — índice del módulo POO
- [[POO — Herencia]] — módulo siguiente: reutilizar clases
