# Phase 3 — Toolhead EBB42 USB

**Objetivo:** Instalar placa EBB42 en el toolhead y conectar todos los componentes del extrusor/hotend. Establecer comunicación USB con SKR 1.4 Turbo.

**Estado:** ✅ Hardware instalado y actualizado (2025-12-26)

---

## ⚡ ACTUALIZACIÓN IMPORTANTE (2025-12-26)

El probe sensor **Tronxy XY-08N** documentado inicialmente fue **abandonado** por incompatibilidades eléctricas (voltaje 24V vs 3.3V MCU).

**Probe actual:** **BIGTREETECH Eddy Coil V1.0** (corrientes de Eddy, comunicación I2C)

📗 **Documentación completa:**
- [EDDY_COIL_INSTALLATION.md](../../guides/phase3/EDDY_COIL_INSTALLATION.md) - Instalación física
- [EDDY_COIL_CALIBRATION.md](../../guides/phase3/EDDY_COIL_CALIBRATION.md) - Calibración y uso
- [INSTALACION_COMPLETADA.md](../../guides/phase3/INSTALACION_COMPLETADA.md) - Estado actual completo (v2.0)

---

## 📚 Documentación Principal

### 🚀 Guía de Implementación - MONTAJE TEMPORAL ⭐
📗 **[IMPLEMENTACION_TEMPORAL.md](../../guides/phase3/IMPLEMENTACION_TEMPORAL.md)** ← **USAR ESTA**
- ✅ Plan específico para montaje temporal junto a SKR
- 📋 Lista materiales reducida (cables cortos)
- 🔧 Proceso paso a paso optimizado (3-4 horas)
- ⚙️ Testing y verificación completa
- 🎯 Solo 1 cable largo necesario (sensor Omron)
- 💡 Troubleshooting específico montaje temporal

### 🎯 Guía Completa de Integración (Referencia)
📘 **[EBB42_INTEGRATION.md](../../guides/phase3/EBB42_INTEGRATION.md)**
- ✅ Todas las decisiones tomadas y justificadas
- 📋 Pinout completo de EBB42
- 🔧 Plan de cableado detallado (versión toolhead)
- ⚙️ Configuración Klipper completa
- 📝 Plan de implementación con 8 fases
- 🛡️ Troubleshooting y seguridad
- ⚠️ **Nota:** Asume montaje en toolhead (largo plazo)

### 📦 Checklist de Materiales
📋 **[MATERIALS_CHECKLIST.md](../../guides/phase3/MATERIALS_CHECKLIST.md)**
- Lista completa de hardware necesario
- Conectores y cables específicos
- Herramientas requeridas
- Lista de compra rápida

### 🔧 Guías de Flasheo
📗 **[FLASH_SKR_INSTRUCTIONS.md](../../guides/phase3/FLASH_SKR_INSTRUCTIONS.md)**
- Proceso paso a paso para flashear SKR 1.4 Turbo
- Configuración correcta LPC1769
- Flasheo vía tarjeta SD
- Verificación de detección USB

📘 **[FLASHEO_SKR_EXITOSO.md](../../guides/phase3/FLASHEO_SKR_EXITOSO.md)** ✅
- Caso de estudio completo del flasheo exitoso
- Troubleshooting: Problema chip incorrecto en docs
- Diagnóstico y solución detallada (STM32F407 → LPC1769)
- Configuración completa y verificación
- Lecciones aprendidas y referencias oficiales

📗 **[FLASH_EBB42_INSTRUCTIONS.md](../../guides/phase3/FLASH_EBB42_INSTRUCTIONS.md)**
- Proceso paso a paso para flashear EBB42 CAN V1.2
- Configuración correcta STM32G0B1 (USB mode)
- Flasheo vía DFU mode
- Verificación de detección USB

📘 **[FLASHEO_EBB42_EXITOSO.md](../../guides/phase3/FLASHEO_EBB42_EXITOSO.md)** ✅
- Caso de estudio completo del flasheo exitoso de EBB42
- Troubleshooting: Necesidad de alimentación 24V y reset manual
- Proceso completo DFU mode (offset 0x08000000)
- Configuración dual-MCU (SKR + EBB42)
- Lecciones aprendidas críticas (alimentación, jumper VUSB, reset)
- Guía de troubleshooting detallada

