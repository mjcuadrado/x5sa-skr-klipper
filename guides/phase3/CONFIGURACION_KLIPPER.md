# Configuración Klipper - Phase 3

**Fecha:** 2025-12-26
**Versión:** 1.0
**Hardware:** SKR 1.4 Turbo + EBB42 CAN V1.2 (USB Mode)
**Firmware:** Klipper v0.12.0+

---

## Índice

1. [Resumen de Cambios](#1-resumen-de-cambios)
2. [Configuración del Probe - Eddy Coil Rapid Scan](#2-configuración-del-probe---eddy-coil-rapid-scan)
3. [Configuración Bed Mesh - Optimizado para Rapid Scan](#3-configuración-bed-mesh---optimizado-para-rapid-scan)
4. [Macros de Producción](#4-macros-de-producción)
5. [Macros de Calibración](#5-macros-de-calibración)
6. [Firmware Retraction](#6-firmware-retraction)
7. [Referencias Cruzadas](#7-referencias-cruzadas)

---

## 1. Resumen de Cambios

### Decisiones Críticas de Configuración

Esta documentación cubre los cambios realizados en `printer.cfg` durante Phase 3, enfocándose en **optimización para producción** con el probe Eddy Coil.

**Archivo de configuración:** `/Users/mjcuadrado/projects/x5sa-skr-klipper/klipper/config/printer.cfg`

### Cambios Principales

| Componente | Cambio | Razón | Línea |
|------------|--------|-------|-------|
| **Probe Speed** | `speed: 120` (era 5.0) | Rapid scan requiere alta velocidad | 318 |
| **Lift Speed** | `lift_speed: 60` (era 10.0) | Movimientos rápidos entre puntos | 319 |
| **Horizontal Move Z** | `horizontal_move_z: 2` (era 5) | Altura mínima para rapid_scan | 347 |
| **Probe Count** | `probe_count: 5, 5` (25 puntos) | Balance velocidad/calidad | 354 |
| **Scan Overshoot** | `scan_overshoot: 8` | Requerido para rapid_scan | 356 |
| **Adaptive Margin** | `adaptive_margin: 5` | Margen para mesh adaptativo | 364 |

### ¿Por qué NO 50×50?

**Decisión documentada:** Se usa mesh de **5×5 puntos**, NO 50×50.

**Razones:**

1. **Tiempo de probing:**
   - 5×5 (25 puntos) con rapid_scan: ~15 segundos
   - 50×50 (2500 puntos) con rapid_scan: ~15-20 minutos
   - 50×50 con probing normal: >1 hora

2. **Bicubic interpolation:**
   - Klipper interpola entre puntos con algoritmo bicubic
   - 5×5 es suficiente para camas bien tramadas
   - Más puntos NO mejora calidad si el bed físico está plano

3. **Adaptive meshing:**
   - El mesh adaptativo solo sondea el área de impresión + margen
   - Para piezas pequeñas, puede reducirse a 3×3 automáticamente
   - Innecesario sondar toda la cama para cada impresión

4. **Referencia técnica:**
   - Documentación Klipper: 5×5 a 9×9 es óptimo
   - Ellis' Guide: 5×5 o 7×7 recomendado
   - 50×50 es excesivo y contraproducente

**Ver también:** [HARDWARE_EVOLUTION.md](../../HARDWARE_EVOLUTION.md) - Decisión de usar Eddy Coil

---

## 2. Configuración del Probe - Eddy Coil Rapid Scan

### Sección en printer.cfg (líneas 298-329)

```ini
[probe_eddy_current btt_eddy]
# BIGTREETECH Eddy Coil V1.0 connected via I2C to EBB42
sensor_type: ldc1612

# I2C configuration for EBB42 V1.2
i2c_mcu: EBBCan
i2c_address: 42
i2c_speed: 400000
i2c_software_scl_pin: EBBCan:PB3
i2c_software_sda_pin: EBBCan:PB4

# Physical offsets
x_offset: 0.0
y_offset: 0.0
z_offset: 1.0              # Actualizado tras CALIBRATE_EDDY_CURRENT

# Probing parameters - OPTIMIZED FOR RAPID SCAN
speed: 120                 # Fast probing speed for rapid_scan mode
lift_speed: 60.0           # Fast travel speed when not probing
samples: 2
samples_result: average
sample_retract_dist: 2.0
samples_tolerance: 0.050
samples_tolerance_retries: 3
```

### Parámetros Clave

#### `speed: 120`
- **Qué hace:** Velocidad de movimiento durante el scan (mm/s)
- **Por qué 120:** El modo rapid_scan del Eddy Coil requiere alta velocidad para muestreo continuo
- **Valor anterior:** 5.0 mm/s (modo probe tradicional)
- **NO cambiar** a menos que experimentes problemas de precisión

#### `lift_speed: 60.0`
- **Qué hace:** Velocidad de movimiento vertical entre puntos de probe
- **Por qué 60:** Reduce tiempo total de mesh sin comprometer seguridad
- **Valor anterior:** 10.0 mm/s
- **Ajustar si:** Escuchas ruidos o vibraciones durante lift

#### `samples: 2`
- **Qué hace:** Número de mediciones por punto de probe
- **Por qué 2:** Balance entre precisión y velocidad
- **Aumentar a 3** si: `PROBE_ACCURACY` muestra range > 0.05mm

#### `samples_tolerance: 0.050`
- **Qué hace:** Desviación máxima permitida entre samples (mm)
- **Por qué 0.050:** Eddy Coil tiene precisión de ±0.01mm, 0.050 da margen
- **Reducir a 0.030** para mayor exigencia (aumenta retries)

### Calibración Requerida

Antes de usar el probe, ejecutar:

```gcode
CALIBRATE_EDDY_CURRENT
```

Este macro ejecuta:
1. `LDC_CALIBRATE_DRIVE_CURRENT CHIP=btt_eddy` - Calibra corriente del sensor (solo 1 vez)
2. `G28` - Home all axes
3. `PROBE_EDDY_CURRENT_CALIBRATE CHIP=btt_eddy` - Calibra Z offset (interactivo)

**Ver guía completa:** [EDDY_COIL_CALIBRATION.md](EDDY_COIL_CALIBRATION.md)

---

## 3. Configuración Bed Mesh - Optimizado para Rapid Scan

### Sección en printer.cfg (líneas 345-365)

```ini
[bed_mesh]
speed: 120
horizontal_move_z: 2       # CRITICAL: Lower height for rapid_scan

# Probe area
mesh_min: 30, 30
mesh_max: 300, 300         # 330 - 30 margin

# Probe grid - OPTIMIZED FOR RAPID SCAN
probe_count: 5, 5          # 5×5 grid = 25 points (~15 seconds)
algorithm: bicubic
scan_overshoot: 8          # Required for rapid_scan mode

# Fade settings
fade_start: 1.0
fade_end: 10.0
fade_target: 0

# Adaptive bed mesh support
adaptive_margin: 5         # Margin around print area
```

### Parámetros Clave

#### `horizontal_move_z: 2`
- **Qué hace:** Altura Z durante movimientos horizontales entre puntos
- **CRÍTICO para rapid_scan:** Debe ser bajo (2-3mm) para mantener el sensor en rango
- **Valor anterior:** 5mm (tradicional)
- **NO aumentar** o rapid_scan fallará con "out of range"

#### `probe_count: 5, 5`
- **Qué hace:** Grid de sondeo (X puntos × Y puntos)
- **Por qué 5×5:** 25 puntos en ~15 segundos con rapid_scan
- **Alternativas:**
  - `7, 7` (49 puntos, ~25 segundos) - Mayor detalle
  - `3, 3` (9 puntos, ~8 segundos) - Más rápido, menor precisión
- **NUNCA usar 50×50** (ver sección 1)

#### `scan_overshoot: 8`
- **Qué hace:** Distancia adicional de "overshoot" al final de cada línea de scan
- **REQUERIDO para rapid_scan:** El sensor necesita espacio para frenar/acelerar
- **Por qué 8mm:** Valor recomendado por BTT para Eddy Coil
- **NO omitir** o el scan será impreciso

#### `adaptive_margin: 5`
- **Qué hace:** Margen alrededor del área de impresión para mesh adaptativo
- **Por qué 5mm:** Asegura que el mesh cubra completamente la primera capa
- **Cómo funciona:** Si imprimes pieza de 50×50mm en el centro, solo sondea 60×60mm (50+5+5)

### Fade Settings

```ini
fade_start: 1.0    # Comenzar fade a 1mm de altura
fade_end: 10.0     # Terminar fade a 10mm
fade_target: 0     # Converger a Z offset base
```

**Qué hace el fade:**
- Aplica corrección de mesh al 100% en capa 1
- Gradualmente reduce corrección entre 1-10mm
- A partir de 10mm, NO aplica corrección (asume que irregularidades del bed no afectan)

**Por qué es importante:**
- Evita "ondulaciones" en impresiones altas
- Mejora precisión dimensional en eje Z
- Reduce carga computacional durante impresión

### Uso del Mesh Adaptativo

El mesh adaptativo está habilitado en el macro `PRINT_START`:

```gcode
BED_MESH_CALIBRATE METHOD=rapid_scan ADAPTIVE=1
```

**Ventajas:**
- Solo sondea área de impresión + 5mm margen
- Impresión pequeña (50×50mm): ~8 segundos
- Impresión grande (300×300mm): ~15 segundos
- Reduce desgaste del probe

**Desactivar mesh adaptativo:**

Si prefieres SIEMPRE sondar toda la cama:

```gcode
BED_MESH_CALIBRATE METHOD=rapid_scan
# Omitir parámetro ADAPTIVE=1
```

---

## 4. Macros de Producción

### 4.1 PRINT_START (líneas 443-485)

**Macro principal** llamado por OrcaSlicer al inicio de cada impresión.

```gcode
[gcode_macro PRINT_START]
variable_bed_temp: 60
variable_extruder_temp: 200
gcode:
    # Get temps from slicer
    {% set BED_TEMP = params.BED_TEMP|default(60)|float %}
    {% set EXTRUDER_TEMP = params.EXTRUDER_TEMP|default(200)|float %}

    # Set temperatures (don't wait yet)
    M140 S{BED_TEMP}             # Set bed temp
    M104 S150                    # Set extruder to 150C (prevent oozing)

    # Home all axes
    G28

    # Wait for bed to reach temperature
    M190 S{BED_TEMP}

    # Level dual Z steppers
    Z_TILT_ADJUST

    # Generate adaptive bed mesh using rapid_scan
    BED_MESH_CALIBRATE METHOD=rapid_scan ADAPTIVE=1

    # Move to purge position
    G1 X5 Y5 Z10 F6000

    # Heat extruder to printing temperature
    M109 S{EXTRUDER_TEMP}

    # Purge line
    G1 Z0.3 F3000
    G92 E0
    G1 X150 E15 F1500            # Draw purge line
    G1 Y5.4
    G1 X5 E30 F1500              # Draw second purge line
    G92 E0
    G1 Z2.0 F3000                # Lift nozzle

    M117 Printing...
```

#### Flujo del Macro

```
1. Recibir temperaturas desde slicer (BED_TEMP, EXTRUDER_TEMP)
   ↓
2. Calentar bed (sin esperar), precalentar extrusor a 150°C (evita oozing)
   ↓
3. Home all axes (G28)
   ↓
4. Esperar a que bed alcance temperatura target (M190)
   ↓
5. Nivelar dual Z steppers (Z_TILT_ADJUST)
   ↓
6. Generar mesh adaptativo con rapid_scan (~15 segundos)
   ↓
7. Mover a posición de purga (X5 Y5 Z10)
   ↓
8. Calentar extrusor a temperatura de impresión (M109)
   ↓
9. Dibujar línea de purga (2 líneas paralelas, 30mm filamento total)
   ↓
10. Levantar nozzle 2mm, iniciar impresión
```

#### Integración con OrcaSlicer

El slicer llama al macro con:

```gcode
PRINT_START BED_TEMP=[bed_temperature_initial_layer_single] EXTRUDER_TEMP=[nozzle_temperature_initial_layer]
```

**Variables automáticas:**
- `[bed_temperature_initial_layer_single]` → temperatura bed del perfil de filamento
- `[nozzle_temperature_initial_layer]` → temperatura nozzle del perfil de filamento

**Ver:** [ORCA_SLICER_SETUP.md](ORCA_SLICER_SETUP.md) para configuración completa

#### Personalización

**Cambiar posición de purga:**

```gcode
G1 X5 Y5 Z10 F6000    # Frente izquierda (default)
# O
G1 X160 Y5 Z10 F6000  # Frente centro
```

**Cambiar cantidad de purga:**

```gcode
G1 X150 E15 F1500     # 15mm filamento, línea de 145mm
G1 X5 E30 F1500       # Total 30mm extruido
```

**Desactivar mesh adaptativo:**

```gcode
BED_MESH_CALIBRATE METHOD=rapid_scan ADAPTIVE=1
# Cambiar a:
BED_MESH_CALIBRATE METHOD=rapid_scan  # Siempre mesh completo
```

---

### 4.2 PRINT_END (líneas 487-523)

**Macro llamado** al finalizar impresión.

```gcode
[gcode_macro PRINT_END]
gcode:
    # Get print stats
    {% set max_x = printer.toolhead.axis_maximum.x|float %}
    {% set max_y = printer.toolhead.axis_maximum.y|float %}
    {% set max_z = printer.toolhead.axis_maximum.z|float %}
    {% set cur_z = printer.toolhead.position.z|float %}

    # Safe Z retract
    {% if cur_z < (max_z - 10) %}
        G91
        G1 Z10 F600              # Lift Z 10mm
    {% endif %}

    G90
    G1 X5 Y{max_y - 5} F6000     # Park at rear

    # Turn off heaters
    M104 S0
    M140 S0

    # Turn off part cooling fan
    M107

    # Retract filament to prevent oozing
    G91
    G1 E-5 F300                  # Retract 5mm
    G90

    # Disable steppers
    M84

    # Clear bed mesh
    BED_MESH_CLEAR

    M117 Print complete!
```

#### Flujo del Macro

```
1. Detectar posición actual Z y límites de ejes
   ↓
2. Levantar Z 10mm (si hay espacio disponible)
   ↓
3. Aparcar toolhead en parte trasera (Y máximo - 5mm)
   ↓
4. Apagar heaters (extrusor y bed)
   ↓
5. Apagar ventilador de capa (M107)
   ↓
6. Retraer filamento 5mm (prevenir oozing)
   ↓
7. Desactivar steppers (M84)
   ↓
8. Limpiar mesh de la memoria (BED_MESH_CLEAR)
```

#### Safety Feature: Z Retract Condicional

```gcode
{% if cur_z < (max_z - 10) %}
    G91
    G1 Z10 F600
{% endif %}
```

**Por qué es importante:**
- Si la impresión termina cerca del tope Z (ej. Z=395mm en bed de 400mm)
- Intentar subir 10mm causaría error o colisión
- El check evita movimientos fuera de límites

**Personalizar posición de parking:**

```gcode
G1 X5 Y{max_y - 5} F6000     # Trasera izquierda (default)
# O
G1 X{max_x - 5} Y{max_y - 5} F6000  # Trasera derecha
# O
G1 X{max_x / 2} Y{max_y - 5} F6000  # Trasera centro
```

---

### 4.3 PAUSE (líneas 525-545)

Pausa impresión con retracción y parking seguro.

```gcode
[gcode_macro PAUSE]
rename_existing: PAUSE_BASE
gcode:
    SAVE_GCODE_STATE NAME=PAUSE_state
    PAUSE_BASE

    # Retract and lift Z
    G91
    G1 E-2 F2100                 # Retract 2mm
    G1 Z10 F600                  # Lift Z 10mm

    # Move to park position
    G90
    G1 X5 Y5 F6000               # Park at front left

    # Turn off hotend
    M104 S0

    M117 Print paused
```

#### Uso

**Desde OrcaSlicer:**
- Agregar `M600` en altura específica (cambio de filamento)
- OrcaSlicer enviará comando PAUSE

**Desde Mainsail/Fluidd:**
- Botón "Pause" en interfaz
- Comando manual: `PAUSE`

**Comportamiento:**
1. Guarda estado actual (posición, extrusión, etc.)
2. Retrae 2mm filamento (prevenir oozing)
3. Sube Z 10mm (evitar colisión con pieza)
4. Mueve toolhead a frente izquierdo (acceso fácil)
5. Apaga hotend (seguridad)

---

### 4.4 RESUME (líneas 547-563)

Reanuda impresión tras pausa.

```gcode
[gcode_macro RESUME]
rename_existing: RESUME_BASE
gcode:
    # Heat hotend back up
    M109 S{printer.extruder.target}

    # Prime nozzle
    G91
    G1 E2 F2100                  # Unretract 2mm
    G90

    # Restore print state
    RESTORE_GCODE_STATE NAME=PAUSE_state
    RESUME_BASE

    M117 Resuming print...
```

#### Comportamiento

1. Calienta hotend a temperatura previa (lee de memoria)
2. Espera temperatura con M109 (crítico para calidad)
3. Des-retrae 2mm filamento (compensa retracción de PAUSE)
4. Restaura estado guardado (posición, velocidad, etc.)
5. Continúa impresión desde donde pausó

**IMPORTANTE:** NO mover toolhead manualmente durante pausa o la restauración fallará.

---

### 4.5 CANCEL_PRINT (líneas 565-600)

Cancela impresión en curso.

```gcode
[gcode_macro CANCEL_PRINT]
rename_existing: CANCEL_PRINT_BASE
gcode:
    SAVE_GCODE_STATE NAME=CANCEL_state

    # Safe Z retract
    {% set max_z = printer.toolhead.axis_maximum.z|float %}
    {% set cur_z = printer.toolhead.position.z|float %}
    {% if cur_z < (max_z - 10) %}
        G91
        G1 Z10 F600
    {% endif %}

    # Park toolhead
    G90
    G1 X5 Y{printer.toolhead.axis_maximum.y - 5} F6000

    # Turn off heaters
    M104 S0
    M140 S0

    # Turn off fan
    M107

    # Disable steppers
    M84

    # Clear bed mesh
    BED_MESH_CLEAR

    CANCEL_PRINT_BASE

    M117 Print cancelled
```

Similar a PRINT_END pero más agresivo (no retrae filamento, apaga todo inmediatamente).

---

## 5. Macros de Calibración

### 5.1 CALIBRATE_EDDY_CURRENT (líneas 606-614)

Macro todo-en-uno para calibración del Eddy Coil.

```gcode
[gcode_macro CALIBRATE_EDDY_CURRENT]
gcode:
    M118 Step 1: Calibrating drive current...
    LDC_CALIBRATE_DRIVE_CURRENT CHIP=btt_eddy
    M118 Step 2: Calibrating Z offset...
    G28
    PROBE_EDDY_CURRENT_CALIBRATE CHIP=btt_eddy
```

**Uso:**
```gcode
CALIBRATE_EDDY_CURRENT
```

**Ver guía completa:** [EDDY_COIL_CALIBRATION.md](EDDY_COIL_CALIBRATION.md)

---

### 5.2 GENERATE_BED_MESH (líneas 616-622)

Genera mesh completo del bed con rapid_scan.

```gcode
[gcode_macro GENERATE_BED_MESH]
gcode:
    G28
    Z_TILT_ADJUST
    BED_MESH_CALIBRATE METHOD=rapid_scan
    SAVE_CONFIG
```

**Cuándo ejecutar:**
- Después de calibrar Z offset
- Después de cambiar bed surface
- Mensualmente (mantenimiento)

**Tiempo:** ~15 segundos (5×5 grid con rapid_scan)

---

### 5.3 PID_TUNE_HOTEND (líneas 624-631)

PID tuning del hotend con parte cooling al 25%.

```gcode
[gcode_macro PID_TUNE_HOTEND]
gcode:
    {% set TARGET = params.TARGET|default(200)|float %}
    M106 S64                     # Part cooling at 25%
    PID_CALIBRATE HEATER=extruder TARGET={TARGET}
    M107
    SAVE_CONFIG
```

**Uso:**
```gcode
PID_TUNE_HOTEND TARGET=200    # Para PLA
PID_TUNE_HOTEND TARGET=240    # Para PETG
PID_TUNE_HOTEND TARGET=250    # Para ABS
```

**Por qué part cooling al 25%:**
- Simula condiciones reales de impresión
- PID más estable con flujo de aire

---

### 5.4 PID_TUNE_BED (líneas 633-638)

PID tuning del heated bed.

```gcode
[gcode_macro PID_TUNE_BED]
gcode:
    {% set TARGET = params.TARGET|default(60)|float %}
    PID_CALIBRATE HEATER=heater_bed TARGET={TARGET}
    SAVE_CONFIG
```

**Uso:**
```gcode
PID_TUNE_BED TARGET=60     # Para PLA
PID_TUNE_BED TARGET=80     # Para PETG
PID_TUNE_BED TARGET=100    # Para ABS
```

---

### 5.5 CALIBRATE_EXTRUDER (líneas 640-650)

Helper para calibración de E-steps (rotation_distance).

```gcode
[gcode_macro CALIBRATE_EXTRUDER]
gcode:
    M118 E-Steps Calibration:
    M118 1. Mark filament 120mm from extruder
    M118 2. Heat hotend: M109 S200
    M118 3. Extrude 100mm: G91 then G1 E100 F60
    M118 4. Measure remaining distance from mark
    M118 5. Calculate: new_rotation_distance = current_rotation_distance * (100 / actual_extruded)
    M118 6. Update [extruder] rotation_distance in printer.cfg
    M118 Current rotation_distance: {printer.configfile.settings.extruder.rotation_distance}
```

**Procedimiento completo:**

1. Marcar filamento a 120mm del extrusor
2. Calentar hotend: `M109 S200`
3. Cambiar a modo relativo: `G91`
4. Extruir 100mm: `G1 E100 F60`
5. Medir distancia restante desde marca
6. Calcular nuevo valor:
   ```
   nuevo_rotation_distance = actual_rotation_distance × (100 / mm_realmente_extruidos)
   ```
7. Actualizar en `[extruder]` línea 174

**Ver:** [CALIBRACION_COMPLETA.md](CALIBRACION_COMPLETA.md) sección E-steps

---

### 5.6 PRESSURE_ADVANCE_CALIBRATION (líneas 652-666)

Helper para pressure advance tuning.

```gcode
[gcode_macro PRESSURE_ADVANCE_CALIBRATION]
gcode:
    {% set PA_START = params.PA_START|default(0.0)|float %}
    {% set PA_END = params.PA_END|default(0.1)|float %}
    {% set PA_STEP = params.PA_STEP|default(0.005)|float %}

    M118 Pressure Advance Calibration:
    M118 Range: {PA_START} to {PA_END}, step {PA_STEP}
    M118 Use Ellis' PA calibration pattern generator

    SET_PRESSURE_ADVANCE ADVANCE={PA_START}
    M118 Starting PA: {PA_START}
```

**Uso:**
```gcode
PRESSURE_ADVANCE_CALIBRATION PA_START=0.0 PA_END=0.1 PA_STEP=0.005
```

**Método recomendado:** Usar generador de Ellis
- https://ellis3dp.com/Print-Tuning-Guide/articles/pressure_advance.html

**Ver:** [CALIBRACION_COMPLETA.md](CALIBRACION_COMPLETA.md) sección Pressure Advance

---

### 5.7 RETRACTION_CALIBRATION (líneas 668-681)

Helper para retraction tuning.

```gcode
[gcode_macro RETRACTION_CALIBRATION]
gcode:
    M118 Retraction Calibration:
    M118 Current firmware retraction settings:
    M118 - Distance: {printer.firmware_retraction.retract_length}mm
    M118 - Speed: {printer.firmware_retraction.retract_speed}mm/s
    M118 - Unretract extra: {printer.firmware_retraction.unretract_extra_length}mm
    M118 - Unretract speed: {printer.firmware_retraction.unretract_speed}mm/s
    M118
    M118 Test with retraction tower STL or Orca Slicer calibration
    M118 Typical ranges:
    M118 - Direct drive: 0.5-2mm @ 30-50mm/s
    M118 - Bowden: 4-8mm @ 40-60mm/s
```

Muestra valores actuales de firmware retraction para referencia durante calibración.

---

### 5.8 TEST_SPEED (líneas 683-720)

Test de velocidad máxima y aceleración (Ellis guide).

```gcode
[gcode_macro TEST_SPEED]
gcode:
    {% set speed = params.SPEED|default(300)|int %}
    {% set iterations = params.ITERATIONS|default(5)|int %}
    {% set accel = params.ACCEL|default(3000)|int %}

    # Save current settings
    {% set max_accel = printer.configfile.settings.printer.max_accel %}

    # Set test parameters
    SET_VELOCITY_LIMIT VELOCITY={speed} ACCEL={accel}

    # Home if needed
    {% if printer.toolhead.homed_axes != "xyz" %}
        G28
    {% endif %}

    # Run test pattern (diagonal movements)
    M118 Testing speed {speed}mm/s with {accel}mm/s² accel...
    {% for i in range(iterations) %}
        # ... diagonal movements ...
    {% endfor %}

    # Restore settings
    SET_VELOCITY_LIMIT VELOCITY={max_velocity} ACCEL={max_accel}
```

**Uso:**
```gcode
TEST_SPEED SPEED=300 ACCEL=3000 ITERATIONS=5
TEST_SPEED SPEED=400 ACCEL=5000 ITERATIONS=3
```

Útil para encontrar límites mecánicos del sistema antes de ajustar `max_velocity` y `max_accel`.

---

### 5.9 VERIFY_EDDY_PROBE (líneas 732-747)

Safety check del Eddy Coil antes de primer homing.

```gcode
[gcode_macro VERIFY_EDDY_PROBE]
gcode:
    M118 ===== EDDY COIL SAFETY CHECK =====
    M118 1. Testing probe in air (should be OPEN)...
    QUERY_PROBE
    G4 P2000
    M118 2. Place metal object near coil now!
    G4 P3000
    M118 3. Testing probe with metal (should be TRIGGERED)...
    QUERY_PROBE
    M118 4. Remove metal object
    G4 P2000
    M118 5. Final test (should be OPEN again)...
    QUERY_PROBE
    M118 ===== SAFETY CHECK COMPLETE =====
```

**Ejecutar antes del primer G28** para verificar que el probe funciona correctamente.

---

## 6. Firmware Retraction

### Configuración (líneas 777-781)

```ini
[firmware_retraction]
retract_length: 1.0           # Direct drive starting point
retract_speed: 40             # mm/s
unretract_extra_length: 0.0
unretract_speed: 40           # mm/s
```

### ¿Qué es Firmware Retraction?

**Modo tradicional (slicer-controlled):**
- Slicer genera comandos explícitos: `G1 E-1.0 F2400`
- Difícil de ajustar sin re-slicear

**Modo firmware retraction:**
- Slicer envía: `G10` (retract) y `G11` (unretract)
- Klipper controla distancia y velocidad
- Ajustes en tiempo real sin re-slicear

### Integración con OrcaSlicer

Los perfiles de OrcaSlicer están configurados para usar firmware retraction:

```json
"use_firmware_retraction": "1"
```

Cuando el slicer necesita retracción, envía `G10` en lugar de comandos G1 E.

### Ajustar Retraction en Tiempo Real

Durante una impresión, puedes ajustar:

```gcode
SET_RETRACTION RETRACT_LENGTH=1.2 RETRACT_SPEED=40
```

Después de encontrar el valor óptimo, actualizar en `printer.cfg`:

```ini
[firmware_retraction]
retract_length: 1.2    # Valor calibrado
```

### Valores Recomendados por Filamento

**PLA (Direct Drive):**
- `retract_length: 1.0mm`
- `retract_speed: 40mm/s`

**PETG (Direct Drive):**
- `retract_length: 1.2mm`
- `retract_speed: 35mm/s`
- PETG tiende a stringing, necesita más retracción

**ABS (Direct Drive):**
- `retract_length: 1.0mm`
- `retract_speed: 40mm/s`

**Ver:** [CALIBRACION_COMPLETA.md](CALIBRACION_COMPLETA.md) sección Retraction

---

## 7. Referencias Cruzadas

### Documentos Relacionados

| Documento | Tema | Cuándo Consultar |
|-----------|------|------------------|
| [EDDY_COIL_CALIBRATION.md](EDDY_COIL_CALIBRATION.md) | Calibración Eddy Coil | Antes del primer G28 |
| [CALIBRACION_COMPLETA.md](CALIBRACION_COMPLETA.md) | Checklist de calibración | Después de configurar |
| [ORCA_SLICER_SETUP.md](ORCA_SLICER_SETUP.md) | Configuración slicer | Antes de primera impresión |
| [EBB42_INTEGRATION.md](EBB42_INTEGRATION.md) | Hardware EBB42 | Referencia pinout |
| [HARDWARE_EVOLUTION.md](../../HARDWARE_EVOLUTION.md) | Decisiones de hardware | Contexto del proyecto |

### Líneas Importantes en printer.cfg

| Sección | Líneas | Tema |
|---------|--------|------|
| `[mcu]` | 28-31 | Serial ID SKR |
| `[mcu EBBCan]` | 34-37 | Serial ID EBB42 |
| `[printer]` | 43-48 | Límites velocidad/accel |
| `[extruder]` | 168-223 | Configuración extrusor |
| `[probe_eddy_current btt_eddy]` | 298-329 | Configuración probe |
| `[bed_mesh]` | 345-365 | Configuración mesh |
| `[z_tilt]` | 394-408 | Dual Z leveling |
| `[firmware_retraction]` | 777-781 | Retracción firmware |

### Comandos de Referencia Rápida

**Calibración inicial:**
```gcode
CALIBRATE_EDDY_CURRENT    # Calibrar probe
PID_TUNE_HOTEND TARGET=200
PID_TUNE_BED TARGET=60
GENERATE_BED_MESH
```

**Testing:**
```gcode
VERIFY_EDDY_PROBE         # Safety check probe
PROBE_ACCURACY            # Test precisión
TEST_SPEED SPEED=300      # Test velocidad
```

**Producción:**
```gcode
PRINT_START BED_TEMP=60 EXTRUDER_TEMP=200
# ... impresión ...
PRINT_END
```

---

## Próximos Pasos

✅ **Configuración completada**

**Siguiente:**
1. Calibrar sistema completo → [CALIBRACION_COMPLETA.md](CALIBRACION_COMPLETA.md)
2. Configurar OrcaSlicer → [ORCA_SLICER_SETUP.md](ORCA_SLICER_SETUP.md)
3. Primera impresión de prueba

---

**Versión:** 1.0
**Fecha:** 2025-12-26
**Autor:** Documentación Phase 3 - Tronxy X5SA Klipper Migration
**Archivo:** `/Users/mjcuadrado/projects/x5sa-skr-klipper/klipper/config/printer.cfg`
