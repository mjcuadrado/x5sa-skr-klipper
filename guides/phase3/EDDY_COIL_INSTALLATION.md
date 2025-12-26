# Instalación BIGTREETECH Eddy Coil V1.0

**Fecha:** 2025-12-26
**Hardware:** BIGTREETECH Eddy Coil V1.0 (LDC1612)
**Conexión:** I2C a EBB42 CAN V1.2
**Versión Guía:** 1.0

---

## Índice

1. [Descripción del Hardware](#1-descripción-del-hardware)
2. [Requisitos Previos](#2-requisitos-previos)
3. [Materiales Necesarios](#3-materiales-necesarios)
4. [Montaje Físico](#4-montaje-físico)
5. [Conexión I2C](#5-conexión-i2c)
6. [Verificación de Hardware](#6-verificación-de-hardware)
7. [Troubleshooting](#7-troubleshooting)
8. [Referencias](#8-referencias)

---

## 1. Descripción del Hardware

### ¿Qué es el Eddy Coil?

El **BIGTREETECH Eddy Coil V1.0** es un sensor de proximidad basado en **corrientes de Eddy** (eddy current) que utiliza el chip **LDC1612** de Texas Instruments. A diferencia de los sensores inductivos tradicionales, el Eddy Coil:

- **No requiere alimentación de alto voltaje** (24V)
- **Comunicación I2C** directa con la placa toolhead
- **Mayor precisión** y repetibilidad
- **Sin circuitos de adaptación** de voltaje
- **Calibración automática** mediante Klipper

### Ventajas sobre sensores tradicionales

| Característica | Tronxy XY-08N (NPN) | Eddy Coil V1.0 |
|----------------|---------------------|----------------|
| Alimentación | 6-36V (24V típico) | 3.3V/5V (del toolboard) |
| Señal | Digital ON/OFF | Analógica (I2C) |
| Precisión | ±0.1mm | ±0.01mm |
| Calibración | Manual (resistencias) | Automática (Klipper) |
| Cableado | 3 cables (VCC/GND/Signal) | 4 cables I2C |
| Compatibilidad | Requiere adaptación 24V→3.3V | Directa a EBB42 |

### Limitaciones conocidas

⚠️ **IMPORTANTE:**
- **No tiene compensación de temperatura** (a diferencia del Eddy USB con MCU propio)
- En impresoras con cámara cerrada, la temperatura puede afectar lecturas
- Para esta instalación (Tronxy X5SA abierta): **No es problema**

---

## 2. Requisitos Previos

### Hardware instalado

Antes de instalar el Eddy Coil, debes tener:

- ✅ **EBB42 CAN V1.2** instalado y funcionando en toolhead
- ✅ **Klipper firmware** actualizado en EBB42 (rama `master`)
- ✅ **Comunicación USB** entre SKR 1.4 Turbo y EBB42 verificada
- ✅ **Heater y fans** funcionando en EBB42
- ✅ **Sistema básico operativo** (homing XY, temperatura, etc.)

### Software

- Klipper versión **master** (o posterior a 2024-06)
- Acceso SSH a la Raspberry Pi / PC con Klipper
- Mainsail o Fluidd para enviar comandos GCode

### Estado del proyecto

Este documento asume que has completado:
- ✅ **Phase 0:** Baseline y auditoría
- ✅ **Phase 1:** Preparación SKR 1.4 Turbo
- ✅ **Phase 2:** Migración a SKR 1.4 Turbo
- ✅ **Phase 3:** Instalación EBB42 básica (heater, fans, thermistor)

---

## 3. Materiales Necesarios

### Componentes principales

| Componente | Cantidad | Fuente | Notas |
|------------|----------|--------|-------|
| BIGTREETECH Eddy Coil V1.0 | 1 | BTT oficial | Incluye cable I2C |
| Cable I2C 4 pines | 1 | Incluido con Eddy | JST-XH 2.54mm típico |
| Soporte de montaje | 1 | Impresión 3D (opcional) | Debe permitir ajuste de altura |

### Herramientas

- Destornillador (según tu toolhead)
- Cinta métrica o calibre digital
- Multímetro (opcional, para verificar I2C)

### Materiales opcionales

- Bridas (zip ties) para cable management
- Cinta Kapton para fijar cable temporalmente
- Loctite para tornillos (evitar aflojamiento con vibración)

---

## 4. Montaje Físico

### 4.1 Posición del sensor

**Regla crítica:**
El Eddy Coil debe montarse **2-3mm por encima de la punta del nozzle**.

```
Vista lateral del toolhead:

          [Eddy Coil]
               ║
    ╔══════════╩══════════╗
    ║    Toolhead         ║
    ║                     ║
    ╚═════════╤═══════════╝
              │
         ┌────┴────┐
         │ Nozzle  │ ← 2-3mm DEBAJO del Eddy Coil
         └─────────┘
```

### 4.2 Opciones de montaje

#### Opción A: Montaje directo (temporal)

Si no tienes soporte impreso, puedes:

1. Usar cinta Kapton para fijar temporalmente el Eddy Coil al lado del toolhead
2. Asegurar con bridas
3. **Verificar altura con calibre:** nozzle debe estar 2-3mm más bajo que la bobina

⚠️ **Advertencia:** Este método es temporal. Diseña e imprime un soporte adecuado.

#### Opción B: Soporte impreso (recomendado)

Para Tronxy X5SA con toolhead stock:

1. **STL disponible:** `stls/phase3/eddy_coil_mount.stl` (si existe)
2. **Si no existe:** Diseña uno usando estos parámetros:
   - Distancia nozzle-coil: 2-3mm
   - Orientación: Bobina paralela al bed
   - Fijación: M3 o tornillos del toolhead existente
   - Clearance: No debe chocar con fans o conductos

3. **Imprime con:**
   - Material: PETG o ABS (mejor resistencia térmica que PLA)
   - Infill: 30-50%
   - Perímetros: 3-4

#### Opción C: Integración Stealthburner (Phase 12)

⏸️ **Futuro:** En Phase 12 migraremos a Stealthburner con Orbiter v2.
El Eddy Coil se integrará en el soporte Stealthburner oficial.

### 4.3 Procedimiento de instalación física

1. **Apagar la impresora completamente** (desconectar alimentación)

2. **Posicionar el Eddy Coil:**
   - Colocar bobina 2-3mm sobre el nozzle
   - Verificar con calibre o regla
   - Marcar posición de tornillos

3. **Fijar el soporte:**
   - Atornillar con M3 (o según diseño)
   - **No aplicar Loctite todavía** (primero verificar altura en calibración)

4. **Verificar clearance:**
   - Mover toolhead manualmente en XY
   - Verificar que NO choque con:
     - Bed
     - Frame
     - Cable chains
     - Fans

5. **Cable routing:**
   - Dejar cable I2C sin tensión
   - Usar bridas para guiar cable hasta EBB42
   - **No tensar:** debe permitir movimiento completo de XY

---

## 5. Conexión I2C

### 5.1 Identificar conector I2C en EBB42

En la **EBB42 CAN V1.2**, el conector I2C es:

- **Ubicación:** Cerca del puerto USB-C
- **Tipo:** JST-XH 2.54mm de 4 pines
- **Etiqueta:** "I2C" en la PCB

**Pinout del conector I2C en EBB42:**

```
Pin 1 (GND)  ━━━┓
Pin 2 (VCC)      ┃
Pin 3 (SDA) PB4  ┃  Conector I2C
Pin 4 (SCL) PB3  ┃
                 ┻
```

| Pin | Señal | Color Cable | Función | Pin MCU |
|-----|-------|-------------|---------|---------|
| 1 | GND | Negro | Tierra | GND |
| 2 | VCC | Rojo | Alimentación 3.3V o 5V | VCC |
| 3 | SDA | Blanco/Amarillo | I2C Data | PB4 |
| 4 | SCL | Verde/Azul | I2C Clock | PB3 |

⚠️ **VERIFICAR:** Los colores pueden variar según el fabricante del cable. **Confirma con el pinout del Eddy Coil.**

### 5.2 Conectar el cable I2C

1. **Verificar polaridad:**
   - Cable del Eddy Coil debe tener 4 conductores
   - Verificar pinout en documentación BTT
   - **NO invertir VCC/GND** (dañará el sensor)

2. **Insertar en EBB42:**
   - Alinear conector JST con el puerto I2C
   - Presionar firmemente hasta escuchar "click"
   - Verificar que quede bien asentado

3. **Conectar al Eddy Coil:**
   - El otro extremo va al conector del Eddy Coil
   - Verificar que encaje completamente

### 5.3 Verificación eléctrica (opcional)

Si tienes multímetro, puedes verificar:

1. **Con impresora apagada:**
   - Continuidad GND: Pin 1 del I2C → GND del EBB42
   - **NO debe haber continuidad VCC→GND** (sería cortocircuito)

2. **Con impresora encendida:**
   - Voltaje VCC: Pin 2 del I2C → debe leer 3.3V o 5V
   - **Si lee 0V:** revisar conexión
   - **Si lee >5.5V:** ¡APAGAR! Hay problema de alimentación

---

## 6. Verificación de Hardware

### 6.1 Encender el sistema

1. Conectar alimentación 24V a la impresora
2. Esperar a que Klipper inicie
3. Abrir Mainsail/Fluidd

### 6.2 Verificar configuración de Klipper

El archivo `printer.cfg` debe contener:

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
z_offset: 1.0
# ... resto de parámetros
```

Si NO está configurado, consulta: **[EDDY_COIL_CALIBRATION.md](EDDY_COIL_CALIBRATION.md)**

### 6.3 Test de comunicación I2C

Ejecuta en la consola de Klipper:

```gcode
QUERY_PROBE
```

**Resultado esperado:**
```
probe: open
```

**Si aparece error:**
```
!! Unable to reach btt_eddy
!! I2C communication error
```

→ Ver sección [Troubleshooting](#7-troubleshooting)

### 6.4 Test de detección de metal

1. Ejecuta: `QUERY_PROBE`
   - Debe mostrar: `probe: open`

2. Acerca un objeto metálico (destornillador, cama) a la bobina del Eddy Coil

3. Ejecuta nuevamente: `QUERY_PROBE`
   - Debe cambiar a: `probe: TRIGGERED`

4. Aleja el metal y ejecuta: `QUERY_PROBE`
   - Debe volver a: `probe: open`

✅ **Si funciona:** Instalación física correcta, pasar a calibración
❌ **Si NO funciona:** Ver [Troubleshooting](#7-troubleshooting)

---

## 7. Troubleshooting

### Problema: "Unable to reach btt_eddy" o "I2C communication error"

**Causas posibles:**

1. **Cable I2C mal conectado:**
   - ✅ Verificar que el conector JST esté bien insertado en EBB42
   - ✅ Verificar conexión en el Eddy Coil
   - ✅ Revisar que no haya pines doblados

2. **Dirección I2C incorrecta:**
   - Por defecto: `i2c_address: 42` (decimal)
   - Algunos sensores usan: `i2c_address: 0x2A` (hexadecimal, equivalente)
   - Prueba cambiar en `printer.cfg` y ejecutar `RESTART`

3. **Pines I2C incorrectos:**
   - Verificar: `i2c_software_scl_pin: EBBCan:PB3`
   - Verificar: `i2c_software_sda_pin: EBBCan:PB4`
   - Si tienes EBB42 V1.0 o V1.1: los pines pueden ser diferentes

4. **Firmware EBB42 desactualizado:**
   - Verificar que EBB42 esté con firmware Klipper **master** (no CAN)
   - Re-flashear si es necesario

5. **Cable I2C defectuoso:**
   - Probar otro cable I2C
   - Verificar continuidad con multímetro

### Problema: `QUERY_PROBE` siempre muestra "triggered"

**Causas posibles:**

1. **Sensor demasiado cerca de superficie metálica:**
   - Alejar el toolhead del bed
   - Ejecutar `QUERY_PROBE` en el aire
   - Si ahora muestra "open" → sensor funciona, ajustar altura

2. **Calibración de drive current necesaria:**
   - Ejecutar: `LDC_CALIBRATE_DRIVE_CURRENT CHIP=btt_eddy`
   - Ver guía: [EDDY_COIL_CALIBRATION.md](EDDY_COIL_CALIBRATION.md)

3. **Interferencia electromagnética:**
   - Alejar cables de alimentación 24V del cable I2C
   - Usar cable I2C blindado (shielded)

### Problema: `QUERY_PROBE` siempre muestra "open"

**Causas posibles:**

1. **Sensor defectuoso:**
   - Verificar voltaje VCC en cable I2C: debe ser 3.3V o 5V
   - Si es 0V → problema de alimentación EBB42

2. **Bobina dañada:**
   - Inspeccionar físicamente el Eddy Coil
   - Buscar grietas o daños

3. **Configuración incorrecta:**
   - Verificar `sensor_type: ldc1612` (no otro tipo)
   - Verificar dirección I2C

### Problema: Lecturas inconsistentes

**Causas posibles:**

1. **Montaje con vibración:**
   - Apretar tornillos del soporte
   - Aplicar Loctite (azul) en tornillos

2. **Cable I2C con tensión:**
   - Dejar más holgura en el cable
   - Asegurar con bridas sin tensar

3. **Temperatura variable:**
   - El Eddy Coil es sensible a temperatura
   - Esperar a que la impresora se estabilice térmicamente antes de calibrar

---

## 8. Referencias

### Documentación oficial

- [Klipper Eddy Probe Documentation](https://www.klipper3d.org/Eddy_Probe.html)
- [BIGTREETECH Eddy GitHub](https://github.com/bigtreetech/Eddy)
- [BIGTREETECH Eddy Wiki](https://bttwiki.com/Eddy.html)
- [EBB42 CAN Documentation](https://github.com/bigtreetech/EBB)

### Guías del proyecto

- [EDDY_COIL_CALIBRATION.md](EDDY_COIL_CALIBRATION.md) - Calibración y uso
- [EBB42_INTEGRATION.md](EBB42_INTEGRATION.md) - Integración completa EBB42
- [INSTALACION_COMPLETADA.md](INSTALACION_COMPLETADA.md) - Estado actual del sistema

### Issues conocidos

- [Eddy Coil V1 EBB42 CAN - Issue #68](https://github.com/bigtreetech/Eddy/issues/68)
- [BTT Eddy Guide - Community](https://github.com/krautech/btt-eddy-guide)

---

## Próximos pasos

✅ **Instalación física completa**
➡️ **Siguiente:** [EDDY_COIL_CALIBRATION.md](EDDY_COIL_CALIBRATION.md) - Calibración del sensor

---

**Versión:** 1.0
**Fecha:** 2025-12-26
**Autor:** Documentación generada durante Phase 3 - Tronxy X5SA Migration