### 🔧 Configuración Multi-MCU
📘 **[CONFIGURACION_DUAL_MCU.md](../../guides/phase3/CONFIGURACION_DUAL_MCU.md)** ✅
- Guía completa de configuración múltiples MCUs en Klipper
- Arquitectura del sistema (SKR + EBB42)
- Sintaxis de pines multi-MCU (prefijos, modificadores)
- Obtención y uso de Serial IDs correctos
- Configuración de printer.cfg para [mcu] y [mcu EBBCan]
- Comandos de verificación y debugging
- Troubleshooting específico de dual-MCU
- Ejemplos prácticos de extrusor, ventiladores, probe
- Referencias y documentación oficial

---

## ✅ Estado Actual del Hardware

### SKR 1.4 Turbo - COMPLETADA ✅
**Fecha de flasheo exitoso:** 2025-12-21

- ✅ **Chip identificado:** LPC1769FBD100 (NXP ARM Cortex-M3, 120 MHz)
- ✅ **Firmware compilado:** Klipper v0.12.0-239-g8b8f7c09
- ✅ **Configuración correcta:** LPC176x + lpc1769 (120 MHz) + 16KiB bootloader + USB
- ✅ **Flasheo vía SD:** Exitoso (FIRMWARE.CUR confirmado)
- ✅ **Detección USB:** `usb-Klipper_lpc1769_12345-if00`
- ✅ **Conexión Klipper:** Sin errores, comunicación bidireccional funcional
- ✅ **printer.cfg:** Configurado con serial ID correcto
- ✅ **Gestión:** Enlace simbólico a repositorio git activo

**Documentación completa:** [FLASHEO_SKR_EXITOSO.md](../../guides/phase3/FLASHEO_SKR_EXITOSO.md)

### EBB42 CAN V1.2 - COMPLETADA ✅
**Fecha de flasheo exitoso:** 2025-12-21

- ✅ **Chip identificado:** STM32G0B1CBT6 (ARM Cortex-M0+)
- ✅ **Firmware compilado:** Klipper (STM32G0B1, USB mode, No bootloader)
- ✅ **Configuración correcta:** STM32G0B1 + No bootloader + USB (PA11/PA12) + 8MHz crystal
- ✅ **Flasheo vía DFU:** Exitoso (offset 0x08000000)
- ✅ **Detección USB:** `usb-Klipper_stm32g0b1xx_12345-if00`
- ✅ **Alimentación 24V:** Conectada y funcionando (cable AWG 20/18)
- ✅ **Jumper VUSB:** Instalado (requerido para modo USB)
- ✅ **Configuración dual-MCU:** SKR + EBB42 ambas detectadas en Klipper
- ✅ **printer.cfg:** Configurado con [mcu] y [mcu EBBCan]
- ✅ **Lecciones críticas:** Necesidad de alimentación 24V + reset manual post-DFU

**Documentación completa:** [FLASHEO_EBB42_EXITOSO.md](../../guides/phase3/FLASHEO_EBB42_EXITOSO.md)

---

## ✅ Decisiones Arquitectónicas Tomadas

### 1. Conexión: USB (NO CAN)
- ✅ Cable USB-C desde SKR a EBB42
- ✅ Simplicidad de configuración
- ✅ No requiere transceiver CAN

### 2. Ubicación: **TEMPORAL junto a SKR** (Frame Superior) 🔄
**Decisión Phase 3:** Montaje temporal junto a SKR en frame superior

**Razones:**
- ✅ Cables stock del toolhead **ya llegan al frame superior**
- ✅ Solo necesita 1 cable nuevo: Sensor Omron Z (~1.5m)
- ✅ USB-C y 24V entre placas: cables cortos (~30cm)
- ✅ Troubleshooting inicial más fácil (ambas placas accesibles)
- ✅ Minimiza complejidad en fase de testing
- ✅ Phase 12: Migración definitiva a toolhead con Stealthburner

