# Configuración OrcaSlicer - Phase 3

**Fecha:** 2025-12-26
**Versión:** 1.0
**Slicer:** OrcaSlicer 1.9.0+
**Hardware:** Tronxy X5SA + Klipper (SKR 1.4 Turbo + EBB42)

---

## Índice

1. [Resumen de Perfiles](#1-resumen-de-perfiles)
2. [Instalación de Perfiles](#2-instalación-de-perfiles)
3. [Configuración del Printer Profile](#3-configuración-del-printer-profile)
4. [Perfiles de Filamento](#4-perfiles-de-filamento)
5. [Perfiles de Proceso (Calidad)](#5-perfiles-de-proceso-calidad)
6. [Integración con Macros Klipper](#6-integración-con-macros-klipper)
7. [Calibración Requerida](#7-calibración-requerida)
8. [Primera Impresión](#8-primera-impresión)
9. [Troubleshooting](#9-troubleshooting)

---

## 1. Resumen de Perfiles

### Perfiles Creados

**Ubicación:** `/Users/mjcuadrado/projects/x5sa-skr-klipper/orca-slicer-profiles/`

| Archivo | Tipo | Descripción |
|---------|------|-------------|
| `printer_tronxy_x5sa_klipper.json` | Printer | Configuración principal impresora |
| `filament_pla.json` | Filament | PLA genérico (210°C / 60°C bed) |
| `filament_petg.json` | Filament | PETG genérico (240°C / 80°C bed) |
| `filament_abs.json` | Filament | ABS genérico (250°C / 100°C bed) |
| `process_draft.json` | Process | Draft 0.30mm (rápido) |
| `process_standard.json` | Process | Standard 0.20mm (balanceado) |
| `process_fine.json` | Process | Fine 0.10mm (calidad) |

### Características Principales

**Integración Klipper:**
- ✅ Macros `PRINT_START` / `PRINT_END`
- ✅ Firmware retraction (G10/G11)
- ✅ Adaptive bed meshing con rapid_scan
- ✅ Exclude objects habilitado
- ✅ Pressure advance configurado por filamento

**Optimizaciones:**
- ✅ Velocidades ajustadas para CoreXY
- ✅ Retracción optimizada para direct drive
- ✅ Cooling por tipo de filamento
- ✅ Preparado para multicolor (Phase 12)

---

## 2. Instalación de Perfiles

### Método 1: Importación Individual (Recomendado)

Este método es el más fácil y funciona en todas las plataformas.

#### Paso 1: Abrir OrcaSlicer

Descargar e instalar OrcaSlicer 1.9.0 o superior:
- https://github.com/SoftFever/OrcaSlicer/releases

#### Paso 2: Importar Printer Profile

1. En OrcaSlicer, ir a pestaña **Printer**
2. Hacer clic en el **icono de engranaje** (configuración)
3. Seleccionar **Import**
4. Navegar a: `/Users/mjcuadrado/projects/x5sa-skr-klipper/orca-slicer-profiles/`
5. Seleccionar: `printer_tronxy_x5sa_klipper.json`
6. Hacer clic en **Open**

**Resultado esperado:**
- Nuevo printer "Tronxy X5SA Klipper" aparece en la lista de impresoras

#### Paso 3: Importar Filament Profiles

1. Ir a pestaña **Filament**
2. Hacer clic en el **icono de engranaje**
3. Seleccionar **Import**
4. Seleccionar: `filament_pla.json`
5. Hacer clic en **Open**
6. **Repetir** para `filament_petg.json` y `filament_abs.json`

**Resultado esperado:**
- 3 nuevos filamentos en la lista:
  - Generic PLA (Tronxy)
  - Generic PETG (Tronxy)
  - Generic ABS (Tronxy)

#### Paso 4: Importar Process Profiles

1. Ir a pestaña **Process**
2. Hacer clic en el **icono de engranaje**
3. Seleccionar **Import**
4. Seleccionar: `process_draft.json`
5. Hacer clic en **Open**
6. **Repetir** para `process_standard.json` y `process_fine.json`

**Resultado esperado:**
- 3 nuevos procesos en la lista:
  - 0.30mm Draft (Tronxy)
  - 0.20mm Standard (Tronxy)
  - 0.10mm Fine (Tronxy)

#### Paso 5: Verificar Instalación

1. Seleccionar printer: **Tronxy X5SA Klipper**
2. Seleccionar filament: **Generic PLA (Tronxy)**
3. Seleccionar process: **0.20mm Standard (Tronxy)**

Si todo funciona correctamente, puedes cargar un STL y slice.

---

### Método 2: Copia Manual de Archivos (Avanzado)

**Solo para usuarios avanzados.** Permite control directo sobre el directorio de perfiles.

#### Ubicaciones del Directorio OrcaSlicer

**macOS:**
```bash
~/Library/Application Support/OrcaSlicer/user/
```

**Windows:**
```
%APPDATA%\OrcaSlicer\user\
```

**Linux:**
```bash
~/.config/OrcaSlicer/user/
```

#### Pasos

1. **Cerrar OrcaSlicer** (importante)

2. **Navegar al directorio de usuario:**
   ```bash
   cd ~/Library/Application\ Support/OrcaSlicer/user/
   ```

3. **Crear estructura de directorios:**
   ```bash
   mkdir -p Tronxy/machine Tronxy/filament Tronxy/process
   ```

4. **Copiar perfiles:**
   ```bash
   # Printer profile
   cp /Users/mjcuadrado/projects/x5sa-skr-klipper/orca-slicer-profiles/printer_tronxy_x5sa_klipper.json \
      Tronxy/machine/

   # Filament profiles
   cp /Users/mjcuadrado/projects/x5sa-skr-klipper/orca-slicer-profiles/filament_*.json \
      Tronxy/filament/

   # Process profiles
   cp /Users/mjcuadrado/projects/x5sa-skr-klipper/orca-slicer-profiles/process_*.json \
      Tronxy/process/
   ```

5. **Reiniciar OrcaSlicer**

Los perfiles deberían aparecer automáticamente en sus respectivas pestañas.

---

## 3. Configuración del Printer Profile

### Especificaciones Técnicas

El profile `printer_tronxy_x5sa_klipper.json` incluye:

**Build Volume:**
- X: 330mm
- Y: 330mm
- Z: 400mm

**Kinematics:**
- Tipo: CoreXY
- Max velocity: 300 mm/s
- Max acceleration: 3000 mm/s²

**Extruder (Phase 3-11):**
- Tipo: Direct drive (Titan clone)
- Nozzle: 0.4mm
- Max temp: 300°C

**Heated Bed:**
- Max temp: 130°C
- Sensor: Thermistor NTC 100K

**Probe:**
- Tipo: BIGTREETECH Eddy Coil V1.0
- Offset X: 0mm (centrado con nozzle)
- Offset Y: 0mm (centrado con nozzle)

### G-code de Inicio (Start G-code)

```gcode
M104 S0 ; Stops OrcaSlicer from sending temp waits separately
M140 S0
PRINT_START BED_TEMP=[bed_temperature_initial_layer_single] EXTRUDER_TEMP=[nozzle_temperature_initial_layer]
```

**Qué hace:**

1. `M104 S0` y `M140 S0`: Desactiva comandos de temperatura automáticos de OrcaSlicer
2. `PRINT_START`: Llama al macro de Klipper con temperaturas del perfil de filamento
3. Variables automáticas:
   - `[bed_temperature_initial_layer_single]` → temp bed de primera capa
   - `[nozzle_temperature_initial_layer]` → temp nozzle de primera capa

**El macro PRINT_START ejecuta:**
- Heating bed + precalentar extrusor a 150°C
- Homing (G28)
- Z-tilt adjustment (dual Z leveling)
- Adaptive bed mesh con rapid_scan (~15 segundos)
- Heating extrusor a temp de impresión
- Purge line (2 líneas paralelas)

**Ver detalles:** [CONFIGURACION_KLIPPER.md](CONFIGURACION_KLIPPER.md) sección PRINT_START

### G-code de Finalización (End G-code)

```gcode
PRINT_END
```

**El macro PRINT_END ejecuta:**
- Levanta Z 10mm (safe retract)
- Aparca toolhead en parte trasera
- Apaga heaters (extrusor + bed)
- Apaga ventilador de capa
- Retrae 5mm filamento (prevenir oozing)
- Desactiva steppers
- Limpia bed mesh de la memoria

**Ver detalles:** [CONFIGURACION_KLIPPER.md](CONFIGURACION_KLIPPER.md) sección PRINT_END

### Cambio de Filamento (Tool Change / Pause)

El perfil soporta cambio de filamento con `M600`:

```gcode
M600 ; Pause for filament change
```

OrcaSlicer enviará este comando en la altura especificada, y Klipper ejecutará el macro `PAUSE`:
- Retrae 2mm
- Levanta Z 10mm
- Aparca en frente izquierdo
- Apaga hotend

**Reanudar:** Usar botón "Resume" en Mainsail/Fluidd o comando `RESUME`

---

## 4. Perfiles de Filamento

### 4.1 Generic PLA (filament_pla.json)

**Temperaturas:**
- Nozzle: 210°C (primera capa) / 210°C (otras capas)
- Bed: 60°C (primera capa) / 60°C (otras capas)
- Rango: 190-220°C nozzle / 50-70°C bed

**Cooling:**
- Capa 1: 0% (sin cooling)
- Capa 2: 20%
- Capa 3+: 100% (máximo cooling)
- Overhangs: 100%
- Bridges: 100%

**Retraction (Firmware Retraction):**
- Distancia: 1.0mm
- Velocidad: 40mm/s
- Configurado en Klipper, no en slicer

**Pressure Advance:**
- Valor inicial: 0.04 (comentado en start G-code)
- **DEBE calibrarse** para tu filamento específico

**Cuándo usar PLA:**
- Prototipos
- Piezas decorativas
- Buena adhesión sin calentamiento excesivo
- No requiere enclosure

**Limitaciones:**
- Baja resistencia térmica (<60°C)
- Frágil en piezas mecánicas
- No para exterior (UV degradation)

---

### 4.2 Generic PETG (filament_petg.json)

**Temperaturas:**
- Nozzle: 240°C (primera capa) / 240°C (otras capas)
- Bed: 85°C (primera capa) / 80°C (otras capas)
- Rango: 230-250°C nozzle / 75-90°C bed

**Cooling:**
- Capa 1-3: 0% (sin cooling, prevenir warping)
- Capa 4+: 20-50% (cooling mínimo)
- Overhangs: 50%
- Bridges: 80%

**CRÍTICO para PETG:**
- **Cooling bajo** es esencial para prevenir warping
- **Bed adhesión fuerte:** usar glue stick o PEI
- **Dejar enfriar** antes de remover pieza (bonding muy fuerte)

**Retraction:**
- Distancia: 1.2mm (más que PLA, PETG tiende a stringing)
- Velocidad: 35mm/s (más lento que PLA)

**Pressure Advance:**
- Valor inicial: 0.055 (mayor que PLA)

**Cuándo usar PETG:**
- Piezas funcionales
- Mayor resistencia mecánica que PLA
- Resistencia química moderada
- Uso exterior (UV resistente)

**Limitaciones:**
- Stringing común (requiere buen tuning de retracción/PA)
- Sensible a humedad (almacenar con desecante)
- Requiere bed limpio (IPA + glue stick)

---

### 4.3 Generic ABS (filament_abs.json)

**Temperaturas:**
- Nozzle: 250°C (primera capa) / 250°C (otras capas)
- Bed: 105°C (primera capa) / 100°C (otras capas)
- Rango: 240-260°C nozzle / 95-110°C bed

**Cooling:**
- Capa 1-10: 0% (sin cooling)
- Capa 11+: 0-30% (muy bajo)
- Overhangs: 30%
- Bridges: 50%

**⚠️ REQUERIMIENTOS CRÍTICOS:**

1. **ENCLOSURE OBLIGATORIO:**
   - Tronxy X5SA NO tiene enclosure de fábrica
   - **DEBE fabricarse** antes de imprimir ABS
   - Temperatura ambiente: >40°C dentro del enclosure

2. **VENTILACIÓN:**
   - ABS emite **styrene fumes** (tóxico)
   - Imprimir en área ventilada
   - Considerar extractor de aire

3. **BED ADHESION:**
   - Crítico para prevenir warping
   - Usar ABS slurry (ABS disuelto en acetona)
   - O glue stick muy generoso
   - Brim o raft para piezas grandes

**Retraction:**
- Distancia: 1.0mm
- Velocidad: 40mm/s

**Pressure Advance:**
- Valor inicial: 0.045

**Cuándo usar ABS:**
- Máxima resistencia mecánica
- Resistencia térmica alta (>90°C)
- Post-procesado con acetone vapor smoothing
- Piezas para alta temperatura

**Limitaciones:**
- **Requiere enclosure** (no incluido en X5SA)
- Fumes tóxicos (ventilación obligatoria)
- Warping severo sin ambiente controlado
- Sensible a corrientes de aire

**ADVERTENCIA:** NO imprimir ABS en Tronxy X5SA sin enclosure. Resultados garantizados: warping, delaminación, fallos.

---

## 5. Perfiles de Proceso (Calidad)

### 5.1 Draft Profile (0.30mm) - process_draft.json

**Para:** Prototipos, test fits, piezas grandes funcionales

| Setting | Value | Notas |
|---------|-------|-------|
| **Layer Height** | 0.30mm | Máximo para nozzle 0.4mm |
| **First Layer Height** | 0.25mm | Mejor adhesión |
| **Wall Loops** | 2 | Perímetros mínimos |
| **Top Solid Layers** | 4 | Suficiente para draft |
| **Bottom Solid Layers** | 3 | |
| **Infill Density** | 15% | Lightweight |
| **Infill Pattern** | Adaptive Cubic | Rápido y fuerte |

**Velocidades:**
- Outer wall: 80 mm/s
- Inner wall: 100 mm/s
- Sparse infill: 150 mm/s
- Top surface: 60 mm/s
- First layer: 30 mm/s

**Tiempo de impresión:**
- ~30-40% más rápido que Standard
- Pieza de 100g: ~2-3 horas (vs 4-5 Standard)

**Calidad esperada:**
- Líneas visibles (0.3mm es perceptible)
- Buena para función, pobre para estética
- Superficies NO suaves

**Cuándo usar:**
- ✅ Prototipos rápidos
- ✅ Test de ensamblaje
- ✅ Soportes/jigs temporales
- ❌ Piezas finales visibles
- ❌ Miniaturas/detalles finos

---

### 5.2 Standard Profile (0.20mm) - process_standard.json

**Para:** Uso general, balance calidad/velocidad

| Setting | Value | Notas |
|---------|-------|-------|
| **Layer Height** | 0.20mm | Sweet spot para 0.4mm nozzle |
| **First Layer Height** | 0.20mm | |
| **Wall Loops** | 3 | Buena resistencia |
| **Top Solid Layers** | 5 | Superficie sólida |
| **Bottom Solid Layers** | 4 | |
| **Infill Density** | 15% | Balance |
| **Infill Pattern** | Adaptive Cubic | |

**Velocidades:**
- Outer wall: 60 mm/s
- Inner wall: 80 mm/s
- Sparse infill: 120 mm/s
- Top surface: 50 mm/s
- First layer: 30 mm/s

**Tiempo de impresión:**
- Baseline de referencia
- Pieza de 100g: ~4-5 horas

**Calidad esperada:**
- Buena calidad general
- Líneas apenas visibles
- Buena para piezas funcionales y decorativas

**Cuándo usar:**
- ✅ **Default recomendado** (90% de casos)
- ✅ Piezas funcionales
- ✅ Piezas decorativas generales
- ✅ Balance tiempo/calidad óptimo

---

### 5.3 Fine Profile (0.10mm) - process_fine.json

**Para:** Miniaturas, detalles finos, showcase pieces

| Setting | Value | Notas |
|---------|-------|-------|
| **Layer Height** | 0.10mm | Máxima calidad |
| **First Layer Height** | 0.20mm | Mejor adhesión que 0.10 |
| **Wall Loops** | 4 | Superficie ultra suave |
| **Top Solid Layers** | 7 | Sólido garantizado |
| **Bottom Solid Layers** | 6 | |
| **Infill Density** | 15% | No afecta calidad exterior |
| **Infill Pattern** | Adaptive Cubic | |

**Velocidades:**
- Outer wall: 50 mm/s (lento para precisión)
- Inner wall: 60 mm/s
- Sparse infill: 100 mm/s
- Top surface: 40 mm/s
- First layer: 25 mm/s

**Tiempo de impresión:**
- ~2-3× más lento que Standard
- Pieza de 100g: ~10-12 horas

**Calidad esperada:**
- Superficie ultra suave
- Detalles finos capturados
- Líneas casi invisibles

**Cuándo usar:**
- ✅ Miniaturas (D&D, wargaming)
- ✅ Showcase pieces
- ✅ Piezas con texto pequeño
- ✅ Detalles intrincados
- ❌ Prototipos (demasiado lento)
- ❌ Piezas grandes (tiempo prohibitivo)

**Limitación:** Primera capa a 0.20mm (no 0.10mm) para mejor adhesión. El resto de capas son 0.10mm.

---

## 6. Integración con Macros Klipper

### 6.1 PRINT_START

**Llamado automáticamente** por el start G-code de OrcaSlicer:

```gcode
PRINT_START BED_TEMP=[bed_temperature_initial_layer_single] EXTRUDER_TEMP=[nozzle_temperature_initial_layer]
```

**Variables pasadas desde OrcaSlicer:**
- `BED_TEMP`: Temperatura bed de primera capa (del perfil de filamento)
- `EXTRUDER_TEMP`: Temperatura extrusor de primera capa

**Flujo completo:**

```
OrcaSlicer slice → Genera G-code
  ↓
G-code incluye: PRINT_START BED_TEMP=60 EXTRUDER_TEMP=210
  ↓
Klipper ejecuta macro PRINT_START:
  1. Calentar bed a 60°C (no espera)
  2. Precalentar extrusor a 150°C (prevenir oozing)
  3. Home all (G28)
  4. Esperar bed a 60°C
  5. Z-tilt adjust (dual Z leveling)
  6. Adaptive bed mesh con rapid_scan (~15 seg)
  7. Mover a posición purga (X5 Y5)
  8. Calentar extrusor a 210°C (espera)
  9. Purge line (30mm filamento)
  10. Comenzar impresión
```

**NO modificar** el start G-code en OrcaSlicer a menos que entiendas completamente el flujo.

---

### 6.2 Adaptive Bed Meshing

El macro PRINT_START incluye:

```gcode
BED_MESH_CALIBRATE METHOD=rapid_scan ADAPTIVE=1
```

**Qué es adaptive meshing:**

En lugar de sondar toda la cama (330×330mm), solo sondea:
- Área de impresión (calculada desde G-code)
- + 5mm de margen (`adaptive_margin: 5` en printer.cfg)

**Ejemplos:**

| Pieza | Área de impresión | Área sondeo | Puntos | Tiempo |
|-------|-------------------|-------------|--------|--------|
| Calibration cube 20mm | 20×20mm | 30×30mm | ~9 (3×3 auto) | ~6 seg |
| Pieza mediana 100×100mm | 100×100mm | 110×110mm | ~25 (5×5) | ~12 seg |
| Pieza grande 300×300mm | 300×300mm | 310×310mm (max) | ~25 (5×5) | ~15 seg |

**Ventajas:**
- Inicio de impresión MUY rápido
- Reduce desgaste del probe
- Mesh siempre relevante para la pieza actual

**Desactivar adaptive mesh:**

Si prefieres SIEMPRE mesh completo, editar macro PRINT_START en printer.cfg:

```gcode
BED_MESH_CALIBRATE METHOD=rapid_scan ADAPTIVE=1
# Cambiar a:
BED_MESH_CALIBRATE METHOD=rapid_scan
```

---

### 6.3 Firmware Retraction

Todos los perfiles usan **firmware retraction**:

```json
"use_firmware_retraction": "1"
```

**Cómo funciona:**

1. OrcaSlicer genera G-code con `G10` (retract) y `G11` (unretract)
2. Klipper ejecuta retracción según configuración en `printer.cfg`:
   ```ini
   [firmware_retraction]
   retract_length: 1.0
   retract_speed: 40
   ```
3. Puedes ajustar retracción **en tiempo real** durante impresión:
   ```gcode
   SET_RETRACTION RETRACT_LENGTH=1.2 RETRACT_SPEED=40
   ```

**Ventajas vs slicer retraction:**
- Ajustes en tiempo real sin re-slicear
- Consistencia entre slicers
- Calibración centralizada en Klipper

**Si ves stringing:**
1. Durante impresión: `SET_RETRACTION RETRACT_LENGTH=1.5`
2. Si mejora, actualizar en `printer.cfg`
3. Re-slicear NO es necesario

---

### 6.4 Pressure Advance

Los perfiles de filamento incluyen **comentarios** con valores iniciales de PA:

**PLA start G-code:**
```gcode
; Recommended pressure advance: 0.04
; Uncomment to enable: SET_PRESSURE_ADVANCE ADVANCE=0.04
```

**Activar Pressure Advance:**

1. Descomentar línea en filament profile (pestaña Filament → Filament Settings → Custom G-code)
2. O agregar en `printer.cfg` bajo `[extruder]`:
   ```ini
   pressure_advance: 0.04  # Para PLA
   ```

**Calibrar PA:**

Ver guía completa: [CALIBRACION_COMPLETA.md](CALIBRACION_COMPLETA.md) sección Pressure Advance

Usar generador de Ellis:
- https://ellis3dp.com/Print-Tuning-Guide/articles/pressure_advance.html

**Valores iniciales por filamento:**
- PLA: 0.04
- PETG: 0.055
- ABS: 0.045

**Ajustar durante impresión:**
```gcode
SET_PRESSURE_ADVANCE ADVANCE=0.05
```

---

## 7. Calibración Requerida

### Checklist ANTES de la Primera Impresión

**Hardware:**
- [ ] ✅ SKR 1.4 Turbo flasheada y detectada
- [ ] ✅ EBB42 flasheada y detectada
- [ ] ✅ Dual MCU configurado en printer.cfg
- [ ] ✅ Heater funcional (puede calentar a 200°C)
- [ ] ✅ Bed funcional (puede calentar a 60°C)
- [ ] ✅ Fans funcionan (part cooling + hotend fan)

**Eddy Coil Probe:**
- [ ] ✅ `QUERY_PROBE` funciona (open/triggered)
- [ ] ✅ Drive current calibrado (`LDC_CALIBRATE_DRIVE_CURRENT`)
- [ ] ✅ Z offset calibrado (`PROBE_EDDY_CURRENT_CALIBRATE`)
- [ ] ✅ `PROBE_ACCURACY` muestra range < 0.05mm

**Klipper Tuning:**
- [ ] ✅ PID hotend calibrado (`PID_TUNE_HOTEND TARGET=200`)
- [ ] ✅ PID bed calibrado (`PID_TUNE_BED TARGET=60`)
- [ ] ✅ E-steps calibrados (rotation_distance)
- [ ] ✅ Sensorless homing funciona (X y Y)
- [ ] ✅ Dual Z nivelado (`Z_TILT_ADJUST` converge)
- [ ] ✅ Bed mesh generado (`GENERATE_BED_MESH`)

**OrcaSlicer:**
- [ ] ✅ Perfiles importados correctamente
- [ ] ✅ Printer seleccionado: Tronxy X5SA Klipper
- [ ] ✅ Filament seleccionado: Generic PLA
- [ ] ✅ Process seleccionado: 0.20mm Standard

**Opcional pero Recomendado:**
- [ ] Pressure advance calibrado por filamento
- [ ] Retraction calibrado (si hay stringing)
- [ ] Input shaper calibrado (ADXL345)
- [ ] First layer Z offset ajustado (baby stepping)

**Ver guía completa:** [CALIBRACION_COMPLETA.md](CALIBRACION_COMPLETA.md)

---

## 8. Primera Impresión

### Test Recomendado: Calibration Cube 20mm

**Archivo STL:** Buscar "XYZ Calibration Cube 20mm" en Thingiverse/Printables

**Configuración:**
- Printer: Tronxy X5SA Klipper
- Filament: Generic PLA (Tronxy)
- Process: 0.20mm Standard (Tronxy)
- Infill: 15% (default)
- Supports: NO (cube no necesita)

**Slicing:**

1. Importar STL en OrcaSlicer
2. Verificar orientación (flat side down)
3. Centrar en bed (auto-center)
4. Hacer slice
5. Verificar preview:
   - Primera capa debe cubrir completamente
   - Purge line fuera del área de impresión
   - Sin warnings de colisiones

**Antes de Imprimir:**

1. Verificar temperaturas en G-code preview:
   - Bed: 60°C
   - Extruder: 210°C

2. Verificar start G-code incluye:
   ```gcode
   PRINT_START BED_TEMP=60 EXTRUDER_TEMP=210
   ```

3. Verificar end G-code incluye:
   ```gcode
   PRINT_END
   ```

**Durante la Impresión:**

**Fase de inicio (~2-3 minutos):**
- Bed calentando a 60°C
- Extrusor precalentando a 150°C
- G28 homing (sensorless X/Y, probe Z)
- Z_TILT_ADJUST (dual Z leveling)
- Adaptive bed mesh (~6 segundos para 20mm cube)
- Extrusor calentando a 210°C
- Purge line

**CRITICAL: Monitorear Primera Capa**

- [ ] Purge line se adhiere correctamente
- [ ] Primera línea de perímetro se adhiere
- [ ] NO hay gaps entre líneas
- [ ] NO hay exceso de squishing (líneas muy planas/anchas)
- [ ] Nozzle NO arrastra filamento

**Si primera capa falla:**

**Problema: Nozzle muy lejos (no adhiere)**
```gcode
SET_GCODE_OFFSET Z_ADJUST=-0.05 MOVE=1
```
Repetir en decrementos de -0.05 hasta que adhiera.

**Problema: Nozzle muy cerca (squishing excesivo)**
```gcode
SET_GCODE_OFFSET Z_ADJUST=0.05 MOVE=1
```
Repetir en incrementos de +0.05 hasta correcto.

**Guardar ajuste permanente:**
```gcode
Z_OFFSET_APPLY_PROBE
SAVE_CONFIG
```

**Resto de impresión:**
- Observar calidad de capas
- Escuchar ruidos anormales (skip steps, grinding)
- Monitorear temperaturas estables

**Tiempo esperado:** ~45-60 minutos (20mm cube, Standard profile)

---

### Evaluación del Resultado

**Mediciones:**

Con calibre, medir el cubo:
- X: debe ser ~20.0mm (±0.2mm aceptable)
- Y: debe ser ~20.0mm (±0.2mm aceptable)
- Z: debe ser ~20.0mm (±0.2mm aceptable)

**Si dimensiones incorrectas:**
- X/Y incorrectos: verificar `rotation_distance` de steppers
- Z incorrecto: verificar `rotation_distance` stepper_z

**Calidad visual:**

- [ ] Primera capa uniforme
- [ ] Sin gaps entre líneas
- [ ] Esquinas afiladas (no redondeadas)
- [ ] Superficies superiores sólidas (sin huecos)
- [ ] Sin stringing visible

**Si hay defectos:**
- Stringing: calibrar retraction/pressure advance
- Gaps: aumentar flow rate o verificar E-steps
- Warping: verificar bed adhesión y temperatura
- Layer shifting: verificar belt tension y corrientes TMC

---

## 9. Troubleshooting

### 9.1 Primera Capa NO Adhiere

**Síntomas:**
- Filamento no se pega al bed
- Se enrolla alrededor del nozzle
- Se arrastra

**Causas y soluciones:**

1. **Z offset muy alto**
   ```gcode
   SET_GCODE_OFFSET Z_ADJUST=-0.05 MOVE=1
   ```
   Guardar si funciona: `Z_OFFSET_APPLY_PROBE` → `SAVE_CONFIG`

2. **Bed sucio**
   - Limpiar con IPA (alcohol isopropílico)
   - Usar glue stick para PETG/ABS

3. **Bed frío**
   - Verificar temperatura alcanza target
   - Para PLA: aumentar a 65°C
   - Para PETG: aumentar a 90°C

4. **Bed desnivelado**
   - Ejecutar: `Z_TILT_ADJUST`
   - Regenerar mesh: `GENERATE_BED_MESH`

---

### 9.2 Stringing (Hilos entre Movimientos)

**Síntomas:**
- Hilos finos entre partes de la impresión
- "Cabello" en la pieza

**Soluciones:**

1. **Aumentar retraction**
   ```gcode
   SET_RETRACTION RETRACT_LENGTH=1.5 RETRACT_SPEED=40
   ```
   Si mejora, actualizar en `printer.cfg`

2. **Reducir temperatura**
   - PLA: probar 205°C (era 210°C)
   - PETG: probar 235°C (era 240°C)

3. **Calibrar Pressure Advance**
   - Ver: [CALIBRACION_COMPLETA.md](CALIBRACION_COMPLETA.md)
   - Usar generador de Ellis

4. **Secar filamento**
   - PETG/ABS MUY sensibles a humedad
   - Hornear a 50°C por 4-6 horas
   - Almacenar con desecante

---

### 9.3 Warping (Esquinas Levantan)

**Síntomas:**
- Esquinas de la pieza se levantan del bed
- Delaminación de capas inferiores

**Soluciones:**

1. **Aumentar temperatura bed**
   - PLA: probar 65°C
   - PETG: probar 90°C
   - ABS: **REQUIERE ENCLOSURE**

2. **Mejorar adhesión**
   - Glue stick generoso
   - Laca para cabello
   - PEI sheet (upgrade recomendado)

3. **Agregar brim en OrcaSlicer**
   - Process settings → Brim width: 5mm
   - Aumenta área de contacto

4. **Reducir cooling primera capa**
   - Filament settings → Cooling → Layer 1: 0%

5. **Cerrar ventanas/corrientes de aire**
   - Temperatura ambiente variable causa warping
   - ABS: **enclosure obligatorio**

---

### 9.4 Layer Shifting (Desplazamiento de Capas)

**Síntomas:**
- Capas desalineadas en X o Y
- Impresión "shifted" desde cierta altura

**Causas y soluciones:**

1. **Belt tension**
   - Verificar tensión de correas X/Y
   - Deben estar firmes como cuerda de guitarra
   - Apretar si están flojas

2. **TMC current demasiado bajo**
   - Motores pierden pasos
   - Verificar `run_current` en printer.cfg:
     ```ini
     [tmc2209 stepper_x]
     run_current: 0.800  # Aumentar a 0.850 si persiste
     ```

3. **Velocidad/aceleración muy alta**
   - Reducir temporalmente:
     ```gcode
     SET_VELOCITY_LIMIT VELOCITY=200 ACCEL=2000
     ```
   - Si mejora, ajustar en printer.cfg

4. **Sensorless homing mal calibrado**
   - Aumentar `driver_SGTHRS` en printer.cfg
   - Ver: [CONFIGURACION_KLIPPER.md](CONFIGURACION_KLIPPER.md)

---

### 9.5 Hotend Temperature Fluctuating

**Síntomas:**
- Temperatura oscila ±5°C
- Mensaje "heating failed" o "thermal runaway"

**Soluciones:**

1. **Recalibrar PID**
   ```gcode
   PID_TUNE_HOTEND TARGET=210
   ```

2. **Part cooling fan muy fuerte**
   - Reducir max fan speed en filament profile
   - O redirigir fan (no debe soplar al heater block)

3. **Verificar thermistor connection**
   - Revisar cable en EBB42 TH0
   - Verificar no hay cortocircuitos

---

### 9.6 Adaptive Mesh NO Funciona

**Síntomas:**
- Mesh siempre usa área completa
- O error durante `BED_MESH_CALIBRATE ADAPTIVE=1`

**Soluciones:**

1. **Verificar exclude_object habilitado**

   En printer.cfg debe existir:
   ```ini
   [exclude_object]
   ```

2. **Verificar adaptive_margin configurado**
   ```ini
   [bed_mesh]
   adaptive_margin: 5
   ```

3. **Usar Klipper version actualizada**
   - Adaptive meshing requiere Klipper relativamente reciente
   - Actualizar a latest: `cd ~/klipper && git pull`

4. **Fallback a mesh completo**

   Si persiste problema, editar macro PRINT_START:
   ```gcode
   BED_MESH_CALIBRATE METHOD=rapid_scan
   # Eliminar ADAPTIVE=1
   ```

---

### 9.7 Print NO Inicia (Klipper Error)

**Síntomas:**
- Al enviar G-code, Klipper muestra error
- Impresión no comienza

**Errores comunes:**

**"MCU not connected"**
- Verificar cables USB (SKR y EBB42)
- Verificar serial IDs en printer.cfg
- Reiniciar Klipper: `sudo systemctl restart klipper`

**"Probe not triggered"**
- Ejecutar: `VERIFY_EDDY_PROBE`
- Verificar Eddy Coil funciona con `QUERY_PROBE`

**"Z_TILT_ADJUST failed to converge"**
- Bed muy desnivelado
- Ajustar manualmente tornillos del bed
- Re-intentar: `Z_TILT_ADJUST`

**"Move out of range"**
- G-code intenta mover fuera de límites configurados
- Verificar `position_max` en printer.cfg
- Re-centrar pieza en OrcaSlicer

---

## 10. Recursos Adicionales

### Documentación del Proyecto

| Documento | Tema |
|-----------|------|
| [CONFIGURACION_KLIPPER.md](CONFIGURACION_KLIPPER.md) | Configuración detallada printer.cfg |
| [CALIBRACION_COMPLETA.md](CALIBRACION_COMPLETA.md) | Checklist completo de calibración |
| [EDDY_COIL_CALIBRATION.md](EDDY_COIL_CALIBRATION.md) | Calibración del probe |
| [EBB42_INTEGRATION.md](EBB42_INTEGRATION.md) | Integración hardware EBB42 |

### Recursos Externos

**Klipper:**
- Official Docs: https://www.klipper3d.org/
- Config Reference: https://www.klipper3d.org/Config_Reference.html
- Pressure Advance: https://www.klipper3d.org/Pressure_Advance.html

**Calibración:**
- Ellis' Print Tuning Guide: https://ellis3dp.com/Print-Tuning-Guide/
- Teaching Tech Calibration: https://teachingtechyt.github.io/calibration.html

**OrcaSlicer:**
- GitHub: https://github.com/SoftFever/OrcaSlicer
- Wiki: https://github.com/SoftFever/OrcaSlicer/wiki
- Profile Management: https://www.obico.io/blog/orcaslicer-comprehensive-profile-management-guide/

**BTT Hardware:**
- SKR 1.4 Turbo: https://github.com/bigtreetech/SKR-1.4
- EBB42: https://github.com/bigtreetech/EBB
- Eddy Coil: https://github.com/bigtreetech/Eddy

---

## Próximos Pasos

✅ **OrcaSlicer configurado**

**Siguiente:**
1. Completar calibración → [CALIBRACION_COMPLETA.md](CALIBRACION_COMPLETA.md)
2. Primera impresión de test (20mm cube)
3. Ajuste fino (PA, retraction, Z offset)
4. ¡Producción!

---

**Versión:** 1.0
**Fecha:** 2025-12-26
**Autor:** Documentación Phase 3 - Tronxy X5SA Klipper Migration
**Perfiles:** `/Users/mjcuadrado/projects/x5sa-skr-klipper/orca-slicer-profiles/`
