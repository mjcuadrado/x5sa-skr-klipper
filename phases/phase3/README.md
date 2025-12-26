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

**Inicio planificación:** 2025-12-21
**Decisiones completadas:** 2025-12-21
**Implementación hardware:** 2025-12-21
**Upgrade Eddy Coil:** 2025-12-26
**Estado:** ✅ Hardware instalado - Pendiente calibración Eddy Coil
