---
tipo: concepto
tags: [python, POO, polimorfismo, duck-typing]
fuentes: [raw/Modulo_07_Polimorfismo.md]
actualizado: 2026-05-20
---

# Módulo 7 — Polimorfismo

---

## ¿Qué es el Polimorfismo?

Polimorfismo significa **"muchas formas"**. En POO, se refiere a que el **mismo método** puede comportarse de manera distinta dependiendo del objeto que lo llame.

La clave es: **mismo nombre de método, distinto comportamiento**.

---

## ¿Por qué es útil?

Te permite escribir código que trabaja con distintos tipos de objetos **de forma uniforme**, sin necesitar saber exactamente de qué tipo es cada uno.

---

## Ejemplo sin polimorfismo — el problema

```python
def hacer_hablar(animal, tipo):
    if tipo == "perro":
        print(f"{animal} dice: ¡Guau!")
    elif tipo == "gato":
        print(f"{animal} dice: ¡Miau!")
    elif tipo == "pato":
        print(f"{animal} dice: ¡Cuac!")
    # cada animal nuevo requiere otro elif... 😵
```

Cada vez que agregas un animal nuevo, tienes que modificar esta función. Con polimorfismo, no.

---

## Ejemplo con polimorfismo — la solución

```python
class Animal:
    def __init__(self, nombre):
        self.nombre = nombre

    def hablar(self):
        print("...")                          # comportamiento genérico


class Perro(Animal):
    def hablar(self):                         # sobreescribe
        print(f"{self.nombre} dice: ¡Guau!")


class Gato(Animal):
    def hablar(self):                         # sobreescribe
        print(f"{self.nombre} dice: ¡Miau!")


class Pato(Animal):
    def hablar(self):                         # sobreescribe
        print(f"{self.nombre} dice: ¡Cuac!")


# La magia del polimorfismo:
animales = [Perro("Max"), Gato("Luna"), Pato("Donald")]

for animal in animales:
    animal.hablar()            # cada uno responde a su manera
# Max dice: ¡Guau!
# Luna dice: ¡Miau!
# Donald dice: ¡Cuac!
```

El `for` no sabe qué tipo de animal es cada uno — simplemente llama a `hablar()` y cada objeto sabe qué hacer.

---

## Polimorfismo con funciones

También puedes pasar distintos objetos a una misma función:

```python
def hacer_sonar(animal):
    animal.hablar()            # funciona con cualquier animal


hacer_sonar(Perro("Rex"))      # Rex dice: ¡Guau!
hacer_sonar(Gato("Misi"))      # Misi dice: ¡Miau!
hacer_sonar(Pato("Pato"))      # Pato dice: ¡Cuac!
```

La función `hacer_sonar` no necesita saber el tipo — solo que el objeto tiene un método `hablar()`.

---

## Polimorfismo sin herencia

El polimorfismo también funciona entre clases que **no están relacionadas**, siempre que compartan el mismo nombre de método. Esto se llama **duck typing** en Python:

> "Si camina como un pato y habla como un pato, es un pato."

```python
class Persona:
    def __init__(self, nombre):
        self.nombre = nombre

    def hablar(self):
        print(f"{self.nombre} dice: ¡Hola!")


class Robot:
    def __init__(self, modelo):
        self.modelo = modelo

    def hablar(self):
        print(f"{self.modelo} dice: BLEEP BLOOP")


class Loro:
    def __init__(self, nombre):
        self.nombre = nombre

    def hablar(self):
        print(f"{self.nombre} dice: ¡Polly quiere una galleta!")


# No heredan entre sí, pero los tres tienen hablar()
hablantes = [Persona("Juan"), Robot("R2D2"), Loro("Polly")]

for h in hablantes:
    h.hablar()
# Juan dice: ¡Hola!
# R2D2 dice: BLEEP BLOOP
# Polly dice: ¡Polly quiere una galleta!
```

---

## Ejemplo completo — sistema de pagos

Un ejemplo más real: distintos métodos de pago que comparten la misma interfaz.

```python
class PagoEfectivo:
    def __init__(self, monto):
        self.monto = monto

    def procesar(self):
        print(f"💵 Pago en efectivo de ${self.monto} procesado")


class PagoTarjeta:
    def __init__(self, monto, numero_tarjeta):
        self.monto           = monto
        self.numero_tarjeta  = numero_tarjeta

    def procesar(self):
        print(f"💳 Pago con tarjeta ****{self.numero_tarjeta[-4:]} de ${self.monto} procesado")


class PagoTransferencia:
    def __init__(self, monto, banco):
        self.monto = monto
        self.banco = banco

    def procesar(self):
        print(f"🏦 Transferencia desde {self.banco} de ${self.monto} procesada")


def realizar_pago(metodo_pago):
    print("Procesando pago...")
    metodo_pago.procesar()         # polimorfismo — no importa el tipo
    print("✅ Listo\n")


# Tres tipos distintos, misma función
realizar_pago(PagoEfectivo(5000))
realizar_pago(PagoTarjeta(15000, "1234567890123456"))
realizar_pago(PagoTransferencia(50000, "Banco Estado"))
```

---

## Polimorfismo vs Herencia — ¿cuál es la diferencia?

| | Herencia | Polimorfismo |
| ---- | -------- | ------------ |
| ¿Qué es? | Una clase toma lo de otra | El mismo método actúa diferente |
| ¿Requieren relación? | Sí | No necesariamente |
| ¿Se usan juntos? | Sí, muy seguido | Sí |

---

## Resumen

```
Polimorfismo  →  mismo método, distinto comportamiento según el objeto
sobreescribir →  redefinir un método heredado en la clase hija
duck typing   →  si tiene el método, funciona — sin importar la clase
```

---

## Mini ejercicio

Crea tres clases: `Circulo`, `Rectangulo`, `Triangulo`.
Cada una con:
- Sus propios atributos (radio, base/altura, lados)
- Un método `area()` que calcule y retorne el área correspondiente
- Un método `__str__` que describa la figura

Luego crea una lista con una figura de cada tipo e imprime el área de todas usando un `for`.

> Fórmulas:
> - Círculo: `3.14159 * radio ** 2`
> - Rectángulo: `base * altura`
> - Triángulo: `(base * altura) / 2`

---

## Conexiones

- [[POO — Encapsulamiento]] — módulo anterior: proteger datos internos
- [[POO — Herencia]] — la herencia habilita el polimorfismo con sobreescribir
- [[POO de Python]] — índice del módulo POO
