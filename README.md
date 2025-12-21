# Tronxy X5SA (stock) → Klipper Migration Project

![Project Status](https://img.shields.io/badge/status-active-success)
![Phase](https://img.shields.io/badge/phase-3-blue)
![License](https://img.shields.io/badge/license-MIT-green)

**Migración profesional y completamente documentada de Tronxy X5SA/Pro a Klipper con BTT SKR 1.4 Turbo**

---

## 📖 Acerca de Este Proyecto

Este es un proyecto de migración **real, en vivo y completamente documentado** de una impresora 3D Tronxy X5SA desde su electrónica stock a un sistema completo basado en Klipper.

**Características del proyecto:**
- ✅ Documentación paso a paso con **fotos reales**
- ✅ Manual detallado para personas **novatas en impresión 3D** (requiere conocimientos básicos de electrónica y Klipper)
- ✅ Guías reproducibles para cualquier usuario
- ✅ Configuración Klipper versionada
- ✅ STLs de calibración y upgrades
- ✅ Proceso reversible y seguro

---

## 🎯 Objetivo Final

Transformar una Tronxy X5SA stock en una impresora de alto rendimiento:

**Hardware final:**
- BTT SKR 1.4 Turbo (MCU @ 120MHz)
- 5× TMC2209 (UART, silent)
- BTT EBB42 CAN (toolhead board)
- PT100 con MAX31865 (sensor alta precisión)
- ADXL345 (input shaping)
- Omron TL-Q5MC1-Z (sensor inductivo)

**Software:**
- Klipper firmware
- Mainsail / Fluidd UI
- Input shaping calibrado
- Pressure advance optimizado

---

## 📚 Documentación

### 🚀 Empezar Aquí

**[📘 GUÍA DE MIGRACIÓN (GUIDE.md)](GUIDE.md)** ← **Empieza aquí si quieres replicar el proyecto**

La guía principal con:
- Introducción para principiantes
- Lista de la compra completa
- Índice de todas las fases
- Enlaces a pasos específicos

### 📋 Documentación Adicional

| Documento | Descripción |
|-----------|-------------|
| **[ROADMAP.md](ROADMAP.md)** | Plan técnico completo del proyecto (8 fases) |
| **[HARDWARE_REFERENCE.md](HARDWARE_REFERENCE.md)** | Especificaciones técnicas de todos los componentes |
| **[guides/](guides/)** | Guías paso a paso con fotos |

---

## 🗺️ Estado Actual

### ✅ Completado

- **Phase 0:** Baseline y auditoría inicial ✅
- **Phase 1:** SKR 1.4 Turbo preparación ✅ (2025-12-20)
  - ✅ Jumpers UART configurados
  - ✅ Drivers TMC2209 instalados
  - ✅ Verificación completa
- **Phase 2:** SKR Cableado Básico ✅ (2025-12-21)
  - ✅ Documentación wiring stock (36 fotos)
  - ✅ SKR montada posición superior
  - ✅ Cables extensión fabricados
  - ✅ Motores, cama y alimentación conectados

### 🔄 En Progreso

**Phase 3: Toolhead EBB42 CAN** ⬅️ **SIGUIENTE**

Instalación y cableado de EBB42 en toolhead con comunicación CAN bus

### 📋 Próximo

- Phase 4: Firmware Klipper + configuración completa
- Phase 5: Primera impresión + calibraciones
- Phase 6-7: Input Shaper + optimizaciones finales

---

## 📁 Estructura del Repositorio

```
x5sa-skr-klipper/
├── README.md                   # ⬅️ Estás aquí
├── GUIDE.md                    # Guía de migración principal
├── ROADMAP.md                  # Plan técnico del proyecto
├── HARDWARE_REFERENCE.md       # Especificaciones hardware
│
├── guides/                     # Guías paso a paso
│   ├── phase1/                 # ✅ Preparación SKR 1.4 Turbo
│   │   ├── step1_skr_stock.md
│   │   ├── step2_uart_jumpers.md
│   │   ├── step3_driver_orientation.md
│   │   ├── step4_driver_installation.md
│   │   └── step5_verification.md
│   ├── phase2/                 # ✅ SKR Cableado Básico
│   │   ├── README.md
│   │   ├── step1_documentation.md
│   │   ├── step2_stock_disconnection.md
│   │   ├── step3_skr_mounting.md
│   │   ├── step4_skr_basic_wiring.md
│   │   └── step5_verification.md
│   └── phase3/                 # 🔄 Toolhead EBB42 CAN (en curso)
│
├── photos/                     # Fotos reales del proceso
│   ├── phase0/
│   ├── phase1/
│   └── ...
│
├── stls/                       # Archivos STL
│   ├── calibration/            # Piezas de calibración
│   └── upgrades/               # Mejoras impresas
│
├── klipper/                    # Configuración Klipper
│   └── printer.cfg             # (enlace simbólico a sistema)
│
└── phases/                     # Documentación técnica
    ├── phase0/
    ├── phase1/
    └── ...
```

---

## 🛠️ Hardware Utilizado

### Impresora Base
- **Modelo:** Tronxy X5SA / X5SA Pro
- **Cinemática:** CoreXY
- **Volumen:** 330×330×400mm
- **Voltaje:** 24V DC

### Electrónica Nueva
| Componente | Modelo | Estado |
|------------|--------|--------|
| Placa principal | BTT SKR 1.4 Turbo | ✅ Instalado |
| Drivers | TMC2209 ×5 | ✅ Instalados |
| Toolhead board | BTT EBB42 CAN V1.2 | 🔄 En instalación |
| Sensor temperatura | PT100 + MAX31865 | ✅ Adquirido |
| Acelerómetro | ADXL345 (en EBB42) | ✅ Integrado |
| Sensor Z | Omron TL-Q5MC1-Z | ✅ Adquirido |

---

## 🎓 ¿Para Quién es Este Proyecto?

### Ideal si:
- ✅ Tienes una Tronxy X5SA o X5SA Pro
- ✅ Quieres migrar a Klipper con una guía detallada
- ✅ Valoras la documentación paso a paso con fotos
- ✅ Prefieres entender cada paso antes de ejecutarlo
- ✅ Eres **novato en impresión 3D** pero tienes base técnica

### Requiere (conocimientos previos):
- ⚙️ **Electrónica básica** (cables, polaridad, multímetro)
- ⚙️ **Klipper básico** (conceptos de printer.cfg, MCU, etc.)
- ⚙️ **Linux básico** (SSH, navegación terminal)
- ⚙️ **Lectura técnica** (datasheets, pinouts)

### Requiere (hardware/tiempo):
- ⚙️ Paciencia (22-32 horas estimadas para proyecto completo)
- ⚙️ Herramientas básicas (destornilladores, multímetro, crimpadora)
- ⚙️ PC/Raspberry Pi con Linux para Klipper
- ⚙️ Ganas de aprender y documentar

### NO requiere:
- ❌ Experiencia avanzada con Klipper
- ❌ Conocimientos de programación
- ❌ Experiencia en soldadura (opcional para algunas modificaciones)

---

## 📦 STLs Disponibles

### Calibración
*(A añadir conforme se utilicen)*
- XYZ calibration cube
- First layer test
- Temperature tower
- Retraction test
- Y más...

### Upgrades
*(A diseñar/añadir)*
- EBB42 mount
- Cable chain mounts
- Sensor mounts
- Y más...

---

## ⚙️ Configuración Klipper

La configuración de Klipper se versiona en este repositorio y está enlazada simbólicamente al sistema:

```bash
~/printer_data/config/printer.cfg → /ruta/repo/klipper/printer.cfg
```

Cada fase importante genera un commit con la configuración actualizada.

---

## 🤝 Contribuciones

Este proyecto está diseñado para ser reproducible. Si sigues la guía:

**Abre un Issue si:**
- Encuentras errores en la documentación
- Algún paso no funciona como se describe
- Tienes sugerencias de mejora

**Haz un Pull Request si:**
- Mejoras la documentación
- Añades fotos adicionales útiles
- Corriges errores tipográficos

---

## 📜 Licencia

Este proyecto está bajo licencia **MIT**.

Eres libre de:
- ✅ Usar esta documentación
- ✅ Compartirla
- ✅ Modificarla
- ✅ Crear proyectos derivados

**Con atribución:** Menciona este repositorio original.

---

## 🔗 Enlaces Útiles

### Klipper
- [Documentación oficial Klipper](https://www.klipper3d.org/)
- [Klipper Discord](https://discord.klipper3d.org/)
- [Klipper GitHub](https://github.com/Klipper3d/klipper)

### Hardware
- [BTT GitHub - SKR 1.4](https://github.com/bigtreetech/BIGTREETECH-SKR-V1.3)
- [BTT GitHub - EBB Series](https://github.com/bigtreetech/EBB)
- [BTT Wiki](https://bttwiki.com/)

### Tronxy
- [Tronxy Official](https://www.tronxy3d.com/)
- [Tronxy Community](https://www.thingiverse.com/groups/tronxy)

---

## 📊 Progreso del Proyecto

```
Phase 0: ████████████ 100% ✅ Baseline
Phase 1: ████████████ 100% ✅ SKR preparación
Phase 2: ████████████ 100% ✅ SKR cableado
Phase 3: ████░░░░░░░░  30% 🔄 EBB42 CAN (en curso)
Phase 4: ░░░░░░░░░░░░   0% ⏸️ Firmware
Phase 5: ░░░░░░░░░░░░   0% ⏸️ Primera impresión
Phase 6: ░░░░░░░░░░░░   0% ⏸️ Input Shaper
Phase 7: ░░░░░░░░░░░░   0% ⏸️ Calibraciones finales
```

**Progreso total:** ~35% (8h de 22-32h estimadas)
**Última actualización:** 2025-12-21

---

## 🚀 Empezar

**¿Listo para empezar tu migración?**

➡️ **[LEE LA GUÍA COMPLETA (GUIDE.md)](GUIDE.md)**

---

**Creado con ❤️ y mucha documentación**

*Si esta guía te ayuda, dale una ⭐ al repositorio*
