# Phase 3 — Toolhead EBB42 USB

**Objetivo:** Instalar placa EBB42 en el toolhead y conectar todos los componentes del extrusor/hotend. Establecer comunicación USB con SKR 1.4 Turbo.

**Estado:** ✅ Decisiones tomadas - Listo para implementación (2025-12-21)

---

## 📚 Documentación Principal

### 🎯 Guía Completa de Integración
📘 **[EBB42_INTEGRATION.md](../../guides/phase3/EBB42_INTEGRATION.md)**
- ✅ Todas las decisiones tomadas y justificadas
- 📋 Pinout completo de EBB42
- 🔧 Plan de cableado detallado paso a paso
- ⚙️ Configuración Klipper completa
- 📝 Plan de implementación con 8 fases
- 🛡️ Troubleshooting y seguridad

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

### EBB42 - PENDIENTE ⏳
- ⏳ **Próximo paso:** Flashear firmware Klipper (USB mode)
- ⏳ **Configuración:** Pendiente compilación y flasheo
- ⏳ **Integración:** Pendiente cableado y montaje físico

---

## ✅ Decisiones Arquitectónicas Tomadas

### 1. Conexión: USB (NO CAN)
- ✅ Cable USB-C desde SKR a EBB42
- ✅ Simplicidad de configuración
- ✅ No requiere transceiver CAN

### 2. Ubicación: Toolhead
- ✅ EBB42 montada en toolhead
- ✅ Phase 3-11: Montaje provisional (cinta + bridas)
- ✅ Phase 12: Montaje definitivo en Stealthburner

### 3. Alimentación: 24V desde SKR
- ✅ Cable dedicado 24V desde FAN2/HE1 de SKR
- ✅ Always-on (100%)
- ✅ Capacidad sobrada (~0.6A usados de 1-2A disponibles)

### 4. Conectores: Enfoque Mixto
- ✅ JST-XH: Críticos (24V, hotend, thermistor, probe)
- ✅ Dupont + hot glue: Ventiladores

### 5. Ventiladores
- ✅ FAN0 → Part cooling (controlado PWM)
- ✅ FAN1 → Hotend fan (always-on T>50°C)

### 6. Probe: Omron NC (Fail-Safe)
- ✅ Configuración Normally Closed
- ✅ Fallo del cable = error inmediato, NO imprime
- ✅ Bed leveling garantizado funcional

### 7. Motor Extrusor
- ⚠️ Phase 3-11: Motor E en SKR E0 (lateral)
- ⏭️ Phase 12: Migra a EBB42 con Orbiter v2

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
- [ ] Hardware confirmado:
  - [ ] BTT EBB42 CAN V1.2
  - [x] Sensor Omron TL-Q5MC1-Z (instalado)
  - [x] Thermistor NTC 100K stock (usar actual)
  - [ ] Cable USB-C a USB-C (~2m)
  - [ ] Cable alimentación 2x1.5mm² (~2m)
  - [ ] 2x Anillos ferrita
- [ ] Herramientas: destornilladores, multímetro, crimpadora
- [ ] Firmware Klipper para EBB42 compilado (USB mode)

---

## Objetivo al Finalizar Phase 3

### Hardware a Instalar

- [ ] EBB42 montada en toolhead (provisional)
- [ ] Calentador hotend → EBB42 HE
- [ ] Thermistor NTC 100K → EBB42 TH0
- [ ] Ventilador part cooling → EBB42 FAN0
- [ ] Ventilador hotend → EBB42 FAN1
- [ ] Sensor Omron → EBB42 PROBE
- [ ] Cable USB tendido y conectado (SKR ↔ EBB42)
- [ ] Cable 24V tendido y conectado (SKR ↔ EBB42)

### NO Migrado en Phase 3
- ❌ Motor extrusor → **Se queda en SKR E0** hasta Phase 12

### Arquitectura Final Phase 3

```
┌─────────────────────────────────────────┐
│ SKR 1.4 TURBO (Frame Superior)          │
│                                         │
│ USB    → Cable USB-C → EBB42            │
│ FAN2   → 24V (always-on) → EBB42        │
│ E0     ← Motor Extrusor (se queda aquí) │
└─────────────────────────────────────────┘
         ↓
    USB-C + 24V (cable chain)
         ↓
┌─────────────────────────────────────────┐
│ EBB42 USB (Toolhead - Provisional)      │
│                                         │
│ VIN/GND ← 24V Power                     │
│ USB-C   ← USB desde SKR                 │
│ HE      ← Calentador hotend             │
│ TH0     ← Thermistor NTC 100K           │
│ FAN0    ← Part cooling (PWM control)    │
│ FAN1    ← Hotend fan (auto T>50°C)      │
│ PROBE   ← Sensor Omron NC (fail-safe)   │
│ E0      ← (sin usar hasta Phase 12)     │
└─────────────────────────────────────────┘
```

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

### Resumen Rápido

**Hardware:**
- [ ] BTT EBB42 CAN V1.2
- [x] Sensor Omron TL-Q5MC1-Z (instalado)
- [x] Thermistor stock NTC 100K
- [x] Ventiladores stock (2x)

**Cables:**
- [ ] Cable USB-C a USB-C (~2m, datos)
- [ ] Cable 2x1.5mm² para 24V (~2m)
- [ ] 2x Anillos ferrita
- [ ] Termorretráctil rojo/azul

**Conectores:**
- [ ] JST-XH 2-pin (x4 sets)
- [ ] JST-XH 3-pin (x1 set)
- [ ] Dupont 2-pin (x2 sets)

**Herramientas:**
- [ ] Multímetro (CRÍTICO)
- [ ] Crimpadora JST/Dupont
- [ ] Destornilladores
- [ ] Pistola silicona + hot glue
- [ ] Cinta doble cara (montaje EBB42)

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

### 1. Preparación EBB42
- [ ] Leer completamente [`EBB42_INTEGRATION.md`](../../guides/phase3/EBB42_INTEGRATION.md)
- [ ] Verificar materiales con [`MATERIALS_CHECKLIST.md`](../../guides/phase3/MATERIALS_CHECKLIST.md)
- [ ] Compilar firmware Klipper para EBB42 (USB mode)
- [ ] Flashear EBB42 y verificar detección USB

### 2. Documentación Stock
- [ ] Fotografiar toolhead actual (10+ fotos)
- [ ] Etiquetar todos los cables existentes

### 3. Fabricación Cables
- [ ] Fabricar cable 24V (termorretráctil rojo/azul)
- [ ] Preparar cable USB con ferritas
- [ ] Crimpar conectores en componentes stock

### 4. Implementación
- [ ] Seguir plan detallado en `EBB42_INTEGRATION.md` Fases 1-8

---

**Inicio planificación:** 2025-12-21
**Decisiones completadas:** 2025-12-21
**Estado:** ✅ Listo para implementación física
