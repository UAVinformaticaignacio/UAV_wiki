---
tipo: concepto
tags: [python, POO, __init__, self, constructor]
fuentes: [raw/Modulo_03_Init_y_Self.md]
actualizado: 2026-05-20
---

# Módulo 3 — `__init__` y `self`

---

## El problema sin `__init__`

Sin constructor, tienes que asignar cada dato **manualmente** después de crear el objeto:

```python
class Auto:
    pass


auto1 = Auto()
auto1.marca      = "Toyota"   # asignación manual
auto1.color      = "Rojo"     # asignación manual
auto1.velocidad  = 0          # asignación manual
```

Imagina hacer eso con 50 autos — repetitivo, lento y fácil de olvidar un dato.

---

## ¿Qué es `__init__`?

`__init__` es el **método constructor**. Se ejecuta automáticamente cada vez que creas un objeto. Su trabajo es **preparar el objeto** con sus datos iniciales.

```python
class Auto:
    def __init__(self, marca, color):
        self.marca      = marca
        self.color      = color
        self.velocidad  = 0       # siempre empieza en 0
```

Ahora crear un auto es simple:

```python
auto1 = Auto("Toyota", "Rojo")   # __init__ se ejecuta solo
auto2 = Auto("Ford", "Azul")

print(auto1.marca)      # Toyota
print(auto2.color)      # Azul
print(auto1.velocidad)  # 0
```

---

## ¿Qué pasa por dentro al crear un objeto?

Cuando escribes `Auto("Toyota", "Rojo")`, Python hace esto automáticamente:

```
1. Crea un objeto vacío
2. Llama a __init__(self, "Toyota", "Rojo")
3. self.marca     = "Toyota"
4. self.color     = "Rojo"
5. self.velocidad = 0
6. Te devuelve el objeto listo
```

Tú solo escribes **una línea**, Python hace todo lo demás.

---

## ¿Qué es `self`?

`self` es la forma que tiene Python de referirse al **objeto que se está usando en ese momento**. Es como el pronombre **"yo"** dentro de la clase.

```python
class Auto:
    def __init__(self, marca):
        self.marca = marca    # "mi marca es..."

    def describir(self):
        print(f"Yo soy un {self.marca}")   # "yo soy un..."
```

Cuando tienes varios objetos, `self` apunta al correcto:

```python
auto1 = Auto("Toyota")
auto2 = Auto("Ford")

auto1.describir()   # Yo soy un Toyota  → self = auto1
auto2.describir()   # Yo soy un Ford    → self = auto2
```

Cada objeto mantiene sus propios datos **sin mezclarse**.

---

## Valores por defecto en `__init__`

Puedes darle valores predeterminados a los parámetros:

```python
class Auto:
    def __init__(self, marca, color="Blanco", velocidad=0):
        self.marca      = marca
        self.color      = color
        self.velocidad  = velocidad
```

```python
auto1 = Auto("Toyota", "Rojo")   # color = Rojo
auto2 = Auto("Ford")             # color = Blanco (por defecto)
auto3 = Auto("BMW", "Negro", 100)

print(auto1.color)      # Rojo
print(auto2.color)      # Blanco
print(auto3.velocidad)  # 100
```

---

## Modificar atributos desde métodos

`self` te permite leer **y modificar** los atributos del objeto desde cualquier método:

```python
class Auto:
    def __init__(self, marca):
        self.marca      = marca
        self.velocidad  = 0

    def acelerar(self, cantidad):
        self.velocidad += cantidad           # modifica el atributo
        print(f"{self.marca} → {self.velocidad} km/h")

    def frenar(self, cantidad):
        if self.velocidad - cantidad < 0:    # valida antes de modificar
            self.velocidad = 0
        else:
            self.velocidad -= cantidad
        print(f"{self.marca} → {self.velocidad} km/h")

    def esta_detenido(self):
        return self.velocidad == 0           # retorna True o False


auto = Auto("Toyota")
auto.acelerar(80)            # Toyota → 80 km/h
auto.acelerar(40)            # Toyota → 120 km/h
auto.frenar(50)              # Toyota → 70 km/h
print(auto.esta_detenido())  # False
auto.frenar(200)             # Toyota → 0 km/h (no baja de 0)
print(auto.esta_detenido())  # True
```

---

## Ejemplo completo

```python
class Persona:
    def __init__(self, nombre, edad, ciudad="Sin especificar"):
        self.nombre  = nombre
        self.edad    = edad
        self.ciudad  = ciudad
        self.amigos  = []        # lista vacía, siempre empieza así

    def agregar_amigo(self, nombre_amigo):
        self.amigos.append(nombre_amigo)
        print(f"{nombre_amigo} fue agregado como amigo de {self.nombre}")

    def mostrar_amigos(self):
        if self.amigos:
            print(f"Amigos de {self.nombre}: {', '.join(self.amigos)}")
        else:
            print(f"{self.nombre} no tiene amigos aún")

    def cumpleanos(self):
        self.edad += 1
        print(f"¡Feliz cumpleaños {self.nombre}! Ahora tienes {self.edad} años")


p = Persona("Juan", 30, "Santiago")
p.mostrar_amigos()          # Juan no tiene amigos aún
p.agregar_amigo("Maria")    # Maria fue agregado como amigo de Juan
p.agregar_amigo("Pedro")    # Pedro fue agregado como amigo de Juan
p.mostrar_amigos()          # Amigos de Juan: Maria, Pedro
p.cumpleanos()              # ¡Feliz cumpleaños Juan! Ahora tienes 31 años
```

---

## Resumen

```
__init__         →  se ejecuta al crear el objeto
self             →  representa al objeto actual ("yo")
self.x = valor   →  guarda un dato dentro del objeto
self.x           →  accede a un dato del objeto
parámetro=valor  →  valor por defecto si no se pasa ese argumento
```

---

## Mini ejercicio

Crea una clase `Cuenta` con:
- Atributos: `titular`, `saldo` (por defecto 0), `historial` (lista vacía)
- Método `depositar(monto)` → suma al saldo y agrega al historial
- Método `retirar(monto)` → resta al saldo si hay suficiente, si no avisa
- Método `ver_historial()` → imprime todas las operaciones

Crea una cuenta y realiza varias operaciones.

---

## Conexiones

- [[POO — Atributos y Métodos]] — módulo anterior: datos y acciones de los objetos
- [[POO de Python]] — índice del módulo POO
- [[POO — Métodos Especiales]] — módulo siguiente: dunder methods
