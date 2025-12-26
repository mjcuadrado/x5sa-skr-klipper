# Calibración BIGTREETECH Eddy Coil V1.0

**Fecha:** 2025-12-26
**Hardware:** BIGTREETECH Eddy Coil V1.0 (LDC1612)
**Prerequisito:** [EDDY_COIL_INSTALLATION.md](EDDY_COIL_INSTALLATION.md) completado
**Versión Guía:** 1.0

---

## Índice

1. [Resumen del Proceso](#1-resumen-del-proceso)
2. [Prerequisitos](#2-prerequisitos)
3. [Calibración Drive Current](#3-calibración-drive-current)
4. [Calibración Z Offset](#4-calibración-z-offset)
5. [Verificación y Pruebas](#5-verificación-y-pruebas)
6. [Ajuste Fino](#6-ajuste-fino)
7. [Macros Útiles](#7-macros-útiles)
8. [Troubleshooting](#8-troubleshooting)
9. [Mantenimiento](#9-mantenimiento)

---

## 1. Resumen del Proceso

### ¿Por qué calibrar el Eddy Coil?

A diferencia de sensores ON/OFF tradicionales, el Eddy Coil es un sensor **analógico** que mide la distancia exacta mediante inductancia. Para funcionar correctamente necesita:

1. **Drive Current calibrado:** Ajusta la corriente del sensor LDC1612 para máxima sensibilidad
2. **Z Offset calibrado:** Define la altura exacta del nozzle cuando el sensor "toca" el bed

### Flujo de calibración

```
┌──────────────────────────────────────┐
│ 1. Verificar instalación física     │
│    ✓ Eddy Coil 2-3mm sobre nozzle   │
│    ✓ Cable I2C conectado             │
│    ✓ QUERY_PROBE funciona            │
└───────────────┬──────────────────────┘
                ↓
┌──────────────────────────────────────┐
│ 2. Calibrar Drive Current            │
│    LDC_CALIBRATE_DRIVE_CURRENT       │
│    (Solo 1 vez, se guarda)           │
└───────────────┬──────────────────────┘
                ↓
┌──────────────────────────────────────┐
│ 3. Calibrar Z Offset                 │
│    PROBE_EDDY_CURRENT_CALIBRATE      │
│    (Repetir si cambias nozzle/bed)   │
└───────────────┬──────────────────────┘
                ↓
┌──────────────────────────────────────┐
│ 4. Verificar con G28 y pruebas       │
└──────────────────────────────────────┘
```

**Tiempo estimado:** 15-30 minutos (primera vez)

---

## 2. Prerequisitos

### Hardware verificado

Antes de calibrar, confirma:

- ✅ **Eddy Coil instalado físicamente** (2-3mm sobre nozzle)
- ✅ **Cable I2C conectado** a EBB42
- ✅ **`QUERY_PROBE` funciona** (responde "open" o "triggered")
- ✅ **Sistema básico operativo** (heater, fans, termistores OK)
- ✅ **X/Y homing funciona** (sensorless o endstops)

### Configuración Klipper

Verifica que `printer.cfg` contenga:

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
z_offset: 1.0              # Valor inicial, se actualizará
speed: 5.0
lift_speed: 10.0
samples: 2
samples_result: average
sample_retract_dist: 2.0
samples_tolerance: 0.050
samples_tolerance_retries: 3
```

### Preparación del sistema

1. **Bed limpio:** Limpia el bed con IPA (alcohol isopropílico)
2. **Bed frío:** NO calentar el bed durante calibración inicial
3. **Nozzle limpio:** Limpia cualquier filamento del nozzle
4. **Iluminación:** Buena luz para ver papel/calibre

---

## 3. Calibración Drive Current

### ¿Qué es el Drive Current?

El **drive current** (corriente de excitación) es la corriente que alimenta la bobina del LDC1612. Determina:
- Sensibilidad del sensor
- Rango de detección
- Consumo de energía

Klipper lo calibra **automáticamente** para tu hardware específico.

### 3.1 Ejecutar calibración

**⚠️ IMPORTANTE:** Solo ejecutar **UNA VEZ** durante la instalación inicial.

1. Abrir consola de Klipper (Mainsail/Fluidd)

2. Ejecutar comando:
   ```gcode
   LDC_CALIBRATE_DRIVE_CURRENT CHIP=btt_eddy
   ```

3. **Resultado esperado:**
   ```
   // LDC1612 calibration started...
   // Testing drive current values...
   // Optimal drive current: 15
   // Calibration complete
   // Run SAVE_CONFIG to store results
   ```

4. Guardar configuración:
   ```gcode
   SAVE_CONFIG
   ```

5. **Klipper se reiniciará automáticamente**

### 3.2 Verificar resultados

Después del reinicio, verifica `printer.cfg` (al final del archivo):

```ini
#*# <---------------------- SAVE_CONFIG ---------------------->
#*# DO NOT EDIT THIS BLOCK OR BELOW. The contents are auto-generated.
#*#
#*# [probe_eddy_current btt_eddy]
#*# reg_drive_current = 15
```

✅ **Si aparece `reg_drive_current`:** Calibración exitosa
❌ **Si NO aparece:** Ver [Troubleshooting](#8-troubleshooting)

### 3.3 ¿Cuándo recalibrar?

**Recalibrar drive current si:**
- ❌ Reemplazas el Eddy Coil por uno nuevo
- ❌ Cambias la placa EBB42

**NO recalibrar si:**
- ✅ Cambias nozzle
- ✅ Cambias bed
- ✅ Cambias hotend
- ✅ Ajustas altura del sensor

---

## 4. Calibración Z Offset

### ¿Qué es el Z Offset?

El **Z offset** define la altura del nozzle cuando el sensor detecta el bed. Es crucial para:
- Primera capa correcta
- Adhesión del filamento
- Evitar daños al nozzle/bed

### ⚠️ CHECKLIST DE SEGURIDAD PRE-CALIBRACIÓN

**ANTES de ejecutar cualquier G28 o comando de calibración, VERIFICA:**

- [ ] ✅ **Eddy Coil responde:** `QUERY_PROBE` muestra "open" en el aire
- [ ] ✅ **Eddy Coil detecta metal:** Acercar metal → `QUERY_PROBE` muestra "TRIGGERED"
- [ ] ✅ **Nozzle limpio:** Sin filamento o residuos
- [ ] ✅ **Bed limpio:** Superficie libre de objetos
- [ ] ✅ **Mano en emergency stop:** Listo para detener si algo falla
- [ ] ✅ **Z offset inicial razonable:** `z_offset: 1.0` (o similar) en printer.cfg
- [ ] ✅ **Iluminación adecuada:** Puedes ver nozzle y bed claramente

**Si algún check falla:** NO continuar. Revisar [EDDY_COIL_INSTALLATION.md](EDDY_COIL_INSTALLATION.md) sección Troubleshooting.

---

### 4.1 Método automático (recomendado)

Puedes usar el **macro simplificado** incluido en `printer.cfg`:

```gcode
CALIBRATE_EDDY_CURRENT
```

Este macro ejecuta automáticamente:
1. `LDC_CALIBRATE_DRIVE_CURRENT` (solo si no se ha hecho)
2. `G28` (home all)
3. `PROBE_EDDY_CURRENT_CALIBRATE` (calibración interactiva)

**Sigue las instrucciones en pantalla.**

### 4.2 Método manual (paso a paso)

Si prefieres control manual:

#### Paso 1: Home all axes

```gcode
G28
```

**Resultado esperado:**
- X e Y hacen homing (sensorless)
- Z hace homing con el **Eddy Coil** (virtual endstop)

⚠️ **Si Z NO usa el Eddy como endstop:**
Verifica `printer.cfg` → `[stepper_z]`:
```ini
[stepper_z]
endstop_pin: probe:z_virtual_endstop  # ← Debe ser esto
#endstop_pin: ^P1.27  # ← Comentar endstop físico
```

#### Paso 2: Iniciar calibración

```gcode
PROBE_EDDY_CURRENT_CALIBRATE CHIP=btt_eddy
```

**Klipper responderá:**
```
// Starting eddy current calibration
// Move toolhead to bed center
// Use TESTZ to adjust nozzle height
```

#### Paso 3: Ajustar altura con papel (Paper Test)

1. **Colocar papel** (A4 estándar, ~0.1mm) entre nozzle y bed

2. **Bajar nozzle** con comandos `TESTZ`:
   ```gcode
   TESTZ Z=-0.1    # Baja 0.1mm
   ```

3. **Repetir** hasta que el papel tenga **ligera resistencia** al moverlo

   - **Demasiado alto:** Papel se mueve libremente → bajar más
   - **Perfecto:** Papel tiene resistencia, pero se puede mover sin romper
   - **Demasiado bajo:** Papel atascado o roto → subir

4. **Ajustes finos:** Usar incrementos pequeños
   ```gcode
   TESTZ Z=-0.01   # Baja 0.01mm (10 micras)
   TESTZ Z=+0.01   # Sube 0.01mm
   ```

#### Paso 4: Aceptar calibración

Cuando la altura sea correcta:

```gcode
ACCEPT
```

**Klipper responderá:**
```
// Calibration accepted
// Z offset: 1.234
// Run SAVE_CONFIG to store
```

#### Paso 5: Guardar configuración

```gcode
SAVE_CONFIG
```

Klipper se reiniciará y guardará el `z_offset` en `printer.cfg`.

### 4.3 Verificar resultados

Después del reinicio, verifica `printer.cfg` (sección auto-generada):

```ini
#*# [probe_eddy_current btt_eddy]
#*# reg_drive_current = 15
#*# z_offset = 1.234
```

El valor `z_offset` depende de tu configuración física (típicamente 0.5 - 3.0mm).

---

## 5. Verificación y Pruebas

### 5.1 Test básico de probe

Ejecuta:
```gcode
QUERY_PROBE
```

**Con nozzle en el aire:**
- Resultado: `probe: open` ✅

**Con nozzle cerca del bed (<5mm):**
- Resultado: `probe: TRIGGERED` ✅

### 5.2 Test de precisión

Ejecuta múltiples probes en el mismo punto:

```gcode
PROBE_ACCURACY
```

**Resultado esperado:**
```
probe at 165.000,165.000 is z=1.234567
probe at 165.000,165.000 is z=1.234512
probe at 165.000,165.000 is z=1.234601
...
probe accuracy results: maximum 1.234601, minimum 1.234512, range 0.000089, average 1.234560, median 1.234567, standard deviation 0.000031
```

**Análisis:**
- ✅ **Range < 0.01mm (10 micras):** Excelente precisión
- ⚠️ **Range 0.01-0.05mm:** Aceptable, revisar montaje
- ❌ **Range > 0.05mm:** Problema, ver [Troubleshooting](#8-troubleshooting)

### 5.3 Test de homing Z

```gcode
G28 Z
```

**Resultado esperado:**
- Z baja lentamente
- Sensor detecta bed
- Z se detiene suavemente
- NO hay ruido de choque

⚠️ **Si escuchas choque:** Z offset demasiado bajo, recalibrar inmediatamente

### 5.4 Test de primera capa (virtual)

Sin calentar ni extruir, simula primera capa:

```gcode
G28                    # Home all
G1 Z0.2 F300           # Bajar a altura de primera capa (0.2mm)
G1 X50 Y50 F3000       # Mover a esquina
```

**Verifica:**
- Coloca papel entre nozzle y bed
- Debe tener **ligera resistencia** (similar a calibración)
- Repite en varias posiciones (centro, esquinas)

---

## 6. Ajuste Fino

### 6.1 Ajustar Z offset manualmente

Si necesitas ajustar el offset SIN recalibrar completamente:

**En Mainsail/Fluidd durante una impresión:**

1. Observa la primera capa
2. Ajusta con "Baby Stepping" (Z offset live):
   - Demasiado separada: Bajar offset (-0.01 a -0.05mm)
   - Demasiado aplastada: Subir offset (+0.01 a +0.05mm)

**Guardar ajuste permanente:**

```gcode
Z_OFFSET_APPLY_PROBE
SAVE_CONFIG
```

### 6.2 Ajustar offsets X/Y

Si el Eddy Coil NO está centrado con el nozzle, mide los offsets:

1. **Home all:**
   ```gcode
   G28
   ```

2. **Posicionar nozzle en punto conocido:**
   ```gcode
   G1 X100 Y100 Z10 F3000
   ```

3. **Marcar posición del nozzle** en el bed (lápiz o cinta)

4. **Hacer probe en ese punto:**
   ```gcode
   PROBE
   ```

5. **Medir distancia** entre marca (nozzle) y punto donde probó (sensor)

6. **Actualizar en `printer.cfg`:**
   ```ini
   [probe_eddy_current btt_eddy]
   x_offset: 2.5    # Distancia X medida (+ si sensor está a la derecha)
   y_offset: -3.0   # Distancia Y medida (+ si sensor está atrás)
   ```

7. **Reiniciar Klipper:**
   ```gcode
   RESTART
   ```

---

## 7. Macros Útiles

Los siguientes macros están incluidos en `printer.cfg`:

### 7.1 TEST_EDDY_PROBE

Prueba rápida del estado del sensor:

```gcode
TEST_EDDY_PROBE
```

**Uso:**
- Ejecutar con nozzle en el aire → "open"
- Acercar metal → "TRIGGERED"

### 7.2 CALIBRATE_EDDY_CURRENT

Calibración completa automática:

```gcode
CALIBRATE_EDDY_CURRENT
```

**Flujo:**
1. Calibra drive current (si no está hecho)
2. Home all axes
3. Inicia calibración Z offset interactiva
4. Sigue instrucciones en pantalla

### 7.3 GENERATE_BED_MESH

Genera malla de nivelación usando Eddy Coil:

```gcode
GENERATE_BED_MESH
```

**Flujo:**
1. Home all
2. Z_TILT_ADJUST (nivela dual Z)
3. BED_MESH_CALIBRATE (25 puntos)
4. SAVE_CONFIG

⚠️ **Solo ejecutar DESPUÉS de calibrar Z offset**

---

## 8. Troubleshooting

### Problema: `LDC_CALIBRATE_DRIVE_CURRENT` falla

**Error:**
```
!! Unable to calibrate drive current
!! No response from sensor
```

**Soluciones:**

1. **Verificar conexión I2C:**
   ```gcode
   QUERY_PROBE
   ```
   Si falla → revisar cable I2C

2. **Verificar configuración:**
   - `i2c_address: 42` (o `0x2A` en hex)
   - `i2c_software_scl_pin: EBBCan:PB3`
   - `i2c_software_sda_pin: EBBCan:PB4`

3. **Re-flashear EBB42:**
   - Asegurar firmware Klipper master
   - Ver: [FLASH_EBB42_INSTRUCTIONS.md](FLASH_EBB42_INSTRUCTIONS.md)

### Problema: Z offset inconsistente

**Síntoma:** Primera capa varía entre impresiones

**Causas:**

1. **Temperatura del bed:**
   - El Eddy Coil es sensible a temperatura
   - **Solución:** Siempre calibrar con bed a temperatura de impresión (60°C)

2. **Montaje con vibración:**
   - Verificar tornillos del soporte
   - Aplicar Loctite azul

3. **Cable I2C con tensión:**
   - Dejar holgura en el cable
   - No debe tirar durante movimientos

### Problema: "probe: TRIGGERED" siempre

**Síntoma:** `QUERY_PROBE` siempre muestra triggered, incluso en el aire

**Soluciones:**

1. **Recalibrar drive current:**
   ```gcode
   LDC_CALIBRATE_DRIVE_CURRENT CHIP=btt_eddy
   SAVE_CONFIG
   ```

2. **Alejar de superficies metálicas:**
   - Home Z
   - Subir a Z50: `G1 Z50`
   - Probar `QUERY_PROBE`

3. **Verificar interferencia electromagnética:**
   - Separar cable I2C de cables de potencia 24V
   - Usar cable I2C blindado

### Problema: Range > 0.05mm en PROBE_ACCURACY

**Síntoma:** Baja repetibilidad

**Soluciones:**

1. **Verificar montaje físico:**
   - Apretar tornillos
   - Verificar que no haya holgura

2. **Reducir velocidad de probing:**
   ```ini
   [probe_eddy_current btt_eddy]
   speed: 3.0         # Reducir de 5.0 a 3.0
   ```

3. **Aumentar samples:**
   ```ini
   [probe_eddy_current btt_eddy]
   samples: 3         # Aumentar de 2 a 3
   ```

### Problema: Z hace "doble tap" durante homing

**Síntoma:** Z baja, sube, vuelve a bajar (normal en probes)

**Explicación:**
- Primera bajada: detección rápida (`homing_speed: 10`)
- Subida: retracción (`homing_retract_dist: 3`)
- Segunda bajada: detección precisa (`second_homing_speed: 3`)

✅ **Esto es normal y deseable** para precisión

---

## 9. Mantenimiento

### Mantenimiento regular

**Cada 100 horas de impresión:**
- Limpiar bobina del Eddy Coil con aire comprimido
- Verificar tornillos del soporte (reapretar si necesario)
- Ejecutar `PROBE_ACCURACY` y verificar range < 0.01mm

**Después de cambiar nozzle:**
- Recalibrar Z offset: `CALIBRATE_EDDY_CURRENT`
- Verificar X/Y offsets (solo si cambia posición física)

**Después de cambiar bed surface:**
- Recalibrar Z offset
- Regenerar bed mesh: `GENERATE_BED_MESH`

### Recalibración programada

**Cuándo recalibrar Z offset:**
- ✅ Cambio de nozzle
- ✅ Cambio de bed/superficie
- ✅ Ajuste altura del sensor
- ✅ Primera capa inconsistente

**Cuándo recalibrar drive current:**
- ✅ Reemplazo del Eddy Coil
- ✅ Cambio de placa EBB42
- ❌ **Nunca** para ajustes rutinarios

### Backup de configuración

Después de cada calibración exitosa:

1. **Guardar `printer.cfg`:**
   ```bash
   cp ~/printer_data/config/printer.cfg ~/printer_data/config/printer.cfg.backup
   ```

2. **Commit a Git:**
   ```bash
   git add klipper/config/printer.cfg
   git commit -m "chore: update Eddy Coil calibration (z_offset)"
   git push
   ```

---

## Referencias

### Comandos Klipper

| Comando | Descripción | Frecuencia |
|---------|-------------|------------|
| `LDC_CALIBRATE_DRIVE_CURRENT` | Calibrar corriente del sensor | 1 vez (instalación) |
| `PROBE_EDDY_CURRENT_CALIBRATE` | Calibrar Z offset | Después de cambios |
| `QUERY_PROBE` | Verificar estado del sensor | Testing |
| `PROBE_ACCURACY` | Test de precisión | Verificación |
| `CALIBRATE_EDDY_CURRENT` | Macro todo-en-uno | Recomendado |

### Documentación relacionada

- [EDDY_COIL_INSTALLATION.md](EDDY_COIL_INSTALLATION.md) - Instalación física
- [EBB42_INTEGRATION.md](EBB42_INTEGRATION.md) - Integración EBB42
- [Klipper Eddy Probe Docs](https://www.klipper3d.org/Eddy_Probe.html)
- [BTT Eddy GitHub](https://github.com/bigtreetech/Eddy)

---

## Próximos pasos

✅ **Calibración completa**
➡️ **Siguiente:**
- Homing completo con `G28`
- Nivelación dual Z con `Z_TILT_ADJUST`
- Bed mesh con `GENERATE_BED_MESH`
- Primera impresión de prueba

---

**Versión:** 1.0
**Fecha:** 2025-12-26
**Autor:** Documentación generada durante Phase 3 - Tronxy X5SA Migration