**Configuración temporal:**
- EBB42 montada cerca de SKR (cinta + bridas)
- Cables stock hotend → conectan directamente a EBB42
- Único cable nuevo: Sensor Omron desde toolhead

### 3. Alimentación: 24V desde SKR
- ✅ Cable dedicado 24V desde FAN2/HE1 de SKR
- ✅ Always-on (100%)
- ✅ **Cable corto** (~30cm) SKR → EBB42
- ✅ Capacidad sobrada (~0.6A usados de 1-2A disponibles)

### 4. Conectores: Enfoque Mixto
- ✅ JST-XH: Críticos (24V, hotend, thermistor, probe)
- ✅ Dupont + hot glue: Ventiladores

### 5. Ventiladores
- ✅ FAN0 → Part cooling (controlado PWM)
- ✅ FAN1 → Hotend fan (always-on T>50°C)

### 6. Probe: ~~Omron NC~~ → **BIGTREETECH Eddy Coil V1.0** (2025-12-26)
- ❌ ~~Omron TL-Q5MC1-Z~~ (abandonado - incompatibilidad voltaje)
- ✅ **Eddy Coil V1.0** (LDC1612 sensor - I2C)
- ✅ Comunicación I2C directa (PB3/PB4 en EBB42)
- ✅ Precisión ±0.01mm (vs ±0.1mm inductivo)
- ✅ Calibración automática via Klipper

### 7. Motor Extrusor
- ⚠️ Phase 3: Motor E en SKR E0 (lateral - posición actual)
- ⚠️ NO se migra a EBB42 en esta fase
- ⏭️ Phase 12: Migra a EBB42 con Orbiter v2 (toolhead directo)

---

## Índice de Guías (Pendiente)

📁 **Guías detalladas en:** [`guides/phase3/`](../../guides/phase3/)

**Pasos tentativos:**
1. Documentación toolhead stock
2. Toma de decisiones
3. Preparación hardware
4. Montaje EBB42
5. Migración componentes
6. Cable CAN
7. Verificación física

*(Se completarán tras tomar decisiones)*

---

## Requisitos Previos

- [x] Phase 2 completada (SKR cableada)
- [x] Decisiones de planificación tomadas
- [x] Hardware confirmado:
  - [x] BTT EBB42 CAN V1.2 (flasheada ✅)
  - [x] ~~Sensor Omron TL-Q5MC1-Z~~ → **Eddy Coil V1.0** (instalado ✅)
  - [x] Thermistor NTC 100K stock (instalado ✅)
  - [x] Cable USB-C a USB-C (~30cm montaje temporal)
  - [x] Cable alimentación 24V (~30cm montaje temporal)
- [x] Herramientas: destornilladores, multímetro, crimpadora
- [x] Firmware Klipper para EBB42 compilado (USB mode)

---

## Objetivo al Finalizar Phase 3

### Hardware Instalado ✅

- [x] EBB42 montada en frame superior junto a SKR (montaje temporal)
- [x] Calentador hotend → EBB42 HE (PB13)
- [x] Thermistor NTC 100K → EBB42 TH0 (PA3)
- [x] Ventilador part cooling → EBB42 FAN (PA0)
- [x] Ventilador hotend → EBB42 FAN1 (PA1)
- [x] ~~Sensor Omron → EBB42 PROBE~~ → **Eddy Coil V1.0 → EBB42 I2C (PB3/PB4)**
- [x] Cable USB-C conectado (SKR ↔ EBB42)
- [x] Cable 24V conectado (SKR ↔ EBB42)

### NO Migrado en Phase 3
- ❌ Motor extrusor → **Se queda en SKR E0** hasta Phase 12

### Arquitectura Final Phase 3

**MONTAJE TEMPORAL - Ambas placas en Frame Superior:**

