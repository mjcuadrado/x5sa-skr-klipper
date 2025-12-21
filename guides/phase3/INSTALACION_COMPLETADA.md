# Phase 3 - Instalación Completada

**Fecha:** 2025-12-21
**Estado:** ✅ Hardware conectado - Listo para testing

---

## 📋 Resumen de la Instalación

Se completó la instalación del hardware stock del toolhead en la configuración temporal con EBB42 montada junto a SKR en el frame superior.

---

## 🎯 Arquitectura Implementada

### Montaje Temporal - Frame Superior

```
┌────────────────────────────────────────────────────────┐
│ FRAME SUPERIOR                                         │
│                                                        │
│  ┌──────────────┐  USB-C    ┌───────────────────┐     │
│  │ SKR 1.4 TB   │ ←(cable)→ │ EBB42 (TEMPORAL)  │     │
│  │              │           │                   │     │
│  │ FAN0:        │           │ HE      ← Heater  │ ←── Cables stock
│  │ ├─ VIN ──────┼───────────┼─→ Tronxy +24V    │     toolhead
│  │ └─ GND ──────┼───────────┼─→ Tronxy GND     │     (llegan aquí)
│  │              │           │ TH0     ← Therm   │ ←──
│  │              │           │ FAN1    ← HE fan  │ ←──
│  │              │           │ FAN2    ← Part fan│ ←──
│  │              │           │ PA5     ← Tronxy  │ ←── Signal
│  │              │           │           Signal  │
│  └──────────────┘           └───────────────────┘
│  24V desde PSU               24V desde PSU
└────────────────────────────────────────────────────────┘
                                                     ↓
                                      Cable sensor Tronxy
                                                     ↓
                                            ┌────────────┐
                                            │ TOOLHEAD   │
                                            │ (stock)    │
                                            │            │
                                            │ Tronxy     │
                                            │ XY-08N     │
                                            └────────────┘
```

---

## ✅ Hardware Conectado

### En EBB42:

| Componente | Cable | Conector EBB42 | Estado |
|------------|-------|----------------|--------|
| **Heater hotend** | Blanco con rayas grises | Terminales tornillo HE | ✅ Conectado |
| **Thermistor** | Blanco delgado | TH0 (2-pin JST) | ✅ Conectado |
| **Ventilador hotend** | Negro/Rojo | FAN1 (PA0) | ✅ Conectado |
| **Ventilador part cooling** | Negro/Azul | FAN2 (PA1) | ✅ Conectado |
| **Sensor Tronxy (signal)** | Negro | PA5 (PROBE) | ✅ Conectado |

### En SKR 1.4 Turbo:

| Componente | Cable | Conector SKR | Estado |
|------------|-------|--------------|--------|
| **Sensor Tronxy +24V** | Marrón | FAN0 VIN | ✅ Conectado |
| **Sensor Tronxy GND** | Azul | FAN0 GND | ✅ Conectado |
| **USB a EBB42** | USB-C | Puerto USB | ✅ Conectado |

### Alimentación:

| Placa | Fuente | Cable | Estado |
|-------|--------|-------|--------|
| **SKR 1.4 Turbo** | PSU → Switch | 24V directo | ✅ Conectado |
| **EBB42** | PSU directo | 24V definitivo | ✅ Conectado |

---

## 🔧 Sensor Tronxy XY-08N

### Especificaciones:
- **Modelo:** Tronxy XY-08N
- **Tipo:** Sensor inductivo NPN NO (Normally Open)
- **Voltaje:** 6-36V DC
- **Montaje:** Toolhead stock

### Cableado (3 hilos):

```
Tronxy XY-08N          Conexión
├─ MARRÓN (+24V)   →   SKR FAN0: Pin VIN
├─ AZUL (GND)      →   SKR FAN0: Pin GND
└─ NEGRO (Signal)  →   EBB42: Pin PA5 (PROBE)
```

**Configuración Klipper:**
```ini
[probe]
pin: ^EBBCan:PA5  # Pullup habilitado para NPN NO
```

---

## 📸 Documentación Fotográfica

### Fotos del Estado Inicial (ANTES):

**Toolhead:**
- `01_toolhead_general_view.jpeg` - Vista general completa
- `02_part_cooling_fan_blower.jpeg` - Ventilador part cooling (blower radial)
- `03_hotend_heater_block.jpeg` - Hotend con bloque calefactor
- `04_toolhead_side_view.jpeg` - Vista lateral del toolhead
- `05_extruder_blue_box.jpeg` - Extrusor con caja azul (lateral)
- `06_toolhead_extruder_fans.jpeg` - Vista general con ventiladores

**Frame Superior:**
- `07_skr_ebb42_frame_superior.jpeg` - SKR y EBB42 montadas juntas
- `08_skr_ebb42_wiring_detail.jpeg` - Detalle del cableado entre placas

**Sensor:**
- `09_tronxy_xy08n_sensor_blue.jpeg` - Sensor Tronxy XY-08N azul montado

---

## ⚙️ Configuración Klipper

### Archivos Modificados:

**`printer.cfg`:**
- ✅ Actualizado pin del probe: `^EBBCan:PA5`
- ✅ Documentación del sensor Tronxy XY-08N
- ✅ Cableado híbrido EBB42 + SKR documentado

### Secciones Clave:

**Extruder (Phase 3-11):**
```ini
[extruder]
# Motor en SKR E0 (lateral, se queda aquí hasta Phase 12)
step_pin: P2.0
dir_pin: !P0.5
enable_pin: !P2.1
# Heater y thermistor en EBB42
heater_pin: EBBCan:PA2
sensor_pin: EBBCan:PA3
```

