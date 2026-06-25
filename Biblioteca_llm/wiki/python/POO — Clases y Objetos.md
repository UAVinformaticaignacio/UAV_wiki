---
tipo: concepto
tags: [python, POO, clases, objetos]
fuentes: [raw/Modulo_01_Clases_y_Objetos.md]
actualizado: 2026-05-20
---

# Módulo 1 — Clases y Objetos

---

## ¿Qué es una Clase?

Una clase es un **molde** o **plantilla** para crear cosas. Define cómo va a ser algo, pero por sí sola no crea nada todavía.

Piénsalo como los **planos de una casa** — los planos no son la casa, pero a partir de ellos puedes construir muchas casas iguales o distintas.

---

## ¿Qué es un Objeto?

Un objeto es una **instancia** de una clase — es la casa construida a partir de los planos.

A partir de **una sola clase** puedes crear **muchos objetos**, cada uno independiente del otro.

```
Clase   →   el molde      →   Auto
Objeto  →   lo que creas  →   auto1, auto2, auto3
```

---

## ¿Por qué usar Clases?

Sin clases, organizar datos de múltiples cosas es un caos:

```python
# Sin clases — variables sueltas 😵
auto1_marca    = "Toyota"
auto1_color    = "Rojo"
auto1_velocidad = 0

auto2_marca    = "Ford"
auto2_color    = "Azul"
auto2_velocidad = 0
```

Imagina hacer eso con 100 autos. Con una clase, todo queda **agrupado y ordenado**.

---

## Sintaxis básica

```python
class NombreClase:
    pass
```

- `class` le dice a Python que estás definiendo un molde.
- El nombre de la clase se escribe en **PascalCase** (primera letra de cada palabra en mayúscula).
- `pass` se usa cuando la clase está vacía por ahora.

---

## Ejemplo completo

```python
class Auto:
    pass


# Crear objetos a partir de la clase
auto1 = Auto()
auto2 = Auto()
auto3 = Auto()

# Asignar atributos a cada objeto
auto1.marca = "Toyota"
auto1.color = "Rojo"

auto2.marca = "Ford"
auto2.color = "Azul"

auto3.marca = "BMW"
auto3.color = "Negro"

# Acceder a los atributos
print(auto1.marca)   # Toyota
print(auto2.color)   # Azul
print(auto3.marca)   # BMW
```

Cada objeto tiene sus propios datos sin mezclarse con los demás.

---

## ¿Cómo sabe Python que son objetos distintos?

Cada objeto ocupa un lugar diferente en la memoria del computador. Puedes comprobarlo así:

```python
print(auto1)   # <__main__.Auto object at 0x000001>
print(auto2)   # <__main__.Auto object at 0x000002>
```

Python te dice que ambos son de tipo `Auto`, pero viven en direcciones de memoria distintas — son completamente independientes.

---

## El punto `.` — cómo accedes a los datos

El punto `.` significa **"entra a"** o **"el ... de"**:

```python
auto1.marca   # el 'marca' de 'auto1'
auto2.color   # el 'color' de 'auto2'
```

Sin el punto, Python no sabe de qué objeto estás hablando:

```python
print(marca)       # ❌ NameError — no existe ninguna variable suelta llamada 'marca'
print(auto1.marca) # ✅ Toyota
```

---

## Resumen

| Concepto  | Descripción                | Ejemplo                  |
| --------- | -------------------------- | ------------------------ |
| `class`   | Define el molde            | `class Auto:`            |
| Objeto    | Instancia creada del molde | `auto1 = Auto()`         |
| Atributo  | Dato que tiene el objeto   | `auto1.marca = "Toyota"` |
| Punto `.` | Accede a datos del objeto  | `auto1.color`            |

---

## Mini ejercicio

Crea una clase llamada `Persona` y luego crea **3 objetos** a partir de ella.
A cada uno asígnale:
- `nombre`
- `edad`
- `ciudad`

Luego imprime todos sus datos.

---

## Conexiones

- [[POO de Python]] — índice del módulo POO
- [[POO — Atributos y Métodos]] — módulo siguiente: datos y acciones de los objetos