```
┌────────────────────────────────────────────────────────┐
│ FRAME SUPERIOR (Nueva ubicación electrónica)          │
│                                                        │
│  ┌──────────────┐  USB-C    ┌───────────────────┐     │
│  │ SKR 1.4 TB   │ ←(30cm)→  │ EBB42 (TEMPORAL)  │     │
│  │              │           │                   │     │
│  │ E0 ← Motor E │  24V      │ VIN/GND ← 24V     │     │
│  │    (lateral) │ ←(30cm)→  │ USB-C   ← USB     │     │
│  │              │           │                   │     │
│  │ FAN2 → 24V ──┼───────────→ HE      ← Heater  │ ←── Cables stock
│  │    always-on │           │ TH0     ← Therm   │ ←── toolhead
│  └──────────────┘           │ FAN0    ← Part fan│ ←── (ya llegan aquí)
│                             │ FAN1    ← HE fan  │ ←──
│                             │ PROBE   ←─────────┼─┐
│                             │ E0 (sin usar)     │ │
│                             └───────────────────┘ │
└────────────────────────────────────────────────────┼─┘
                                                     │
                                    Cable nuevo (~1.5m)
                                    Sensor Omron Z
                                                     ↓
                                            ┌────────────┐
                                            │ TOOLHEAD   │
                                            │ (stock)    │
                                            │            │
                                            │ Sensor Z   │
                                            │ Omron NC   │
                                            └────────────┘
```

**Phase 12:** EBB42 migra a toolhead con Stealthburner + Orbiter v2

---

## Estimación Temporal

| Opción | Tiempo |
|--------|--------|
| Rápida (temporal + termistor stock) | ~4 horas |
| Completa (definitivo + PT100 + Omron) | ~6 horas |

**Distribución:**
- Documentación: 30-45 min
- Fabricación cable CAN: 1-1.5h
- Montaje EBB42: 30-60 min
- Migración componentes: 1.5-2h
- Cable CAN instalación: 45-60 min
- Verificación: 30-45 min

---

## Material Necesario

📋 **Ver checklist completo:** [MATERIALS_CHECKLIST.md](../../guides/phase3/MATERIALS_CHECKLIST.md)

### Resumen Rápido - MONTAJE TEMPORAL

**Hardware:**
- [x] BTT EBB42 CAN V1.2 (flasheada ✅)
- [x] **BIGTREETECH Eddy Coil V1.0** (instalado ✅)
- [x] Thermistor stock NTC 100K
- [x] Ventiladores stock (2x)

**Cables (VERSIÓN CORTA - Montaje temporal):**
- [x] Cable USB-C a USB-C **~30-50cm** (datos) - SKR ↔ EBB42 ✅
- [x] Cable 2x1.5mm² para 24V **~30-50cm** - SKR ↔ EBB42 ✅
- [x] Cable I2C (4 hilos, incluido con Eddy Coil) - EBB42 ↔ Eddy Coil ✅
- [x] Termorretráctil rojo/azul (varios tamaños)

**Conectores:**
- [ ] JST-XH 2-pin (x3 sets: 24V, heater, thermistor)
- [ ] JST-XH 3-pin (x1 set: probe Omron)
- [ ] Dupont 2-pin (x2 sets: fans)

**Herramientas:**
- [ ] Multímetro (CRÍTICO)
- [ ] Crimpadora JST/Dupont
- [ ] Destornilladores
- [ ] Pistola silicona + hot glue
- [ ] Cinta doble cara o bridas (montaje EBB42 temporal)

---

## Reglas de Seguridad

1. **NUNCA** trabajar con impresora energizada (excepto testing supervisado)
2. **SIEMPRE** verificar polaridad 24V con multímetro ANTES de conectar
3. **NUNCA** forzar conectores en EBB42
4. **SIEMPRE** verificar continuidad y ausencia de cortocircuitos
5. **SIEMPRE** dejar holgura para movimientos toolhead
6. **NUNCA** calentar hotend sin supervisión
7. **SIEMPRE** probar probe con `QUERY_PROBE` antes de `G28 Z`

---

## Punto de Rollback

Si algo falla:
- Toolhead stock documentado con fotos
- Cables stock identificados
- Sistema puede volver a configuración Phase 2

---

## Enlaces Útiles

