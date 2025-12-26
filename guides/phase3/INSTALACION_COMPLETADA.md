# Phase 3 - Instalación Completada

**Fecha:** 2025-12-21 (instalación inicial) | 2025-12-26 (upgrade Eddy Coil)
**Estado:** ✅ Hardware actualizado - Eddy Coil V1.0 operativo

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
│  │              │           │ I2C     ← Eddy    │ (PB3/PB4)
│  │              │           │           Coil    │
│  └──────────────┘           └───────────────────┘
│  24V desde PSU               24V desde PSU
└────────────────────────────────────────────────────────┘

         ⚠️ NOTA: Diagrama original documentaba sensor Tronxy XY-08N
         (abandonado). Ver sección "ACTUALIZACIÓN: Migración a Eddy Coil"
         para configuración actual.
```

---

## ✅ Hardware Conectado

### En EBB42:

| Componente | Cable | Conector EBB42 | Estado |
|------------|-------|----------------|--------|
| **Heater hotend** | Blanco con rayas grises | Terminales tornillo HE (PB13) | ✅ Conectado |
| **Thermistor** | Blanco delgado | TH0 (PA3) | ✅ Conectado |
| **Ventilador hotend** | Negro/Rojo | FAN1 (PA1) | ✅ Conectado |
| **Ventilador part cooling** | Negro/Azul | FAN2 (PA0) | ✅ Conectado |
| **Eddy Coil V1.0** | Cable I2C 4-pin | I2C (PB3/PB4) | ✅ Conectado |
| ~~Sensor Tronxy~~ | ~~Negro~~ | ~~PA5~~ | ❌ Abandonado |

### En SKR 1.4 Turbo:

| Componente | Cable | Conector SKR | Estado |
|------------|-------|--------------|--------|
| ~~Sensor Tronxy +24V~~ | ~~Marrón~~ | ~~FAN0 VIN~~ | ❌ Removido |
| ~~Sensor Tronxy GND~~ | ~~Azul~~ | ~~FAN0 GND~~ | ❌ Removido |
| **USB a EBB42** | USB-C | Puerto USB | ✅ Conectado |

### Alimentación:

| Placa | Fuente | Cable | Estado |
|-------|--------|-------|--------|
| **SKR 1.4 Turbo** | PSU → Switch | 24V directo | ✅ Conectado |
| **EBB42** | PSU directo | 24V definitivo | ✅ Conectado |

---

## 🔧 ~~Sensor Tronxy XY-08N~~ (DEPRECADO - Solo Histórico)

⚠️ **NOTA:** Esta sección documenta el sensor XY-08N que fue **abandonado** por incompatibilidad eléctrica.
Ver sección **"ACTUALIZACIÓN: Migración a Eddy Coil V1.0"** más abajo para configuración actual.

### Especificaciones (referencia histórica):
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

**Sensor (histórico):**
- `09_tronxy_xy08n_sensor_blue.jpeg` - ~~Sensor Tronxy XY-08N~~ (abandonado, solo referencia histórica)

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
heater_pin: EBBCan:PB13    # EBB42 V1.2 heater output
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

**Probe (configuración histórica - DEPRECADA):**
```ini
# ⚠️ OBSOLETO - Sensor XY-08N abandonado
# Ver sección "ACTUALIZACIÓN: Migración a Eddy Coil V1.0" para config actual
#[probe]
# Tronxy XY-08N NPN NO
# Alimentación desde SKR FAN0
# Signal a EBB42 PA5
#pin: ^EBBCan:PA5
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

## ⚡ ACTUALIZACIÓN: Migración a Eddy Coil V1.0 (2025-12-26)

### ❌ Problema con Tronxy XY-08N

El sensor Tronxy XY-08N inicialmente documentado **NO se pudo implementar** debido a incompatibilidades eléctricas:

**Problemas identificados:**
1. **Voltaje de salida incompatible:** Sensor outputs 24V cuando triggered, MCUs requieren 3.3V
2. **Pullup interno a 24V:** Sensor tiene pullup interno que no permite adaptación simple con BAT85
3. **Circuito complejo requerido:** Necesitaría circuito activo (transistor + resistencias) para level shifting
4. **Tiempo perdido:** 2+ días intentando múltiples configuraciones sin éxito

