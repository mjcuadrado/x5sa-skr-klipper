# Arquitectura Phase 3 - Estado Production-Ready
**Proyecto:** Tronxy X5SA Klipper Migration
**Versión:** Phase 3 (Estado Estable)
**Fecha:** 2025-12-27

---

## 🎯 Filosofía de Phase 3

**Phase 3 NO es una fase temporal o transitoria.**

Este es un **estado PRODUCTION-READY completamente funcional** donde el usuario puede:
- ✅ Imprimir indefinidamente con excelente calidad
- ✅ Usar todas las capacidades del Eddy Coil (rapid scan, adaptive mesh)
- ✅ Disfrutar de macros avanzadas nivel Voron
- ✅ Permanecer en este estado sin urgencia de actualizar

**Phase 12 (Voron toolhead) es un UPGRADE OPCIONAL**, no una necesidad.

---

## 🏗️ Arquitectura Actual Phase 3

### **Diagrama de Sistema**

```
┌─────────────────────────────────────────────────────────────┐
│                    TRONXY X5SA - PHASE 3                    │
│                  Estado Production-Ready                     │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────┐
│   POWER SUPPLY      │
│   24V @ 20A         │
└──────┬──────────────┘
       │
       ├──────────────────────────────────────┐
       │                                      │
       ▼                                      ▼
┌──────────────────┐              ┌─────────────────────┐
│  SKR 1.4 TURBO   │              │   HEATED BED        │
│  (LPC1769)       │              │   24V @ 200W        │
│                  │              │   Thermistor: P0.25 │
│  • 5× TMC2209    │◄─────USB────┤   Heater: P2.5      │
│  • X/Y/Z/Z1/E    │              └─────────────────────┘
│  • Sensorless XY │
│  • Dual Z        │
└────────┬─────────┘
         │
         │ USB Mode (NO CAN)
         │
         ▼
┌──────────────────────────────────────────────────────────┐
│               EBB42 CAN V1.2 (STM32G0B1)                 │
│               Ubicación: DETRÁS del frame                │
│               Conexión: USB to SKR                       │
└──────────────────────────────────────────────────────────┘
         │
         │ Cable bundle through drag chain
         │
         ▼
┌──────────────────────────────────────────────────────────┐
│                    TOOLHEAD ZONE                         │
│                                                          │
│  ┌─────────────────────────────────────────────┐        │
│  │  STOCK TRONXY TOOLHEAD                      │        │
│  │  (E3D V6 Clone Hotend)                      │        │
│  ├─────────────────────────────────────────────┤        │
│  │                                             │        │
│  │  [Eddy Coil V1.0]  ◄─── 2-3mm ───┐          │        │
│  │         │                        │          │        │
│  │         │                    [Nozzle]       │        │
│  │         │                        │          │        │
│  │  I2C: SCL/SDA ──┐               │          │        │
│  │  Power: VCC/GND ─┤               │          │        │
│  │                  │               │          │        │
│  │                  │    Heater ────┤          │        │
│  │                  │    Thermistor─┤          │        │
│  │                  │    Fan Part Cooling─┤    │        │
│  │                  │    Fan Hotend──────┤     │        │
│  │                  │                     │    │        │
│  │                  └─────────────────────┤    │        │
│  │                                        │    │        │
│  │        Cables bajando por drag chain  │    │        │
│  └────────────────────────────────────────┼────┘        │
│                                           │             │
│           ┌───────────────────────────────┘             │
│           │ DRAG CHAIN (Cadena Portacables)             │
│           │ Contenido:                                  │
│           │ • Eddy Coil: VCC, GND, SCL, SDA             │
│           │ • Heater: 2 wires (24V)                     │
│           │ • Thermistor: 2 wires                       │
│           │ • Part cooling fan: 2 wires                 │
│           │ • Hotend fan: 2 wires                       │
│           │ • (Total: ~12 wires organizados)            │
│           │                                             │
│           └──────────┬──────────────────────────────────┘
│                      │
│                      ▼
│           Llegada a zona EBB42 (detrás)
└──────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────┐
│              EBB42 CONNECTIONS (Behind Frame)            │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  Eddy Coil I2C:                                          │
│  ├─ SCL ──► PB3                                          │
│  ├─ SDA ──► PB4                                          │
│  ├─ VCC ──► 5V/3.3V (verificar jumper EBB42)             │
│  └─ GND ──► GND                                          │
│                                                          │
│  Hotend:                                                 │
│  ├─ Heater ──► PB13 (EBB42 V1.2 heater out)              │
│  └─ Thermistor ──► PA3 (TH0)                             │
│                                                          │
│  Fans:                                                   │
│  ├─ Part Cooling ──► PA0                                 │
│  └─ Hotend Fan ──► PA1                                   │
│                                                          │
└──────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────┐
│               EXTRUDER (Phase 3 Location)                │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  Motor: SKR E0 Port (P2.13)                              │
│  Driver: TMC2209 @ 650mA                                 │
│  Type: Titan Clone (gear_ratio 66:22)                    │
│  Location: FRAME-MOUNTED (not on toolhead)               │
│                                                          │
│  Bowden tube: ~60cm from extruder to hotend              │
│                                                          │
│  ⚠️ NOTE: En Phase 12 migrará a Orbiter 2.5 en EBB42      │
│           (direct drive en toolhead)                     │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

---

## 📊 Comparación Phase 3 vs Phase 12

| Aspecto | **Phase 3 (Actual)** | **Phase 12 (Futuro)** |
|---------|---------------------|----------------------|
| **Toolhead** | Stock Tronxy | Voron Stealthburner |
| **Hotend** | E3D V6 Clone | Dragonfly BMO |
| **Extrusor** | Titan clone (frame) | Orbiter 2.5 (toolhead) |
| **Ubicación EBB42** | Detrás del frame | Integrada EN toolhead |
| **Cable Eddy** | A través cadena (~60cm) | Corto dentro cabezal (~10cm) |
| **Bowden** | Sí (~60cm) | No (direct drive) |
| **Peso toolhead** | ~800g | ~600g (Voron optimizado) |
| **Multicolor** | No | Sí (preparado para MMU) |
| **Estado** | ✅ **Production-Ready** | 🔮 Futuro opcional |
| **Complejidad** | Media | Alta |
| **Impresión calidad** | Excelente | Excelente++ |

---

## 🔌 Gestión de Cables Phase 3

### **Cable Bundle Composición**

```
CADENA PORTACABLES (Drag Chain)
│
├─ Eddy Coil (4 wires):
│  ├─ VCC (rojo) ────────────► EBB42 5V/3.3V
│  ├─ GND (negro) ───────────► EBB42 GND
│  ├─ SCL (amarillo/blanco) ─► EBB42 PB3
│  └─ SDA (verde/azul) ──────► EBB42 PB4
│
├─ Heater (2 wires):
│  └─ 24V heater cartridge ──► EBB42 PB13
│
├─ Thermistor (2 wires):
│  └─ NTC 100K ──────────────► EBB42 PA3
│
├─ Part Cooling Fan (2 wires):
│  └─ 24V fan ───────────────► EBB42 PA0
│
└─ Hotend Fan (2 wires):
   └─ 24V fan ───────────────► EBB42 PA1

