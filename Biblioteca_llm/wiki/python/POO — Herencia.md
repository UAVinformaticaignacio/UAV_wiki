---
tipo: concepto
tags: [python, POO, herencia, super]
fuentes: [raw/Modulo_05_Herencia.md]
actualizado: 2026-05-20
---

# Módulo 5 — Herencia

---

## ¿Qué es la Herencia?

La herencia permite que una clase **tome todo lo de otra clase** y le agregue o modifique lo que necesite. Sirve para **reutilizar código** y organizar mejor las clases.

```
Clase Padre  →  tiene lo general y compartido
Clase Hija   →  hereda lo general y agrega lo específico
```

---

## El problema sin herencia

Imagina estas dos clases:

```python
class Perro:
    def __init__(self, nombre):
        self.nombre = nombre
    def comer(self):
        print(f"{self.nombre} está comiendo")
    def dormir(self):
        print(f"{self.nombre} está durmiendo")

class Gato:
    def __init__(self, nombre):
        self.nombre = nombre
    def comer(self):               # repetido 😵
        print(f"{self.nombre} está comiendo")
    def dormir(self):              # repetido 😵
        print(f"{self.nombre} está durmiendo")
```

`comer` y `dormir` son idénticos en ambas clases. Si mañana quieres cambiar algo, tienes que editarlo en **todos lados**. Con herencia, lo escribes **una sola vez**.

---

## Sintaxis básica

```python
class Padre:
    # código compartido
    pass

class Hija(Padre):     # Hija hereda de Padre
    # código propio
    pass
```

---

## Ejemplo — herencia simple

```python
class Animal:                       # clase padre
    def __init__(self, nombre):
        self.nombre = nombre

    def comer(self):
        print(f"{self.nombre} está comiendo")

    def dormir(self):
        print(f"{self.nombre} está durmiendo")


class Perro(Animal):                # hereda de Animal
    def ladrar(self):
        print(f"{self.nombre} dice: ¡Guau!")


class Gato(Animal):                 # hereda de Animal
    def maullar(self):
        print(f"{self.nombre} dice: ¡Miau!")


perro = Perro("Max")
perro.comer()    # Max está comiendo    ← heredado de Animal
perro.ladrar()   # Max dice: ¡Guau!    ← propio de Perro

gato = Gato("Luna")
gato.dormir()    # Luna está durmiendo ← heredado de Animal
gato.maullar()   # Luna dice: ¡Miau!   ← propio de Gato
```

---

## `super()` — usar el `__init__` del padre

Cuando la clase hija necesita su propio `__init__`, usa `super()` para no repetir el código del padre:

```python
class Animal:
    def __init__(self, nombre):
        self.nombre = nombre
        self.energia = 100


class Perro(Animal):
    def __init__(self, nombre, raza):
        super().__init__(nombre)    # ejecuta el __init__ del padre
        self.raza = raza            # agrega lo propio de Perro


perro = Perro("Max", "Labrador")
print(perro.nombre)    # Max       ← viene del padre
print(perro.energia)   # 100       ← viene del padre
print(perro.raza)      # Labrador  ← propio de Perro
```

Sin `super()`, tendrías que repetir `self.nombre = nombre` y `self.energia = 100` en cada clase hija.

---

## Sobreescribir métodos

La clase hija puede **redefinir** un método del padre para que se comporte diferente:

```python
class Animal:
    def __init__(self, nombre):
        self.nombre = nombre

    def hablar(self):
        print(f"{self.nombre} hace un sonido...")    # genérico


class Perro(Animal):
    def hablar(self):                                # sobreescribe
        print(f"{self.nombre} dice: ¡Guau!")


class Gato(Animal):
    def hablar(self):                                # sobreescribe
        print(f"{self.nombre} dice: ¡Miau!")


animales = [Animal("Animal"), Perro("Max"), Gato("Luna")]

for a in animales:
    a.hablar()
# Animal hace un sonido...
# Max dice: ¡Guau!
# Luna dice: ¡Miau!
```

---

## Ejemplo completo con varios niveles

```python
class Vehiculo:                              # abuelo
    def __init__(self, marca, velocidad_max):
        self.marca          = marca
        self.velocidad_max  = velocidad_max
        self.velocidad      = 0

    def acelerar(self, cantidad):
        if self.velocidad + cantidad <= self.velocidad_max:
            self.velocidad += cantidad
        else:
            self.velocidad = self.velocidad_max
        print(f"{self.marca} va a {self.velocidad} km/h")

    def __str__(self):
        return f"{self.marca} (máx {self.velocidad_max} km/h)"


class Auto(Vehiculo):                        # hijo
    def __init__(self, marca, velocidad_max, puertas):
        super().__init__(marca, velocidad_max)
        self.puertas = puertas

    def bocina(self):
        print(f"{self.marca}: ¡Beep beep!")


class AutoElectrico(Auto):                   # nieto
    def __init__(self, marca, velocidad_max, puertas, bateria):
        super().__init__(marca, velocidad_max, puertas)
        self.bateria = bateria

    def cargar(self):
        self.bateria = 100
        print(f"{self.marca} cargado al 100%")


tesla = AutoElectrico("Tesla", 250, 4, 80)
print(tesla)            # Tesla (máx 250 km/h)
tesla.acelerar(120)     # Tesla va a 120 km/h
tesla.bocina()          # Tesla: ¡Beep beep!
tesla.cargar()          # Tesla cargado al 100%
```

---

## Verificar herencia con `isinstance`

```python
print(isinstance(tesla, AutoElectrico))   # True
print(isinstance(tesla, Auto))            # True  (hereda de Auto)
print(isinstance(tesla, Vehiculo))        # True  (hereda de Vehiculo)
print(isinstance(tesla, Gato))            # False
```

---

## Resumen visual

```
        Vehiculo
        ├── marca
        ├── velocidad
        └── acelerar()
              |
            Auto
            ├── puertas
            └── bocina()
                  |
            AutoElectrico
            ├── bateria
            └── cargar()
```

---

## Resumen

```
class Hija(Padre)      →  Hija hereda todo de Padre
super().__init__(...)  →  ejecuta el __init__ del padre
sobreescribir          →  redefinir un método heredado en la hija
isinstance(obj, Clase) →  verifica si un objeto pertenece a una clase
```

---

## Mini ejercicio

Crea una clase `Empleado` con:
- Atributos: `nombre`, `salario`
- Método `trabajar()` que imprima `"[nombre] está trabajando"`

Luego crea dos clases hijas:
- `Programador(Empleado)` con atributo `lenguaje` y método `programar()`
- `Diseñador(Empleado)` con atributo `herramienta` y método `diseñar()`

Crea un objeto de cada una y prueba métodos heredados y propios.

---

## Conexiones

- [[POO — Métodos Especiales]] — módulo anterior: dunder methods
- [[POO de Python]] — índice del módulo POO
- [[POO — Encapsulamiento]] — módulo siguiente: proteger datos internos
- [[POO — Polimorfismo]] — muy relacionado: la herencia habilita el polimorfismo