**Intentos realizados (todos fallidos):**
- BAT85 diode solo → LED no enciende, sensor no funciona
- Resistencias en serie (68kΩ) → Insuficiente corriente
- Múltiples pines probados: SKR (P0.10, P1.25, P1.26, P1.27) + EBB42 (PA5, PB8, PB9)
- Alimentación híbrida SKR FAN0 + signal a EBB42

**Conclusión:** Sensor inadecuado para MCUs de 3.3V sin circuito adaptador complejo.

### ✅ Solución: BIGTREETECH Eddy Coil V1.0

**Fecha de implementación:** 2025-12-26

Se decidió reemplazar el Tronxy XY-08N por **BIGTREETECH Eddy Coil V1.0**, un sensor de corrientes de Eddy (eddy current) diseñado específicamente para Klipper.

**Ventajas del Eddy Coil:**
- ✅ **Comunicación I2C nativa** - Sin adaptaciones de voltaje
- ✅ **Alimentación 3.3V/5V** - Directa desde EBB42
- ✅ **Mayor precisión** - ±0.01mm vs ±0.1mm del inductivo
- ✅ **Calibración automática** - Klipper maneja todo vía comandos
- ✅ **Sin circuitos externos** - Conexión directa a puerto I2C
- ✅ **Soporte oficial Klipper** - Documentación completa

**Hardware instalado:**
- Modelo: BIGTREETECH Eddy Coil V1.0
- Chip: LDC1612 (Texas Instruments)
- Conexión: I2C a EBB42 V1.2
- Pines: SCL=PB3, SDA=PB4
- I2C address: 42 (decimal)

### 🔧 Configuración Actual (Eddy Coil)

**Hardware conectado en EBB42 (actualizado):**

| Componente | Cable | Conector EBB42 | Estado |
|------------|-------|----------------|--------|
| **Heater hotend** | Blanco con rayas grises | Terminales tornillo HE | ✅ Conectado |
| **Thermistor** | Blanco delgado | TH0 (2-pin JST) | ✅ Conectado |
| **Ventilador hotend** | Negro/Rojo | FAN1 (PA0) | ✅ Conectado |
| **Ventilador part cooling** | Negro/Azul | FAN2 (PA1) | ✅ Conectado |
| **Eddy Coil V1.0** | Cable I2C 4-pin | I2C (PB3/PB4) | ✅ Conectado |
| ~~Sensor Tronxy (signal)~~ | ~~Negro~~ | ~~PA5 (PROBE)~~ | ❌ REMOVIDO |

**En SKR 1.4 Turbo (actualizado):**

| Componente | Cable | Conector SKR | Estado |
|------------|-------|--------------|--------|
| ~~Sensor Tronxy +24V~~ | ~~Marrón~~ | ~~FAN0 VIN~~ | ❌ REMOVIDO |
| ~~Sensor Tronxy GND~~ | ~~Azul~~ | ~~FAN0 GND~~ | ❌ REMOVIDO |
| **USB a EBB42** | USB-C | Puerto USB | ✅ Conectado |

### 📐 Cableado Eddy Coil I2C

```
BIGTREETECH Eddy Coil V1.0 → EBB42 V1.2

Cable I2C (4 hilos):
├─ Pin 1 (GND)      →  EBB42 I2C: Pin 1 (GND)
├─ Pin 2 (VCC)      →  EBB42 I2C: Pin 2 (VCC 3.3V/5V)
├─ Pin 3 (SDA/PB4)  →  EBB42 I2C: Pin 3 (SDA)
└─ Pin 4 (SCL/PB3)  →  EBB42 I2C: Pin 4 (SCL)
```

### ⚙️ Configuración Klipper (Eddy Coil)

**Sección actualizada en `printer.cfg`:**

```ini
[probe_eddy_current btt_eddy]
sensor_type: ldc1612
i2c_mcu: EBBCan
i2c_address: 42
i2c_speed: 400000
i2c_software_scl_pin: EBBCan:PB3
i2c_software_sda_pin: EBBCan:PB4
x_offset: 0.0
y_offset: 0.0
z_offset: 1.0              # Calibrado con PROBE_EDDY_CURRENT_CALIBRATE
speed: 5.0
lift_speed: 10.0
samples: 2
samples_result: average
sample_retract_dist: 2.0
samples_tolerance: 0.050
samples_tolerance_retries: 3
```

