---
tipo: concepto
tags: [python, POO, encapsulamiento, getters, setters]
fuentes: [raw/Modulo_06_Encapsulamiento.md]
actualizado: 2026-05-20
---

# Módulo 6 — Encapsulamiento

---

## ¿Qué es el Encapsulamiento?

Es **proteger los datos internos** de un objeto para que no se puedan modificar directamente desde afuera. La idea es controlar **cómo se leen y modifican** los atributos.

Sin encapsulamiento, cualquiera puede romper tu objeto sin querer.

---

## El problema sin encapsulamiento

```python
class CuentaBancaria:
    def __init__(self, saldo):
        self.saldo = saldo


cuenta = CuentaBancaria(1000)
cuenta.saldo = -99999        # ❌ nadie debería poder hacer esto
print(cuenta.saldo)          # -99999 — el objeto quedó en mal estado
```

No hay ninguna validación. Cualquiera puede asignar cualquier valor directamente.

---

## Los tres niveles de acceso

Python tiene tres formas de definir qué tan "accesible" es un atributo:

| Tipo      | Sintaxis        | ¿Qué significa?                              |
| --------- | --------------- | -------------------------------------------- |
| Público   | `self.nombre`   | Cualquiera puede leerlo y modificarlo        |
| Protegido | `self._nombre`  | Convención — "no lo toques desde afuera"     |
| Privado   | `self.__nombre` | Bloqueado — no se puede acceder desde afuera |

---

## Atributo Público

```python
class Persona:
    def __init__(self, nombre):
        self.nombre = nombre    # público


p = Persona("Juan")
print(p.nombre)    # ✅ Juan
p.nombre = "Pedro" # ✅ se puede modificar directamente
```

---

## Atributo Protegido `_`

Un solo guión bajo es una **convención** — no bloquea el acceso, pero le avisa a otros desarrolladores que ese atributo es de uso interno:

```python
class Persona:
    def __init__(self, nombre):
        self._nombre = nombre    # protegido (convención)


p = Persona("Juan")
print(p._nombre)    # ✅ técnicamente funciona, pero es mala práctica
```

---

## Atributo Privado `__`

Con doble guión bajo, Python **renombra el atributo internamente** y lo bloquea desde afuera:

```python
class CuentaBancaria:
    def __init__(self, saldo):
        self.__saldo = saldo    # privado


cuenta = CuentaBancaria(1000)
print(cuenta.__saldo)           # ❌ AttributeError — no existe desde afuera
```

Python lo transforma internamente a `_CuentaBancaria__saldo` — por eso no es accesible como `__saldo`.

---

## Getters — leer atributos privados

Un **getter** es un método que permite leer un atributo privado de forma controlada:

```python
class CuentaBancaria:
    def __init__(self, saldo):
        self.__saldo = saldo

    def get_saldo(self):         # getter
        return self.__saldo


cuenta = CuentaBancaria(1000)
print(cuenta.get_saldo())   # ✅ 1000
```

> Crea un getter **solo si alguien de afuera necesita leer ese dato**.

---

## Setters — modificar con validación

Un **setter** es un método que permite modificar un atributo privado, **pero con validación antes**:

```python
class CuentaBancaria:
    def __init__(self, saldo):
        self.__saldo = saldo

    def get_saldo(self):
        return self.__saldo

    def set_saldo(self, nuevo_saldo):    # setter
        if nuevo_saldo < 0:
            print("❌ El saldo no puede ser negativo")
        else:
            self.__saldo = nuevo_saldo
            print(f"✅ Saldo actualizado a: ${self.__saldo}")


cuenta = CuentaBancaria(1000)
cuenta.set_saldo(2000)    # ✅ Saldo actualizado a: $2000
cuenta.set_saldo(-500)    # ❌ El saldo no puede ser negativo
print(cuenta.get_saldo()) # 2000 — no cambió
```

---

## Ejemplo completo

```python
class Producto:
    def __init__(self, nombre, precio, stock):
        self.__nombre = nombre
        self.__precio = precio
        self.__stock  = stock

    # Getters
    def get_nombre(self):
        return self.__nombre

    def get_precio(self):
        return self.__precio

    def get_stock(self):
        return self.__stock

    # Setters con validación
    def set_precio(self, nuevo_precio):
        if nuevo_precio <= 0:
            print("❌ El precio debe ser mayor a 0")
        else:
            self.__precio = nuevo_precio
            print(f"✅ Precio actualizado a: ${self.__precio}")

    def set_stock(self, cantidad):
        if cantidad < 0:
            print("❌ El stock no puede ser negativo")
        else:
            self.__stock = cantidad

    def vender(self, cantidad):
        if cantidad > self.__stock:
            print(f"❌ Stock insuficiente. Solo hay {self.__stock} unidades")
        else:
            self.__stock -= cantidad
            print(f"✅ Venta de {cantidad} unidades. Stock restante: {self.__stock}")

    def __str__(self):
        return f"{self.__nombre} | ${self.__precio} | Stock: {self.__stock}"


p = Producto("Notebook", 500000, 10)
print(p)                  # Notebook | $500000 | Stock: 10

p.set_precio(450000)      # ✅ Precio actualizado a: $450000
p.set_precio(-100)        # ❌ El precio debe ser mayor a 0

p.vender(3)               # ✅ Venta de 3 unidades. Stock restante: 7
p.vender(20)              # ❌ Stock insuficiente. Solo hay 7 unidades

print(p)                  # Notebook | $450000 | Stock: 7
```

---

## ¿Cuándo usar cada nivel?

```
self.x    →  dato que cualquiera puede ver y cambiar libremente
self._x   →  dato interno, pero sin bloqueo real (solo convención)
self.__x  →  dato crítico que necesita validación para ser modificado
```

---

## Resumen

```
self.__x         →  atributo privado, bloqueado desde afuera
get_x()          →  método para leer el atributo privado
set_x(valor)     →  método para modificar con validación
Encapsulamiento  →  controlar el acceso a los datos internos
```

---

## Mini ejercicio

Crea una clase `Persona` con:
- Atributos privados: `__nombre`, `__edad`
- Getter para ambos
- Setter para `__edad` que no permita valores negativos ni mayores a 120
- `__str__` que imprima `"Nombre: Juan | Edad: 30"`

Prueba intentando asignar una edad inválida directamente y luego por el setter.

---

## Conexiones

- [[POO — Herencia]] — módulo anterior: reutilizar clases
- [[POO de Python]] — índice del módulo POO
- [[POO — Polimorfismo]] — módulo siguiente: mismo método, distinto comportamiento