**Ventiladores:**
```ini
[fan]
# Part cooling fan - EBB42 FAN2
pin: EBBCan:PA0

[heater_fan hotend_fan]
# Hotend fan - EBB42 FAN1
pin: EBBCan:PA1
heater_temp: 50.0
```

**Probe:**
```ini
[probe]
# Tronxy XY-08N NPN NO
# Alimentación desde SKR FAN0
# Signal a EBB42 PA5
pin: ^EBBCan:PA5
```

---

## 🚨 Notas Importantes

### Sensor Tronxy - Alimentación Híbrida:

⚠️ **CONFIGURACIÓN NO ESTÁNDAR:**
- El sensor Tronxy requiere 24V de alimentación
- La EBB42 NO tiene 24V disponible en el conector PROBE (solo 5V)
- **Solución implementada:** Alimentación desde SKR FAN0, signal a EBB42 PA5

**Implicaciones:**
- El sensor tiene cables que van a DOS placas diferentes
- 24V y GND desde SKR
- Signal a EBB42
- Esto es funcional pero NO es la configuración estándar
- En Phase 12 (Stealthburner) se puede reevaluar

### Motor Extrusor:

⚠️ **UBICACIÓN TEMPORAL:**
- Motor E permanece conectado a SKR E0 (puerto lateral)
- NO migra a EBB42 en Phase 3
- Migración planificada para Phase 12 con Orbiter v2

---

## ✅ Checklist Pre-Testing

Antes de energizar, verificar:

### Hardware:
- [x] Heater conectado a terminales HE de EBB42
- [x] Thermistor conectado a TH0 de EBB42
- [x] Ventilador hotend conectado a FAN1 de EBB42
- [x] Ventilador part cooling conectado a FAN2 de EBB42
- [x] Sensor Tronxy: 24V/GND desde SKR FAN0, signal a EBB42 PA5
- [x] USB-C conectado entre SKR y EBB42
- [x] 24V alimentando SKR desde PSU
- [x] 24V alimentando EBB42 desde PSU
- [x] Motor E conectado a SKR E0

### Software:
- [x] `printer.cfg` actualizado con pin correcto del probe (PA5)
- [x] Documentación del sensor Tronxy completada
- [x] Fotos renombradas y organizadas

### Seguridad:
- [x] Impresora apagada durante instalación
- [x] Todas las conexiones verificadas
- [x] No hay cables sueltos o tocando partes metálicas

---

## 🚀 Próximos Pasos

### Fase 7: Primera Energización

1. **Verificación visual final** de todas las conexiones
2. **Encender impresora** (PSU switch ON)
3. **Verificar LEDs:**
   - SKR: LED power encendido
   - EBB42: LED power encendido
4. **SSH a Raspberry Pi Klipper**
5. **Verificar detección USB:**
   ```bash
   ls /dev/serial/by-id/
   ```
   Debe mostrar:
   - `usb-Klipper_lpc1769_XXXXX-if00` (SKR)
   - `usb-Klipper_stm32g0b1xx_XXXXX-if00` (EBB42)

6. **Restart Klipper:**
   ```bash
   sudo systemctl restart klipper
   ```

7. **Verificar log sin errores:**
   ```bash
   tail -f /tmp/klippy.log
   ```

### Fase 8: Testing Básico

1. **Test Thermistor:** Verificar temperatura ambiente razonable
2. **Test Heater:** Calentar a 50°C supervisado
3. **Test Fans:** PWM part cooling + auto hotend fan
4. **Test Probe:** `QUERY_PROBE` y trigger manual
5. **Test Homing:** `G28` con supervisión

### Fase 9: Calibraciones

1. **PID Hotend:** `PID_CALIBRATE HEATER=extruder TARGET=200`
2. **Probe Z Offset:** `PROBE_CALIBRATE`
3. **Bed Mesh:** `BED_MESH_CALIBRATE`
4. **Test Print:** Primera capa pequeña

---

## 📝 Lecciones Aprendidas

### Decisiones Clave:

1. **Montaje temporal de EBB42 junto a SKR:**
   - Permitió usar cables stock del toolhead sin extensiones
   - Facilitó troubleshooting inicial
   - Solo necesitó 1 cable nuevo (sensor, que es híbrido)

2. **Sensor Tronxy alimentación híbrida:**
   - EBB42 no tiene 24V en PROBE
   - SKR FAN0 proporcionó 24V/GND
   - Signal va a EBB42 PA5
   - Funcional pero no estándar

3. **Cables stock reutilizados:**
   - Heater: directo a terminales tornillo (sin crimpar conector)
   - Thermistor, fans: usaron conectores existentes
   - Ahorro de tiempo y materiales

---

## 🔗 Referencias

- **Guía de Implementación Temporal:** [IMPLEMENTACION_TEMPORAL.md](IMPLEMENTACION_TEMPORAL.md)
- **Flasheo SKR Exitoso:** [FLASHEO_SKR_EXITOSO.md](FLASHEO_SKR_EXITOSO.md)
- **Flasheo EBB42 Exitoso:** [FLASHEO_EBB42_EXITOSO.md](FLASHEO_EBB42_EXITOSO.md)
- **Configuración Dual-MCU:** [CONFIGURACION_DUAL_MCU.md](CONFIGURACION_DUAL_MCU.md)
- **Pinout EBB42:** Diagrama oficial BTT
- **Sensor Tronxy XY-08N:** Datasheet NPN NO 6-36V

---

**Preparado por:** mjcuadrado + Claude Code
**Fecha:** 2025-12-21
**Versión:** 1.0
**Estado:** ✅ Instalación hardware completada - Listo para testing