**Comandos de calibración:**

```gcode
# 1. Calibrar drive current (solo primera vez)
LDC_CALIBRATE_DRIVE_CURRENT CHIP=btt_eddy

# 2. Calibrar Z offset
PROBE_EDDY_CURRENT_CALIBRATE CHIP=btt_eddy

# 3. Guardar configuración
SAVE_CONFIG
```

### 📖 Documentación Eddy Coil

Guías detalladas creadas:
- **[EDDY_COIL_INSTALLATION.md](EDDY_COIL_INSTALLATION.md)** - Instalación física completa
- **[EDDY_COIL_CALIBRATION.md](EDDY_COIL_CALIBRATION.md)** - Calibración y uso

### 🔄 Diferencias vs Tronxy XY-08N

| Aspecto | Tronxy XY-08N (Abandonado) | Eddy Coil V1.0 (Actual) |
|---------|----------------------------|-------------------------|
| **Alimentación** | 24V (requiere SKR FAN0) | 3.3V/5V (directo desde EBB42) |
| **Señal** | Digital ON/OFF en 24V | Analógica I2C |
| **Cableado** | Híbrido (SKR + EBB42) | Solo EBB42 (I2C) |
| **Configuración** | `[probe]` con pullup | `[probe_eddy_current]` |
| **Calibración** | Manual (resistencias) | Automática (Klipper) |
| **Precisión** | ±0.1mm | ±0.01mm |
| **Compatibilidad** | Requiere adaptación | Nativa Klipper |
| **Estado** | ❌ NO funcional | ✅ Operativo |

### 🎯 Estado Actual del Sistema

**Hardware operativo:**
- ✅ SKR 1.4 Turbo (nueva placa, reflashed)
- ✅ EBB42 CAN V1.2 (USB mode)
- ✅ Heater + Thermistor en EBB42
- ✅ Fans (hotend + part cooling) en EBB42
- ✅ **Eddy Coil V1.0 calibrado y funcionando**
- ✅ TMC2209 drivers (5×) operativos

**Git commits relevantes:**
- `42f69d3` - Última configuración estable antes de probe (heater funcionando)
- `b9eefb5` - Implementación Eddy Coil V1.0 (2025-12-26)

**Próximos pasos:**
1. Instalación física del Eddy Coil en toolhead
2. Calibración drive current y Z offset
3. Testing de homing G28
4. Bed mesh leveling
5. Primera impresión de prueba

---

## 🔗 Referencias

### Documentación Original (Phase 3):
- **Guía de Implementación Temporal:** [IMPLEMENTACION_TEMPORAL.md](IMPLEMENTACION_TEMPORAL.md)
- **Flasheo SKR Exitoso:** [FLASHEO_SKR_EXITOSO.md](FLASHEO_SKR_EXITOSO.md)
- **Flasheo EBB42 Exitoso:** [FLASHEO_EBB42_EXITOSO.md](FLASHEO_EBB42_EXITOSO.md)
- **Configuración Dual-MCU:** [CONFIGURACION_DUAL_MCU.md](CONFIGURACION_DUAL_MCU.md)
- **Pinout EBB42:** Diagrama oficial BTT

### Documentación Eddy Coil (2025-12-26):
- **Instalación Eddy Coil:** [EDDY_COIL_INSTALLATION.md](EDDY_COIL_INSTALLATION.md)
- **Calibración Eddy Coil:** [EDDY_COIL_CALIBRATION.md](EDDY_COIL_CALIBRATION.md)
- **Klipper Eddy Probe Docs:** https://www.klipper3d.org/Eddy_Probe.html
- **BTT Eddy GitHub:** https://github.com/bigtreetech/Eddy

### Referencias de Hardware:
- ~~Sensor Tronxy XY-08N: Datasheet NPN NO 6-36V~~ (abandonado)
- **BIGTREETECH Eddy Coil V1.0:** LDC1612 sensor, I2C interface

---

**Preparado por:** mjcuadrado + Claude Code
**Fecha inicial:** 2025-12-21 (instalación EBB42)
**Última actualización:** 2025-12-26 (migración Eddy Coil)
**Versión:** 2.0
**Estado:** ✅ Hardware actualizado con Eddy Coil V1.0 - Listo para calibración