Total: ~12 wires organizados en drag chain
```

### **Características del Routing**

✅ **Holgura adecuada:** Cables tienen slack para movimiento completo XY
✅ **Protección mecánica:** Drag chain evita enredos y daños
✅ **Organización:** Cable ties cada 10-15cm dentro de la cadena
✅ **Sin tensión:** Movimientos a máxima velocidad no tensan cables
✅ **Mantenible:** Fácil acceso para troubleshooting o cambios

---

## ⚡ Características Phase 3

### **Capacidades Completas**

✅ **Bed Meshing Ultra-Rápido**
- 5×5 mesh en ~15 segundos con rapid_scan
- Adaptive meshing (solo área de impresión)
- ±0.01mm precisión con Eddy Coil

✅ **Macros Avanzadas Nivel Voron**
- PRINT_START con adaptive mesh automático
- PRINT_END, PAUSE/RESUME/CANCEL
- Integración completa con OrcaSlicer

✅ **Dual Independent Z**
- Z_TILT_ADJUST automático
- Compensación de desalineación mecánica

✅ **Sensorless Homing XY**
- Sin endstops físicos (TMC2209 StallGuard)
- Homing confiable y repetible

✅ **Firmware Retraction**
- G10/G11 con tuning en tiempo real
- Optimizado por filamento

✅ **Pressure Advance**
- Compensación esquinas/aceleraciones
- Calidad de impresión nivel Voron

---

## 🚫 Limitaciones Phase 3 (vs Phase 12)

❌ **No multicolor** - Solo single-color
❌ **Bowden extruder** - Flexible filament limitado
❌ **Peso toolhead** - Mayor que Voron (~800g vs ~600g)
❌ **Cables largos** - A través de drag chain (más susceptibles a EMI)
❌ **No Stealthburner features** - Sin LED Neopixel, sin housing Voron

**PERO:** Para single-color PLA/PETG/ABS, **Phase 3 es perfectamente suficiente**.

---

## 🔧 Mantenimiento Phase 3

### **Tareas Regulares**

| Tarea | Frecuencia | Complejidad |
|-------|------------|-------------|
| Verificar tensión cables | Mensual | Baja |
| Limpiar drag chain | Trimestral | Baja |
| Re-generar bed mesh | Mensual o si cambia cama | Baja (15 seg) |
| Z_TILT_ADJUST | Semanal o si cambias algo en Z | Baja (~2 min) |
| Calibrar PA por filamento nuevo | Cuando cambias marca/tipo | Media (~30 min) |
| PID tuning si cambias nozzle/heater | Solo si cambias hardware | Media (~20 min) |

### **Troubleshooting Común**

**Problema:** Primera capa irregular en una esquina
**Solución:** `Z_TILT_ADJUST` + `GENERATE_BED_MESH` (cama caliente)

**Problema:** Stringing excesivo
**Solución:** Calibrar retraction con torre (RETRACTION_CALIBRATION)

**Problema:** Esquinas "bulging" (abultadas)
**Solución:** Calibrar pressure advance (PRESSURE_ADVANCE_CALIBRATION)

**Problema:** Eddy no detecta metal
**Solución:** Verificar cables I2C no dañados, `QUERY_PROBE` test

---

## 🎯 ¿Cuándo Migrar a Phase 12?

**Considera Phase 12 (Voron toolhead) si:**
- ✅ Quieres imprimir multicolor (MMU/AMS)
- ✅ Necesitas imprimir flexibles frecuentemente (TPU, TPE)
- ✅ Buscas reducir peso del toolhead (~25% menos)
- ✅ Quieres estética Voron + LEDs Neopixel
- ✅ Planeas input shaper extremo (>10k mm/s² accel)

**Quédate en Phase 3 si:**
- ✅ Solo imprimes single-color
- ✅ PLA/PETG/ABS son suficientes
- ✅ La impresora actual funciona perfectamente
- ✅ No quieres gastar en Orbiter 2.5 + Dragonfly BMO (~€200)
- ✅ Prefieres simplicidad y confiabilidad

---

## 📦 Bill of Materials - Phase 3

**Hardware Actual Instalado:**

| Componente | Modelo | Ubicación | Estado |
|------------|--------|-----------|--------|
| Main MCU | SKR 1.4 Turbo | Frame (detrás) | ✅ Instalado |
| Toolhead MCU | EBB42 CAN V1.2 | Frame (detrás) | ✅ Instalado |
| Probe | Eddy Coil V1.0 | Toolhead | ✅ Instalado |
| Hotend | E3D V6 Clone | Toolhead | ✅ Stock |
| Extruder | Titan Clone | Frame | ✅ Stock |
| Drivers | 5× TMC2209 UART | SKR | ✅ Instalado |
| Heated Bed | Stock 330×330mm | Bed | ✅ Stock |
| Drag Chain | Stock Tronxy | X-axis | ✅ Stock |

**Accesorios Necesarios:**
- Cable ties (organización en drag chain)
- Cable 4-wire para Eddy Coil (~80cm)
- Bracket Eddy Coil (impreso 3D o metal)

---

## 🔮 Roadmap Futuro (Phase 12)

**Componentes a Adquirir:**
- [ ] Voron Stealthburner toolhead (impreso)
- [ ] Orbiter 2.5 extruder (~€80)
- [ ] Dragonfly BMO hotend (~€80)
- [ ] Cable corto Eddy Coil (~15cm)
- [ ] Neopixel LEDs (opcional)
- [ ] Clockwork2 gears (si Orbiter no incluye)

**Estimado costo:** €150-200
**Tiempo instalación:** 4-6 horas
**Complejidad:** Alta

---

## ✅ Conclusión

**Phase 3 es un estado PRODUCTION-READY estable y completamente funcional.**

No es "temporal", no es "work-in-progress", no es "hasta que...".

Es una **configuración sólida** donde puedes:
- Imprimir con calidad profesional
- Disfrutar de todas las ventajas del Eddy Coil
- Usar macros avanzadas nivel Voron
- Permanecer indefinidamente si así lo deseas

**Phase 12 es un upgrade OPCIONAL para casos de uso específicos** (multicolor, flexibles, estética Voron).

---

**Documentación relacionada:**
- [CONFIGURACION_KLIPPER.md](CONFIGURACION_KLIPPER.md) - Configuración completa
- [GUIA_FOTOGRAFICA_INSTALACION.md](GUIA_FOTOGRAFICA_INSTALACION.md) - Instalación física
- [CALIBRACION_COMPLETA.md](CALIBRACION_COMPLETA.md) - Todas las calibraciones
- [ORCA_SLICER_SETUP.md](ORCA_SLICER_SETUP.md) - Perfiles slicer
- [../../HARDWARE_EVOLUTION.md](../../HARDWARE_EVOLUTION.md) - Evolución del proyecto
