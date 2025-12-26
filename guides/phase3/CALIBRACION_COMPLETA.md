# Calibración Completa - Phase 3

**Fecha:** 2025-12-26
**Versión:** 1.0
**Hardware:** Tronxy X5SA + SKR 1.4 Turbo + EBB42 CAN V1.2
**Firmware:** Klipper v0.12.0+

---

## Índice

1. [Checklist de Calibración](#1-checklist-de-calibración)
2. [Orden de Ejecución](#2-orden-de-ejecución)
3. [Calibración PID - Hotend](#3-calibración-pid---hotend)
4. [Calibración PID - Heated Bed](#4-calibración-pid---heated-bed)
5. [Calibración E-steps (Rotation Distance)](#5-calibración-e-steps-rotation-distance)
6. [Calibración Z Offset (Eddy Coil)](#6-calibración-z-offset-eddy-coil)
7. [Nivelación Dual Z (Z-Tilt Adjust)](#7-nivelación-dual-z-z-tilt-adjust)
8. [Generación de Bed Mesh](#8-generación-de-bed-mesh)
9. [Calibración Pressure Advance](#9-calibración-pressure-advance)
10. [Calibración Retraction](#10-calibración-retraction)
11. [Calibración Input Shaper (Opcional)](#11-calibración-input-shaper-opcional)
12. [Valores Esperados y Rangos](#12-valores-esperados-y-rangos)
13. [Troubleshooting por Calibración](#13-troubleshooting-por-calibración)

---

## 1. Checklist de Calibración

### Prerequisitos Hardware

Antes de iniciar cualquier calibración:

- [ ] ✅ **SKR 1.4 Turbo** flasheada y conectada vía USB
- [ ] ✅ **EBB42 CAN V1.2** flasheada y conectada vía USB
- [ ] ✅ **Dual-MCU** configurado en printer.cfg (`[mcu]` y `[mcu EBBCan]`)
- [ ] ✅ **Thermistores** leyendo temperatura ambiente (~20-25°C)
- [ ] ✅ **Heater hotend** funcional (puede calentar)
- [ ] ✅ **Heater bed** funcional (puede calentar)
- [ ] ✅ **Fans** operativos (part cooling + hotend fan)
- [ ] ✅ **Eddy Coil** instalado físicamente (2-3mm sobre nozzle)
- [ ] ✅ **Cable I2C** conectado (EBB42 ↔ Eddy Coil)
- [ ] ✅ **Motors X/Y/Z/E** conectados y energizados

### Calibraciones Obligatorias

**DEBE completarse antes de la primera impresión:**

- [ ] 1. **PID Hotend** - Estabilidad temperatura extrusor
- [ ] 2. **PID Bed** - Estabilidad temperatura cama
- [ ] 3. **E-steps** - Cantidad correcta de filamento extruido
- [ ] 4. **Eddy Coil Drive Current** - Sensibilidad del sensor (solo 1 vez)
- [ ] 5. **Z Offset** - Altura nozzle vs bed
- [ ] 6. **Z-Tilt Adjust** - Nivelación dual Z
- [ ] 7. **Bed Mesh** - Mapa de irregularidades del bed

### Calibraciones Recomendadas

**Mejoran significativamente la calidad de impresión:**

- [ ] 8. **Pressure Advance** - Elimina bulging/gaps en esquinas
- [ ] 9. **Retraction** - Reduce stringing
- [ ] 10. **Input Shaper** - Reduce ringing/ghosting (requiere ADXL345)

### Calibraciones Opcionales

**Solo si experimentas problemas específicos:**

- [ ] **Sensorless Homing Tuning** - Ajustar sensibilidad StallGuard
- [ ] **Flow Rate** - Ajuste fino por marca de filamento
- [ ] **Linear Advance** - Similar a PA, menos común
- [ ] **Max Velocity/Accel** - Límites mecánicos del sistema

---

## 2. Orden de Ejecución

**IMPORTANTE:** Seguir este orden específico. Algunas calibraciones dependen de otras.

```
┌─────────────────────────────────────────────────┐
│ FASE 1: Calibraciones Térmicas (30-60 min)     │
├─────────────────────────────────────────────────┤
│ 1. PID Hotend                                   │
│ 2. PID Bed                                      │
└────────────────┬────────────────────────────────┘
                 ↓
┌─────────────────────────────────────────────────┐
│ FASE 2: Calibraciones Mecánicas (45-90 min)    │
├─────────────────────────────────────────────────┤
│ 3. E-steps (Rotation Distance)                  │
│ 4. Eddy Coil Drive Current (solo 1 vez)         │
│ 5. Z Offset                                     │
│ 6. Z-Tilt Adjust                                │
│ 7. Bed Mesh                                     │
└────────────────┬────────────────────────────────┘
                 ↓
┌─────────────────────────────────────────────────┐
│ FASE 3: Calibraciones de Calidad (60-120 min)  │
├─────────────────────────────────────────────────┤
│ 8. Pressure Advance (PLA, PETG, ABS)            │
│ 9. Retraction (si hay stringing)                │
│ 10. Input Shaper (opcional, con ADXL345)        │
└─────────────────────────────────────────────────┘
```

**Tiempo total estimado:** 2.5 - 4.5 horas (primera vez)

**Calibraciones recurrentes:**
- PID: Solo si cambias hotend/bed
- E-steps: Solo si cambias extrusor
- Z Offset: Después de cambiar nozzle/bed
- Bed Mesh: Mensual o tras cambios físicos
- PA/Retraction: Por marca/tipo de filamento

---

## 3. Calibración PID - Hotend

### ¿Qué es PID?

PID (Proportional-Integral-Derivative) controla cómo el hotend mantiene temperatura estable.

**Síntomas de PID mal calibrado:**
- Temperatura oscila ±5-10°C
- Tarda mucho en alcanzar target
- "Thermal runaway" errors

### Procedimiento

#### Paso 1: Preparación

1. **Bed frío** (no calentar bed durante PID hotend)
2. **Nozzle limpio** (sin filamento colgando)
3. **Part cooling al 25%** (simula condiciones de impresión)

#### Paso 2: Ejecutar PID Tuning

**Usando el macro incluido en printer.cfg:**

```gcode
PID_TUNE_HOTEND TARGET=210
```

**O manualmente:**

```gcode
M106 S64                                    # Part cooling 25%
PID_CALIBRATE HEATER=extruder TARGET=210
```

**Parámetros:**
- `TARGET=210` para PLA
- `TARGET=240` para PETG
- `TARGET=250` para ABS

**Duración:** ~10-15 minutos

#### Paso 3: Observar el Proceso

Klipper mostrará en consola:

```
// PID calibration started
// Target: 210.0
// Cycle 1/10: overshoot 2.3°C
// Cycle 2/10: overshoot 1.8°C
// ...
// Cycle 10/10: overshoot 0.5°C
// PID parameters: pid_Kp=22.2 pid_Ki=1.08 pid_Kd=114
// Run SAVE_CONFIG to store results
```

#### Paso 4: Guardar Resultados

```gcode
M107                  # Apagar part cooling
SAVE_CONFIG
```

**Klipper se reiniciará automáticamente.**

#### Paso 5: Verificar Resultados

Después del reinicio, verificar `printer.cfg` (sección auto-generada al final):

```ini
#*# [extruder]
#*# control = pid
#*# pid_kp = 22.200
#*# pid_ki = 1.080
#*# pid_kd = 114.000
```

### Test de Estabilidad

Calentar hotend y verificar estabilidad:

```gcode
M104 S210
# Esperar ~2 minutos
# Observar temperatura en Mainsail/Fluidd
```

**Resultado esperado:**
- Temperatura oscila ±0.5-1°C (excelente)
- ±1-2°C (aceptable)
- >±3°C (recalibrar)

---

## 4. Calibración PID - Heated Bed

### ¿Por qué calibrar el Bed?

El bed tiene **mucha más masa térmica** que el hotend:
- Tarda más en calentar
- Mayor inercia térmica
- Necesita PID específico

### Procedimiento

#### Paso 1: Preparación

- **Hotend frío** (apagado durante PID bed)
- **Bed limpio**
- **Sin objetos en el bed**

#### Paso 2: Ejecutar PID Tuning

**Usando el macro:**

```gcode
PID_TUNE_BED TARGET=60
```

**O manualmente:**

```gcode
PID_CALIBRATE HEATER=heater_bed TARGET=60
```

**Parámetros:**
- `TARGET=60` para PLA
- `TARGET=80` para PETG
- `TARGET=100` para ABS

**Duración:** ~15-25 minutos (bed es más lento que hotend)

#### Paso 3: Guardar Resultados

```gcode
SAVE_CONFIG
```

Klipper reinicia automáticamente.

#### Paso 4: Verificar en printer.cfg

```ini
#*# [heater_bed]
#*# control = pid
#*# pid_kp = 54.027
#*# pid_ki = 0.770
#*# pid_kd = 948.182
```

### Test de Estabilidad

```gcode
M140 S60
# Esperar ~5 minutos hasta estabilización
```

**Resultado esperado:**
- Oscilación ±1-2°C (excelente)
- ±2-3°C (aceptable)
- >±5°C (recalibrar)

---

## 5. Calibración E-steps (Rotation Distance)

### ¿Qué es Rotation Distance?

Define cuánto filamento extruye por vuelta del motor. Crítico para:
- Cantidad correcta de filamento
- Calidad de impresión
- Over/under-extrusion

**Síntomas de E-steps incorrectos:**
- Under-extrusion: gaps entre líneas, top surface con huecos
- Over-extrusion: exceso de plástico, blobs, elephants foot

### Procedimiento

#### Paso 1: Materiales

- [ ] Rotulador permanente
- [ ] Regla o calibre
- [ ] Tijeras (cortar filamento)
- [ ] Filamento cargado en extrusor

#### Paso 2: Marcar Filamento

1. Cargar filamento hasta que salga del hotend
2. Con rotulador, **marcar** filamento a **120mm** de la entrada del extrusor
3. Anotar distancia exacta: `120.0mm`

```
          ┌─────────────┐
          │  Extrusor   │
          │   (Titan)   │
          └──────┬──────┘
                 │
                 │ 120mm
                 │
                 ↓
            ═══════════  ← Marca con rotulador
                 │
            Filamento
```

#### Paso 3: Calentar Hotend

```gcode
M109 S200    # Calentar a 200°C (PLA temp)
```

**Esperar** hasta que alcance temperatura.

#### Paso 4: Extruir 100mm

```gcode
G91              # Modo relativo
G1 E100 F60      # Extruir 100mm a 60mm/min (lento)
G90              # Volver a modo absoluto
```

**Observar:**
- Filamento debe moverse suavemente
- Sin ruido de grinding
- Motor NO debe saltar pasos

#### Paso 5: Medir Distancia Restante

Con regla/calibre, medir distancia desde **entrada del extrusor** hasta la **marca**.

**Ejemplo:**
- Marca original: 120mm
- Marca ahora: 25mm
- **Distancia extruida real:** 120 - 25 = **95mm**

Si el valor es **cercano a 100mm** (ej. 98-102mm), calibración es buena. Si difiere mucho, continuar.

#### Paso 6: Calcular Nuevo Rotation Distance

**Fórmula:**
```
nuevo_rotation_distance = rotation_distance_actual × (100 / distancia_real_extruida)
```

**Ejemplo:**

Valor actual en `printer.cfg`:
```ini
[extruder]
rotation_distance: 22.478   # Titan clone default
```

Distancia real extruida: **95mm**

Cálculo:
```
nuevo = 22.478 × (100 / 95)
nuevo = 22.478 × 1.0526
nuevo = 23.66
```

#### Paso 7: Actualizar printer.cfg

Editar `printer.cfg` línea 174:

```ini
[extruder]
rotation_distance: 23.66    # Valor calibrado (era 22.478)
```

#### Paso 8: Reiniciar y Verificar

```gcode
RESTART
```

**Repetir test** (pasos 2-5) para verificar:
- Marcar a 120mm
- Extruir 100mm
- Medir → Debería ser ~100mm (±1mm aceptable)

Si aún difiere, **repetir cálculo** con nuevo valor.

### Macro de Ayuda

El macro `CALIBRATE_EXTRUDER` muestra instrucciones:

```gcode
CALIBRATE_EXTRUDER
```

Imprime en consola:
- Pasos del procedimiento
- Fórmula de cálculo
- Valor actual de rotation_distance

---

## 6. Calibración Z Offset (Eddy Coil)

### ¿Qué es Z Offset?

Define la altura del nozzle cuando el Eddy Coil detecta el bed. **Crítico** para primera capa correcta.

**Síntomas de Z offset incorrecto:**
- Nozzle muy alto: filamento no adhiere
- Nozzle muy bajo: nozzle raspa bed, filamento aplastado

### Prerequisito: Drive Current Calibrado

**Solo ejecutar UNA VEZ** (instalación inicial):

```gcode
LDC_CALIBRATE_DRIVE_CURRENT CHIP=btt_eddy
SAVE_CONFIG
```

**Ver guía completa:** [EDDY_COIL_CALIBRATION.md](EDDY_COIL_CALIBRATION.md) sección Drive Current

### Procedimiento

#### Método 1: Macro Automático (Recomendado)

```gcode
CALIBRATE_EDDY_CURRENT
```

Este macro ejecuta:
1. Drive current calibration (si no hecho previamente)
2. G28 (home all)
3. Calibración Z offset interactiva

**Seguir instrucciones en pantalla.**

#### Método 2: Manual (Paso a Paso)

**Paso 1: Home all axes**

```gcode
G28
```

**Paso 2: Iniciar calibración**

```gcode
PROBE_EDDY_CURRENT_CALIBRATE CHIP=btt_eddy
```

Klipper responde:
```
// Starting eddy current calibration
// Use TESTZ to adjust nozzle height
```

**Paso 3: Paper Test**

1. Colocar **papel A4** (~0.1mm) entre nozzle y bed
2. Bajar nozzle con:
   ```gcode
   TESTZ Z=-0.1    # Baja 0.1mm
   ```
3. Repetir hasta que papel tenga **ligera resistencia**
   - Demasiado alto: papel se mueve libremente
   - **Perfecto:** papel tiene resistencia pero se mueve sin romper
   - Demasiado bajo: papel atascado o roto

**Ajustes finos:**
```gcode
TESTZ Z=-0.01   # Baja 10 micras
TESTZ Z=+0.01   # Sube 10 micras
```

**Paso 4: Aceptar calibración**

Cuando altura sea correcta:

```gcode
ACCEPT
```

Klipper muestra:
```
// Calibration accepted
// Z offset: 1.234
// Run SAVE_CONFIG to store
```

**Paso 5: Guardar**

```gcode
SAVE_CONFIG
```

Klipper reinicia y guarda `z_offset` en printer.cfg.

### Verificación

Después del reinicio, ejecutar test de primera capa virtual:

```gcode
G28
G1 Z0.2 F300       # Bajar a altura de primera capa
G1 X100 Y100 F3000  # Mover al centro
```

Colocar papel:
- Debe tener **ligera resistencia** (igual que calibración)

**Si difiere:** Recalibrar Z offset.

### Ajuste Fino Durante Impresión (Baby Stepping)

Durante primera impresión, puedes ajustar en tiempo real:

```gcode
SET_GCODE_OFFSET Z_ADJUST=-0.05 MOVE=1   # Bajar nozzle
# O
SET_GCODE_OFFSET Z_ADJUST=+0.05 MOVE=1   # Subir nozzle
```

**Guardar ajuste permanente:**

```gcode
Z_OFFSET_APPLY_PROBE
SAVE_CONFIG
```

---

## 7. Nivelación Dual Z (Z-Tilt Adjust)

### ¿Qué es Z-Tilt?

La Tronxy X5SA tiene **2 motores Z independientes**. Z-Tilt nivela automáticamente ambos lados del pórtico.

**Síntomas de Z desnivelado:**
- Primera capa desigual (buena en un lado, mala en otro)
- Bed mesh con grandes variaciones
- Tolerancias inconsistentes

### Procedimiento

#### Paso 1: Preparación

- [ ] Bed limpio
- [ ] Z offset calibrado
- [ ] Temperatura ambiente (bed frío)

#### Paso 2: Home all

```gcode
G28
```

#### Paso 3: Ejecutar Z-Tilt Adjust

```gcode
Z_TILT_ADJUST
```

Klipper ejecuta:
1. Probe punto izquierdo (X=50, Y=165)
2. Probe punto derecho (X=280, Y=165)
3. Calcula diferencia de altura
4. Ajusta motor Z1 para nivelar
5. Re-verifica con nuevo probe
6. Repite hasta convergencia

**Resultado esperado:**

```
// probe at 50.000,165.000 is z=1.234
// probe at 280.000,165.000 is z=1.240
// z_adjust: stepper_z: 0.000 stepper_z1: -0.006
// Retrying with adjusted z
// probe at 50.000,165.000 is z=1.235
// probe at 280.000,165.000 is z=1.236
// z_tilt_adjust: Success, tolerance 0.001
```

**Análisis:**
- `tolerance 0.001`: Excelente (1 micra)
- `tolerance 0.005-0.020`: Aceptable (5-20 micras)
- `tolerance >0.050`: Revisar montaje mecánico

#### Paso 4: Verificar Convergencia

Si Z_TILT_ADJUST **falla** después de 5 retries:

```
!! Failed to converge, retries exceeded
!! Adjust bed manually and retry
```

**Solución:**
1. Apagar impresora
2. **Manualmente nivelar bed** con tornillos
3. Usar nivel de burbuja o calibre
4. Re-intentar Z_TILT_ADJUST

### Cuándo Ejecutar Z-Tilt

**Obligatorio:**
- Antes de generar bed mesh
- Después de mover la impresora
- Después de ajustes mecánicos (correas, etc.)

**Recomendado:**
- Antes de cada impresión (incluido en macro PRINT_START)

**NO necesario:**
- Entre impresiones consecutivas sin mover impresora

---

## 8. Generación de Bed Mesh

### ¿Qué es Bed Mesh?

Mapa de irregularidades del bed. Compensa:
- Bed no perfectamente plano
- Pequeñas variaciones de altura
- Thermal expansion del bed

### Procedimiento

#### Paso 1: Preparación

- [ ] Z offset calibrado
- [ ] Z-tilt ejecutado (bed nivelado)
- [ ] **Bed caliente** (60°C para PLA) → IMPORTANTE para precisión

**Calentar bed:**
```gcode
M190 S60    # Esperar a que alcance 60°C
```

#### Paso 2: Ejecutar Bed Mesh

**Usando el macro:**

```gcode
GENERATE_BED_MESH
```

Este macro ejecuta:
1. G28 (home all)
2. Z_TILT_ADJUST (nivela dual Z)
3. BED_MESH_CALIBRATE METHOD=rapid_scan (genera mesh)
4. SAVE_CONFIG (guarda mesh)

**O manualmente:**

```gcode
G28
Z_TILT_ADJUST
BED_MESH_CALIBRATE METHOD=rapid_scan
SAVE_CONFIG
```

**Duración:** ~15 segundos (5×5 grid con rapid_scan)

#### Paso 3: Observar el Proceso

Klipper muestra en consola:

```
// Bed mesh calibration started
// Probing point 1/25 at 30.000,30.000
// ...
// Probing point 25/25 at 300.000,300.000
// Mesh calibration complete
// Bed mesh range: min=-0.123 max=0.089 range=0.212
```

**Análisis del range:**
- < 0.2mm: Excelente (bed bien tramado)
- 0.2-0.5mm: Aceptable (bed ligeramente irregular)
- > 0.5mm: Revisar tramming manual del bed

#### Paso 4: Verificar Mesh en printer.cfg

Después del reinicio, verificar sección auto-generada:

```ini
#*# [bed_mesh default]
#*# version = 1
#*# points =
#*#   -0.123, -0.089, -0.045, -0.012, 0.023
#*#   -0.098, -0.067, -0.034, -0.008, 0.034
#*#   -0.078, -0.045, -0.011, 0.015, 0.056
#*#   -0.056, -0.023, 0.011, 0.038, 0.067
#*#   -0.034, -0.001, 0.034, 0.067, 0.089
#*# x_count = 5
#*# y_count = 5
#*# mesh_x_pps = 2
#*# mesh_y_pps = 2
#*# algo = bicubic
#*# tension = 0.2
#*# min_x = 30.0
#*# max_x = 300.0
#*# min_y = 30.0
#*# max_y = 300.0
```

### Visualizar Mesh

**En Mainsail/Fluidd:**
1. Ir a sección "Heightmap"
2. Visualización 3D del mesh
3. Colores indican altura relativa

**Interpretar colores:**
- Azul: Puntos bajos (nozzle más cerca)
- Rojo: Puntos altos (nozzle más lejos)
- Verde: Altura promedio

### Cuándo Regenerar Mesh

**Obligatorio:**
- Después de cambiar bed surface (PEI, glass, etc.)
- Después de ajustar tramming manual del bed
- Después de Z offset recalibration

**Recomendado:**
- Mensualmente (mantenimiento preventivo)
- Si notas primera capa inconsistente

**NO necesario:**
- Entre impresiones (mesh se mantiene en memoria)
- Cada vez que enciendes la impresora (se carga automáticamente)

---

## 9. Calibración Pressure Advance

### ¿Qué es Pressure Advance?

Compensa el retraso de presión en el hotend:
- **Sin PA:** Filamento continúa saliendo en esquinas → bulging
- **Con PA:** Reduce flow antes de esquina, aumenta después → esquinas afiladas

**Síntomas de PA incorrecto:**
- Esquinas redondeadas o con bulging
- Gaps después de esquinas
- Inconsistencia en extrusión

### Prerequisito

- [ ] E-steps calibrados
- [ ] PID hotend calibrado
- [ ] Primera impresión exitosa

### Método 1: Generador de Ellis (Recomendado)

#### Paso 1: Generar Modelo de Test

1. Ir a: https://ellis3dp.com/Print-Tuning-Guide/articles/pressure_advance.html
2. Scroll a "Pattern Generator"
3. Configurar:
   - **Printer:** Klipper
   - **Bed Size:** 330×330
   - **Start PA:** 0.0
   - **End PA:** 0.1
   - **PA Increment:** 0.005
   - **Print Settings:** 0.2mm layer height, 2 perimeters, 0% infill

4. Hacer clic en "Generate G-code"
5. Descargar archivo `pressure_advance_tower.gcode`

#### Paso 2: Imprimir Test

1. Calentar bed/extrusor a temperatura del filamento (PLA: 210°C/60°C)
2. Cargar G-code en Mainsail/Fluidd
3. Imprimir

**Duración:** ~20-30 minutos

#### Paso 3: Analizar Resultados

Observar las esquinas de la torre:

- **PA muy bajo (0.00-0.02):** Esquinas redondeadas, bulging
- **PA óptimo:** Esquinas afiladas, sin bulging ni gaps
- **PA muy alto (>0.08):** Gaps después de esquinas, under-extrusion

**Ejemplo:**

```
PA 0.00  → Bulging en esquinas
PA 0.02  → Aún algo de bulging
PA 0.04  → ✓ Esquinas afiladas (óptimo)
PA 0.06  → Ligeros gaps
PA 0.08  → Gaps evidentes
```

Valor óptimo: **PA = 0.04** para este caso.

#### Paso 4: Configurar en Klipper

**Opción A: En printer.cfg (global)**

Editar `[extruder]` línea 213:

```ini
[extruder]
pressure_advance: 0.04
```

```gcode
RESTART
```

**Opción B: En slicer (por filamento)**

En OrcaSlicer → Filament Settings → Custom G-code → Start G-code:

```gcode
SET_PRESSURE_ADVANCE ADVANCE=0.04
```

**Opción C: Durante impresión (test)**

```gcode
SET_PRESSURE_ADVANCE ADVANCE=0.04
```

### Método 2: Macro de Ayuda

```gcode
PRESSURE_ADVANCE_CALIBRATION PA_START=0.0 PA_END=0.1 PA_STEP=0.005
```

Muestra rango y enlace al generador de Ellis.

### Valores Iniciales por Filamento

**Direct Drive (Titan clone):**
- **PLA:** 0.03 - 0.05
- **PETG:** 0.045 - 0.065
- **ABS:** 0.04 - 0.06

**Bowden (no aplicable en Phase 3, solo referencia):**
- Valores más altos: 0.4 - 1.2

### Verificación

Imprimir pieza real y observar:
- [ ] Esquinas afiladas (no redondeadas)
- [ ] Sin bulging en cambios de dirección
- [ ] Sin gaps después de esquinas
- [ ] Texto/detalles finos precisos

Si no es perfecto, ajustar ±0.005 y re-test.

---

## 10. Calibración Retraction

### ¿Qué es Retraction?

Retracción del filamento durante movimientos sin extrusión (travels):
- Previene oozing
- Reduce stringing
- Mejora calidad en saltos

**Síntomas de retraction incorrecta:**
- **Muy poca:** Stringing (hilos entre partes), oozing, blobs
- **Demasiada:** Under-extrusion después de travel, gaps, clogs

### Configuración Firmware Retraction

La X5SA usa **firmware retraction** (controlado por Klipper):

```ini
[firmware_retraction]
retract_length: 1.0           # Distancia de retracción (mm)
retract_speed: 40             # Velocidad retract (mm/s)
unretract_extra_length: 0.0   # Extra filamento al des-retraer
unretract_speed: 40           # Velocidad unretract (mm/s)
```

**Ventaja:** Ajustes en tiempo real sin re-slicear.

### Procedimiento

#### Paso 1: Valores Iniciales

**Direct Drive (Titan clone):**
- Retract length: **1.0mm** (punto de partida)
- Retract speed: **40mm/s**

**Bowden (referencia, no aplicable):**
- Retract length: 4-8mm
- Retract speed: 40-60mm/s

#### Paso 2: Imprimir Retraction Tower

**Opción A: Retraction Test de OrcaSlicer**

1. En OrcaSlicer: `Help` → `Test Tools` → `Retraction Test`
2. Configurar:
   - Start retraction: 0.5mm
   - End retraction: 2.0mm
   - Step: 0.1mm
3. Slice y imprimir

**Opción B: Modelo externo**

Buscar "Retraction Tower" en Thingiverse/Printables.

#### Paso 3: Analizar Resultados

Observar stringing entre torres:

```
Retraction 0.5mm → Mucho stringing
Retraction 1.0mm → Algo de stringing
Retraction 1.2mm → ✓ Sin stringing (óptimo)
Retraction 1.5mm → Sin stringing pero posible under-extrusion
```

Valor óptimo: **1.2mm** en este caso.

#### Paso 4: Ajustar en Klipper

**Durante impresión (test):**

```gcode
SET_RETRACTION RETRACT_LENGTH=1.2 RETRACT_SPEED=40
```

**Permanente en printer.cfg:**

Editar `[firmware_retraction]` línea 778:

```ini
[firmware_retraction]
retract_length: 1.2    # Valor calibrado
retract_speed: 40
```

```gcode
RESTART
```

### Valores por Filamento

**PLA:**
- Retract: 0.8 - 1.2mm
- Speed: 40-50mm/s

**PETG:**
- Retract: 1.0 - 1.5mm (PETG stringy)
- Speed: 30-40mm/s (más lento)

**ABS:**
- Retract: 0.8 - 1.2mm
- Speed: 40-50mm/s

### Combinación con Pressure Advance

PA y Retraction trabajan juntos:
- **Buen PA** reduce necesidad de retracción alta
- Si PA está calibrado, retraction puede ser menor
- Si ves stringing, calibrar **primero PA**, **luego** retraction

### Troubleshooting

**Stringing persiste después de calibrar:**
1. Verificar PA calibrado
2. Reducir temperatura 5-10°C
3. Secar filamento (PETG/ABS muy sensible a humedad)
4. Aumentar travel speed en slicer

**Under-extrusion después de travels:**
- Retraction demasiado alta → reducir
- Aumentar `unretract_extra_length` a 0.05-0.10mm

---

## 11. Calibración Input Shaper (Opcional)

### ¿Qué es Input Shaper?

Reduce vibraciones (ringing/ghosting) en impresiones a alta velocidad.

**Requisito:** ADXL345 accelerometer (integrado en EBB42)

### Prerequisito

- [ ] ADXL345 conectado a EBB42 (configurado en printer.cfg línea 370)
- [ ] Impresiones previas exitosas

### Procedimiento

#### Paso 1: Verificar ADXL345

```gcode
ACCELEROMETER_QUERY
```

**Resultado esperado:**
```
// adxl345 values (x, y, z): 123.4, -234.5, 9876.5
```

Si falla, revisar conexión SPI en printer.cfg.

#### Paso 2: Medir Resonancias X

```gcode
TEST_RESONANCES AXIS=X
```

**Duración:** ~2-3 minutos (vibraciones intensas, normal)

Klipper genera archivo CSV en: `~/printer_data/config/resonances_x_*.csv`

#### Paso 3: Medir Resonancias Y

```gcode
TEST_RESONANCES AXIS=Y
```

Genera: `~/printer_data/config/resonances_y_*.csv`

#### Paso 4: Generar Gráficas (en host)

**En Raspberry Pi/host via SSH:**

```bash
cd ~/klipper/scripts/calibrate_shaper.py
~/klippy-env/bin/python ~/klipper/scripts/calibrate_shaper.py \
  ~/printer_data/config/resonances_x_*.csv \
  -o /tmp/shaper_calibrate_x.png

~/klippy-env/bin/python ~/klipper/scripts/calibrate_shaper.py \
  ~/printer_data/config/resonances_y_*.csv \
  -o /tmp/shaper_calibrate_y.png
```

**Descargar imágenes** para análisis:
- `/tmp/shaper_calibrate_x.png`
- `/tmp/shaper_calibrate_y.png`

#### Paso 5: Analizar Gráficas

**Gráfica muestra:**
- Frecuencia de resonancia (Hz)
- Tipo de shaper recomendado (MZV, EI, 2HUMP_EI, etc.)

**Ejemplo:**
```
X axis: 54.2 Hz, recommended shaper: MZV
Y axis: 42.8 Hz, recommended shaper: EI
```

#### Paso 6: Configurar en printer.cfg

Editar `[input_shaper]` línea 384:

```ini
[input_shaper]
shaper_freq_x: 54.2
shaper_type_x: mzv
shaper_freq_y: 42.8
shaper_type_y: ei
```

```gcode
RESTART
```

### Verificación

Imprimir test de ringing (ej. "Ringing Tower"):
- [ ] Reducción visible de ghosting
- [ ] Esquinas más afiladas
- [ ] Menos vibraciones audibles

### Cuándo Recalibrar

- Después de cambios mecánicos (belts, toolhead weight)
- Si instalas nuevo hotend/extrusor
- Si notas ringing después de upgrade

**NO necesario:**
- Por cambio de filamento
- Por ajustes de temperatura/velocidad

---

## 12. Valores Esperados y Rangos

### Tabla de Referencias

| Calibración | Valor Esperado | Rango Aceptable | Fuera de Rango |
|-------------|----------------|-----------------|----------------|
| **PID Hotend Kp** | 20-25 | 15-30 | <10 o >40 |
| **PID Hotend Ki** | 1.0-1.5 | 0.5-2.0 | <0.3 o >3.0 |
| **PID Hotend Kd** | 100-150 | 80-200 | <50 o >300 |
| **PID Bed Kp** | 50-60 | 40-80 | <30 o >100 |
| **PID Bed Ki** | 0.5-1.0 | 0.3-1.5 | <0.1 o >2.0 |
| **PID Bed Kd** | 800-1000 | 600-1200 | <400 o >1500 |
| **Rotation Distance** | 22-24mm | 20-26mm | <18 o >28mm |
| **Z Offset (Eddy)** | 0.5-3.0mm | 0.2-5.0mm | <0 o >6mm |
| **Z-Tilt Tolerance** | 0.001-0.010mm | <0.020mm | >0.050mm |
| **Bed Mesh Range** | 0.1-0.3mm | <0.5mm | >0.8mm |
| **Pressure Advance (DD)** | 0.03-0.06 | 0.01-0.10 | <0 o >0.15 |
| **Retraction (DD)** | 0.8-1.5mm | 0.5-2.0mm | <0.3 o >3.0mm |
| **PROBE_ACCURACY Range** | <0.01mm | <0.05mm | >0.1mm |

### Síntomas de Valores Fuera de Rango

**PID fuera de rango:**
- Temperatura muy oscilante
- No alcanza target
- "Thermal runaway" errors

**Rotation distance fuera de rango:**
- Over/under-extrusion severo
- Dimensiones muy incorrectas

**Z offset fuera de rango:**
- Probable error en procedimiento
- Revisar instalación física del probe

**Z-Tilt tolerance alto:**
- Revisar montaje mecánico
- Verificar nivelación manual del bed

**Bed mesh range alto:**
- Bed muy irregular
- Tramming manual necesario

**PA fuera de rango:**
- Posible problema de E-steps
- Revisar temperatura (muy alta/baja)

**Retraction muy alta:**
- Riesgo de clogs
- Revisar PA primero

---

## 13. Troubleshooting por Calibración

### PID: Thermal Runaway Durante Tuning

**Error:**
```
!! Heater extruder not heating at expected rate
!! Thermal runaway
```

**Causas:**
- Heater cartridge suelto
- Thermistor mal conectado
- Voltaje insuficiente (verificar PSU)

**Solución:**
1. Apagar impresora
2. Verificar conexiones físicas (heater + thermistor)
3. Re-intentar PID a temperatura más baja (190°C)

---

### E-steps: Motor Grinding Durante Extrusión

**Síntoma:** Ruido de "click click" del extrusor durante test.

**Causas:**
- Velocidad de extrusión muy alta
- Hotend frío o parcialmente obstruido
- Corriente TMC muy baja

**Solución:**
1. Reducir velocidad: `G1 E100 F30` (era F60)
2. Aumentar temperatura: `M109 S210` (era 200)
3. Verificar TMC current en printer.cfg (debería ser 0.650)

---

### Z Offset: QUERY_PROBE Siempre "triggered"

**Síntoma:** `QUERY_PROBE` muestra "TRIGGERED" incluso con nozzle en el aire.

**Causas:**
- Drive current mal calibrado
- Interferencia electromagnética
- Sensor muy cerca de metal

**Solución:**
1. Recalibrar drive current:
   ```gcode
   LDC_CALIBRATE_DRIVE_CURRENT CHIP=btt_eddy
   SAVE_CONFIG
   ```
2. Alejar de superficies metálicas grandes
3. Verificar cable I2C separado de cables de potencia

---

### Bed Mesh: Range Muy Alto (>0.5mm)

**Síntoma:** Mesh muestra variaciones >0.5mm entre puntos.

**Causas:**
- Bed mal tramado manualmente
- Superficie bed irregular (glass torcido, etc.)
- Z-tilt no ejecutado antes del mesh

**Solución:**
1. Ejecutar Z_TILT_ADJUST primero
2. Tramar bed manualmente (ajustar tornillos)
3. Regenerar mesh
4. Si persiste, considerar upgrade a PEI sheet o glass nuevo

---

### Pressure Advance: Todos los Valores Fallan

**Síntoma:** Ningún valor de PA (0.0 - 0.1) mejora esquinas.

**Causas:**
- E-steps mal calibrados (calibrar primero)
- Flow rate incorrecto
- Temperatura muy alta/baja
- Problema mecánico (atasco parcial)

**Solución:**
1. **Primero:** Verificar E-steps calibrados
2. Verificar temperatura dentro de rango del filamento
3. Test de extrusión manual (debe ser suave)
4. Limpiar nozzle (cold pull)

---

### Input Shaper: ACCELEROMETER_QUERY Falla

**Error:**
```
!! adxl345: Failed to read chip ID
```

**Causas:**
- Cable SPI mal conectado
- Pin incorrecto en printer.cfg
- ADXL345 dañado

**Solución:**
1. Verificar configuración en printer.cfg línea 370:
   ```ini
   [adxl345]
   cs_pin: EBBCan:PB12
   spi_software_sclk_pin: EBBCan:PB10
   spi_software_mosi_pin: EBBCan:PB11
   spi_software_miso_pin: EBBCan:PB2
   ```
2. Revisar conexiones físicas en EBB42
3. Test con `ACCELEROMETER_QUERY` después de `RESTART`

---

## Próximos Pasos

✅ **Calibración completa finalizada**

**Siguiente:**
1. Primera impresión real → [ORCA_SLICER_SETUP.md](ORCA_SLICER_SETUP.md) sección Primera Impresión
2. Ajuste fino basado en resultados
3. ¡Producción regular!

---

## Referencias

### Documentos Relacionados

- [CONFIGURACION_KLIPPER.md](CONFIGURACION_KLIPPER.md) - Configuración detallada printer.cfg
- [EDDY_COIL_CALIBRATION.md](EDDY_COIL_CALIBRATION.md) - Calibración específica Eddy Coil
- [ORCA_SLICER_SETUP.md](ORCA_SLICER_SETUP.md) - Configuración slicer

### Recursos Externos

**Klipper Calibration:**
- Official Calibration: https://www.klipper3d.org/Rotation_Distance.html
- Pressure Advance: https://www.klipper3d.org/Pressure_Advance.html
- Resonance Compensation: https://www.klipper3d.org/Resonance_Compensation.html

**Community Guides:**
- Ellis' Print Tuning: https://ellis3dp.com/Print-Tuning-Guide/
- Teaching Tech Calibration: https://teachingtechyt.github.io/calibration.html

---

**Versión:** 1.0
**Fecha:** 2025-12-26
**Autor:** Documentación Phase 3 - Tronxy X5SA Klipper Migration
**Configuración:** `/Users/mjcuadrado/projects/x5sa-skr-klipper/klipper/config/printer.cfg`