- **📘 Guía Completa:** [`EBB42_INTEGRATION.md`](../../guides/phase3/EBB42_INTEGRATION.md)
- **📋 Checklist Materiales:** [`MATERIALS_CHECKLIST.md`](../../guides/phase3/MATERIALS_CHECKLIST.md)
- **Phase anterior:** [Phase 2](../phase2/README.md)
- **Phase siguiente:** Phase 4 - Firmware Klipper

---

## 🚀 Próximos Pasos para Implementar

### 0. SKR 1.4 Turbo (COMPLETADO ✅)
- [x] Identificar chip correcto (LPC1769FBD100)
- [x] Compilar firmware Klipper con configuración LPC176x
- [x] Flashear SKR vía tarjeta SD
- [x] Verificar detección USB (`usb-Klipper_lpc1769_12345-if00`)
- [x] Configurar printer.cfg con serial ID
- [x] Verificar comunicación bidireccional Klipper

**Ver detalles completos:** [FLASHEO_SKR_EXITOSO.md](../../guides/phase3/FLASHEO_SKR_EXITOSO.md)

### 1. Preparación EBB42 (COMPLETADO ✅)
- [x] Leer completamente [`EBB42_INTEGRATION.md`](../../guides/phase3/EBB42_INTEGRATION.md)
- [x] Verificar materiales con [`MATERIALS_CHECKLIST.md`](../../guides/phase3/MATERIALS_CHECKLIST.md)
- [x] Compilar firmware Klipper para EBB42 (USB mode)
- [x] Flashear EBB42 y verificar detección USB
- [x] Configurar dual-MCU en printer.cfg
- [x] Verificar ambas MCUs detectadas en Klipper

**Ver detalles completos:** [FLASHEO_EBB42_EXITOSO.md](../../guides/phase3/FLASHEO_EBB42_EXITOSO.md)

### 2. Documentación Stock ✅
- [x] Fotografiar toolhead actual (10+ fotos, todos los ángulos)
- [x] Fotografiar conexiones actuales en frame superior
- [x] Etiquetar todos los cables existentes

### 3. Fabricación Cables (Versión Temporal) ✅
- [x] Cable 24V corto (~30-50cm): SKR FAN2 → EBB42 VIN
- [x] Cable USB-C corto (~30-50cm): SKR → EBB42
- [x] Cable I2C (incluido): EBB42 → Eddy Coil
- [x] Verificar conectores JST-XH/Dupont en cables stock

### 4. Montaje Físico ✅
- [x] Montar EBB42 cerca de SKR (cinta doble cara + bridas)
- [x] Conectar cables cortos: USB-C y 24V (SKR ↔ EBB42)
- [x] Conectar cables stock toolhead a EBB42 (heater, therm, fans)
- [x] Instalar Eddy Coil (I2C a EBB42)

### 5. Verificación y Testing ⏳
- [x] Heater funcionando (PB13)
- [x] Thermistor leyendo temperatura
- [x] Fans operativos (PA0, PA1)
- [ ] Eddy Coil calibrado (drive current + Z offset)
- [ ] Homing completo G28
- [ ] Bed mesh leveling

---

## 📝 Configuración y Calibración - Phase 3 (2025-12-26)

### Estado Configuración: ⚙️ EN PROGRESO

Esta sección documenta la **configuración de software** y **calibración** completa del sistema Phase 3.

---

### 🔧 Configuración Klipper Completada

**Fecha:** 2025-12-26

**Archivo configurado:** `/Users/mjcuadrado/projects/x5sa-skr-klipper/klipper/config/printer.cfg`

#### Cambios Principales

**1. Eddy Coil Rapid Scan Optimizado**
- ✅ `speed: 120` (optimizado para rapid_scan)
- ✅ `lift_speed: 60` (movimientos rápidos)
- ✅ `horizontal_move_z: 2` (crítico para rapid_scan)
- ✅ Configuración I2C completa (EBB42 PB3/PB4)

