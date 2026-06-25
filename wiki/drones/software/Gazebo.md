---
tipo: entidad
tags: [Gazebo, drones, simulacion]
fuentes: [raw/pipeline-desarrollo-drones.md]
actualizado: 2026-06-12
---

# Gazebo

**Gazebo** es el simulador que reemplaza al mundo real durante el desarrollo de un dron. Simula física, sensores y ambiente.

---

## Qué simula

- **Física** — gravedad, colisiones, aerodinámica
- **Sensores** — GPS, IMU, cámara, lidar (con ruido)
- **Ambiente** — viento, terreno, obstáculos
- **Dron** — modelo 3D que responde a los motores

> [!info] Dato
> Gazebo reemplaza al mundo real durante el desarrollo. Cuando se vuela de verdad, Gazebo desaparece — el código no cambia (ver [[Pipeline de Desarrollo de Drones]], Fase 7).

---

## Analogía

Gazebo es **el estadio** donde se juega el partido; [[RViz2]] es **la tele** donde se ve. Si apagas la tele, el partido sigue.

---

## Uso (SITL)

```bash
cd ~/PX4-Autopilot
make px4_sitl gz_x500
```

Levanta [[PX4]] + Gazebo juntos. Ver [[SITL Quick Start]].

---

## Conexiones
- [[PX4]] — corre junto a Gazebo en SITL/HITL
- [[RViz2]] — visualiza los datos que Gazebo simula
- [[SITL y HITL]] — fases donde se usa Gazebo
- [[Pipeline de Desarrollo de Drones]] — Fases 3 y 4
