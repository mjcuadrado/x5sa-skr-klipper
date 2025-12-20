# Tronxy X5SA → Klipper Migration Project

![Project Status](https://img.shields.io/badge/status-active-success)
![Phase](https://img.shields.io/badge/phase-1-blue)
![License](https://img.shields.io/badge/license-MIT-green)

**Migración profesional y completamente documentada de Tronxy X5SA/Pro a Klipper con BTT SKR 1.4 Turbo**

---

## 📖 Acerca de Este Proyecto

Este es un proyecto de migración **real, en vivo y completamente documentado** de una impresora 3D Tronxy X5SA desde su electrónica stock a un sistema completo basado en Klipper.

**Características del proyecto:**
- ✅ Documentación paso a paso con **fotos reales**
- ✅ Manual tipo "para tontos" (sin asumir conocimientos previos)
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
| **[ROADMAP.md](ROADMAP.md)** | Plan técnico completo del proyecto (12 fases) |
| **[HARDWARE_REFERENCE.md](HARDWARE_REFERENCE.md)** | Especificaciones técnicas de todos los componentes |
| **[guides/](guides/)** | Guías paso a paso con fotos (en construcción) |

---

## 🗺️ Estado Actual

### ✅ Completado

- **Phase 0:** Baseline y auditoría inicial
- **Phase 1:** SKR 1.4 Turbo preparación
  - ✅ Step 1: SKR stock documentada
  - 🔄 Step 2: Jumpers UART (en curso)

### 🔄 En Progreso

**Phase 1, Step 2: Configuración jumpers UART**

[Ver guía actual →](guides/phase1/step2_uart_jumpers.md)

### 📋 Próximo

- Phase 1, Steps 3-5: Instalación drivers
- Phase 2: Cableado básico
- Phase 3: Firmware Klipper

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
│   ├── phase1/                 # Preparación SKR 1.4 Turbo
│   │   ├── step1_skr_stock.md
│   │   ├── step2_uart_jumpers.md
│   │   ├── step3_driver_orientation.md
│   │   ├── step4_driver_installation.md
│   │   └── step5_verification.md
│   └── phase2/                 # (próximamente)
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
| Placa principal | BTT SKR 1.4 Turbo | ✅ Adquirido |
| Drivers | TMC2209 ×5 | ✅ Adquirido |
| Toolhead board | BTT EBB42 CAN V1.2 | ✅ Adquirido |
| Sensor temperatura | PT100 + MAX31865 | ✅ Adquirido |
| Acelerómetro | ADXL345 (en EBB42) | ✅ Integrado |
| Sensor cama | Omron TL-Q5MC1-Z | ✅ Adquirido |
| DC-DC converter | XL4015 | ✅ Adquirido |

---

## 🎓 ¿Para Quién es Este Proyecto?

### Ideal si:
- ✅ Tienes una Tronxy X5SA o X5SA Pro
- ✅ Quieres aprender Klipper desde cero
- ✅ Buscas una guía paso a paso sin asumir conocimientos
- ✅ Valoras la documentación detallada con fotos
- ✅ Prefieres entender cada paso antes de ejecutarlo

### Requiere:
- ⚙️ Paciencia (35-40 horas estimadas para proyecto completo)
- ⚙️ Herramientas básicas (destornilladores, multímetro)
- ⚙️ PC/Raspberry Pi con Linux para Klipper
- ⚙️ Ganas de aprender

### NO requiere:
- ❌ Experiencia previa con Klipper
- ❌ Conocimientos de programación
- ❌ Experiencia en electrónica avanzada

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
Phase 0: ████████████ 100% ✅
Phase 1: ████████░░░░  40% 🔄
Phase 2: ░░░░░░░░░░░░   0% ⏸️
Phase 3: ░░░░░░░░░░░░   0% ⏸️
...
```

**Última actualización:** 2025-12-20

---

## 🚀 Empezar

**¿Listo para empezar tu migración?**

➡️ **[LEE LA GUÍA COMPLETA (GUIDE.md)](GUIDE.md)**

---

**Creado con ❤️ y mucha documentación**

*Si esta guía te ayuda, dale una ⭐ al repositorio*