**2. Bed Mesh Adaptativo**
- ✅ `probe_count: 5, 5` (25 puntos, ~15 segundos)
- ✅ `scan_overshoot: 8` (requerido para rapid_scan)
- ✅ `adaptive_margin: 5` (mesh adaptativo por pieza)
- ✅ Algoritmo bicubic para interpolación

**Decisión Técnica: ¿Por qué 5×5 y NO 50×50?**
- 5×5 (25 puntos) + bicubic interpolation = precisión óptima
- 50×50 (2500 puntos) = >15 minutos (vs 15 segundos)
- Adaptive meshing solo sondea área de impresión + margen
- Referencia: Klipper docs + Ellis' guide recomiendan 5×5 a 7×7

**3. Macros de Producción Implementados**

**PRINT_START:**
- Heating bed + precalentar extrusor 150°C
- G28 homing
- Z_TILT_ADJUST (dual Z leveling)
- **BED_MESH_CALIBRATE METHOD=rapid_scan ADAPTIVE=1** (~15 seg)
- Purge line (30mm filamento)

**PRINT_END:**
- Safe Z retract
- Parking trasero
- Apagar heaters/fans
- Retract 5mm
- Clear bed mesh

**PAUSE / RESUME / CANCEL_PRINT:**
- Gestión completa de pausas
- Retracción automática
- Parking seguro

**4. Macros de Calibración**

Disponibles:
- `CALIBRATE_EDDY_CURRENT` - Calibración completa Eddy Coil
- `GENERATE_BED_MESH` - Mesh completo con rapid_scan
- `PID_TUNE_HOTEND TARGET=200` - PID hotend
- `PID_TUNE_BED TARGET=60` - PID bed
- `CALIBRATE_EXTRUDER` - Helper E-steps
- `PRESSURE_ADVANCE_CALIBRATION` - Helper PA tuning
- `RETRACTION_CALIBRATION` - Helper retraction tuning
- `TEST_SPEED` - Test velocidad/aceleración (Ellis)
- `VERIFY_EDDY_PROBE` - Safety check probe

**5. Firmware Retraction**
- ✅ Habilitado: `retract_length: 1.0mm @ 40mm/s`
- ✅ Control centralizado en Klipper
- ✅ Ajustes en tiempo real sin re-slicear

**Ver documentación completa:** [CONFIGURACION_KLIPPER.md](../../guides/phase3/CONFIGURACION_KLIPPER.md)

---

### 🎨 Perfiles OrcaSlicer Creados

**Fecha:** 2025-12-26

**Ubicación:** `/Users/mjcuadrado/projects/x5sa-skr-klipper/orca-slicer-profiles/`

#### Perfiles Disponibles

**Printer Profile:**
- `printer_tronxy_x5sa_klipper.json`
- Build volume: 330×330×400mm
- Integración completa macros Klipper
- Firmware retraction habilitado

**Filament Profiles:**
- `filament_pla.json` - 210°C / 60°C bed, cooling 100%
- `filament_petg.json` - 240°C / 80°C bed, cooling 20-50%
- `filament_abs.json` - 250°C / 100°C bed, cooling 0-30% (⚠️ requiere enclosure)

**Process Profiles:**
- `process_draft.json` - 0.30mm Draft (rápido, prototipos)
- `process_standard.json` - 0.20mm Standard (uso general)
- `process_fine.json` - 0.10mm Fine (miniaturas, detalles)

#### Características Clave

- ✅ **Start G-code:** `PRINT_START BED_TEMP=[...] EXTRUDER_TEMP=[...]`
- ✅ **End G-code:** `PRINT_END`
- ✅ **Adaptive bed meshing** automático (~15 seg/impresión)
- ✅ **Firmware retraction** (G10/G11)
- ✅ **Pressure advance** configurado por filamento
- ✅ **Exclude objects** habilitado
- ✅ **Preparado multicolor** (Phase 12)

**Ver guía completa:** [ORCA_SLICER_SETUP.md](../../guides/phase3/ORCA_SLICER_SETUP.md)

---

### 📋 Checklist de Calibración

**Estado:** ⏳ Pendiente ejecución

#### Calibraciones Obligatorias (ANTES de primera impresión)

- [ ] **1. PID Hotend** - `PID_TUNE_HOTEND TARGET=210`
- [ ] **2. PID Bed** - `PID_TUNE_BED TARGET=60`
- [ ] **3. E-steps** - Calibrar rotation_distance
- [ ] **4. Eddy Drive Current** - `LDC_CALIBRATE_DRIVE_CURRENT` (solo 1 vez)
- [ ] **5. Z Offset** - `CALIBRATE_EDDY_CURRENT`
- [ ] **6. Z-Tilt Adjust** - Nivelar dual Z
- [ ] **7. Bed Mesh** - `GENERATE_BED_MESH`

**Tiempo estimado:** 2.5 - 4 horas

#### Calibraciones Recomendadas (mejoran calidad)

- [ ] **8. Pressure Advance** - Por tipo de filamento (PLA, PETG, ABS)
- [ ] **9. Retraction** - Si hay stringing
- [ ] **10. Input Shaper** - Reduce ringing (requiere ADXL345)

**Tiempo estimado adicional:** 1 - 2 horas

**Ver guía paso a paso:** [CALIBRACION_COMPLETA.md](../../guides/phase3/CALIBRACION_COMPLETA.md)

---

### 📚 Documentación Configuración Phase 3

| Documento | Tema | Estado |
|-----------|------|--------|
| [CONFIGURACION_KLIPPER.md](../../guides/phase3/CONFIGURACION_KLIPPER.md) | Configuración printer.cfg detallada | ✅ Completo |
| [ORCA_SLICER_SETUP.md](../../guides/phase3/ORCA_SLICER_SETUP.md) | Instalación y uso OrcaSlicer | ✅ Completo |
| [CALIBRACION_COMPLETA.md](../../guides/phase3/CALIBRACION_COMPLETA.md) | Checklist y procedimientos calibración | ✅ Completo |
| [EDDY_COIL_CALIBRATION.md](../../guides/phase3/EDDY_COIL_CALIBRATION.md) | Calibración específica Eddy Coil | ✅ Existente |

---

### 🎯 Decision Log - Configuración

#### Decisión: Rapid Scan con 5×5 Grid (NO 50×50)

**Fecha:** 2025-12-26

**Contexto:**
- Eddy Coil V1.0 soporta modo rapid_scan (muestreo continuo)
- Decisión crítica: tamaño del grid de sondeo

**Opciones evaluadas:**

**Opción A: 5×5 grid (25 puntos)**
- ✅ Tiempo: ~15 segundos con rapid_scan
- ✅ Suficiente para bicubic interpolation
- ✅ Recomendado por Klipper docs + Ellis' guide
- ✅ Adaptive meshing reduce aún más (3×3 para piezas pequeñas)

**Opción B: 50×50 grid (2500 puntos)**
- ❌ Tiempo: ~15-20 minutos con rapid_scan
- ❌ Innecesario para camas tramadas correctamente
- ❌ Desgaste excesivo del probe
- ❌ Sin mejora real en calidad vs 5×5 + bicubic

**Decisión:** 5×5 grid + adaptive meshing

**Razones:**
1. Tiempo de inicio de impresión crítico (~15 seg acceptable)
2. Bicubic interpolation compensa con precisión
3. Adaptive meshing optimiza por pieza
4. Soporte técnico de comunidad Klipper

**Implementación:**
```ini
[bed_mesh]
probe_count: 5, 5
algorithm: bicubic
adaptive_margin: 5
```

**Referencias:**
- Klipper bed_mesh docs
- Ellis' Print Tuning Guide
- [HARDWARE_EVOLUTION.md](../../HARDWARE_EVOLUTION.md) - Decisión Eddy Coil

---

**Inicio planificación:** 2025-12-21
**Decisiones completadas:** 2025-12-21
**Implementación hardware:** 2025-12-21
**Upgrade Eddy Coil:** 2025-12-26
**Configuración software:** 2025-12-26 ✅
**Estado:** ✅ Hardware instalado + Software configurado - ⏳ Pendiente calibración
