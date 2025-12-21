# EBB42 Integration - Decisiones y Configuración

**Fecha:** 2025-12-21
**Estado:** ✅ Decisiones tomadas, listo para implementación

---

## 📋 Índice

1. [Resumen de Decisiones](#resumen-de-decisiones)
2. [Pinout Completo EBB42](#pinout-completo-ebb42)
3. [Plan de Cableado](#plan-de-cableado)
4. [Materiales Necesarios](#materiales-necesarios)
5. [Configuración Klipper](#configuración-klipper)
6. [Plan de Implementación](#plan-de-implementación)

---

## Resumen de Decisiones

### ✅ Decisión 1: Conexión USB (NO CAN)

**Selección:** USB desde SKR

**Configuración:**
- Cable USB-C (EBB42) a USB-A/C (SKR)
- Con anillos ferrita para reducir EMI
- Por cable chain existente

**Justificación:**
- Simplicidad de configuración (no requiere transceiver CAN)
- Hardware disponible (stock de SKR)
- Menor complejidad de troubleshooting
- Apropiado para Phase 3-12 (temporal antes de Stealthburner)

---

### ✅ Decisión 2: Ubicación Física EBB42

**Selección:** Toolhead (NO en electronics box)

**Montaje:**
- **Phase 3-11:** Provisional con cinta doble cara + bridas
- **Phase 12:** Definitivo en Stealthburner

**Justificación:**
- Minimiza longitud de cables críticos (hotend, thermistor, probe)
- Reduce EMI en señales analógicas
- Configuración estándar para toolhead boards
- Evita re-cableado en Phase 12

---

### ✅ Decisión 3: Alimentación 24V

**Parte 1 - Cable 24V:**
- Cable dedicado 24V al toolhead
- Mismo tipo que cable alimentación SKR (termorretráctil rojo/azul)
- Va por cable chain junto con USB

**Parte 2 - Fuente 24V:**
- Desde salida **FAN2** o **HE1** de la SKR
- Configurado en Klipper como always-on (100%)
- Capacidad sobrada: ~1-2A disponible, ~0.6A necesario

**Consumo estimado:**
- EBB42: ~0.2A
- Hotend fan (4010): ~0.1-0.15A
- Part cooling fan (5015): ~0.15-0.25A
- **Total: ~0.5-0.6A**

---

### ✅ Decisión 4: Conectores

**Enfoque mixto según criticidad:**

| Componente | Conector | Razón |
|------------|----------|-------|
| 24V power | JST-XH 2-pin | Crítico - no queremos desconexiones |
| Hotend heater | JST-XH 2-pin | Seguridad térmica |
| Thermistor | JST-XH 2-pin | Señal crítica |
| Probe Omron | JST-XH 3-pin | Señal crítica (NO/C/NC) |
| Fan part cooling | Dupont 2-pin + hot glue | Re-conectable, menos crítico |
| Fan hotend | Dupont 2-pin + hot glue | Re-conectable, menos crítico |

**Seguridad adicional:**
- Hot glue en conectores Dupont para evitar desconexiones por vibración
- Todos los conectores accesibles para desmontaje en Phase 12

---

### ✅ Decisión 5: Configuración Ventiladores

**Asignación de puertos (estándar):**

- **FAN0 (EBB42)** → Part cooling fan
  - Controlado por Klipper (PWM)
  - Se activa según necesidad de capa
  - `[fan]` en configuración

- **FAN1 (EBB42)** → Hotend fan
  - Always-on cuando T > 50°C
  - `[heater_fan hotend_fan]` en configuración
  - Protección térmica del disipador

---

### ✅ Decisión 6: Probe Omron (Fail-Safe)

**Configuración NC (Normally Closed):**

```
Omron NC  → EBB42 PROBE Signal (PB9)
Omron C   → EBB42 GND
Omron NO  → (sin conectar)
```

**Comportamiento fail-safe:**
- ✅ Probe OK: Bed leveling funcional
- ❌ Cable suelto: Error inmediato, NO imprime
- ❌ Sensor roto: Error inmediato, NO imprime
- 🛡️ **Garantía: No hay impresión sin bed leveling funcional**

**Configuración Klipper:**
```ini
[probe]
pin: ^EBBCan:PB9  # Pullup interno activado
```

---

### ⚠️ Decisión 7: Motor Extrusor (E)

**Phase 3-11:**
- Motor E se queda en **SKR puerto E0** (posición lateral actual)
- NO se migra a EBB42 en esta fase
- Cableado actual se mantiene

**Phase 12 (Orbiter v2):**
- Motor E integrado en toolhead → **migra a EBB42**
- Cable motor E se añade a cable chain
- Re-cableado completo con Stealthburner

**Componentes en EBB42 Phase 3:**
- ✅ Hotend (heater + thermistor)
- ✅ Ventiladores (part cooling + hotend fan)
- ✅ Probe Omron
- ❌ Motor E (se queda en SKR)

---

## Pinout Completo EBB42

### Conexiones Phase 3

```
EBB42 CAN V1.2
┌─────────────────────────────────────┐
│                                     │
│  POWER                              │
│  ├─ VIN (24V)  ← Cable 24V desde SKR│
│  └─ GND        ← Cable 24V GND      │
│                                     │
│  USB                                │
│  └─ USB-C      ← USB a SKR          │
│                                     │
│  HOTEND                             │
│  ├─ HE+        ← Heater +           │
│  ├─ HE-        ← Heater -           │
│  ├─ TH0+       ← Thermistor +       │
│  └─ TH0-       ← Thermistor -       │
│                                     │
│  FANS                               │
│  ├─ FAN0 (+/-) ← Part cooling       │
│  └─ FAN1 (+/-) ← Hotend fan         │
│                                     │
│  PROBE                              │
│  ├─ PB9 (SIG)  ← Omron NC           │
│  └─ GND        ← Omron C            │
│                                     │
│  MOTOR E (sin usar en Phase 3)      │
│  └─ E0         ← (vacío)            │
│                                     │
└─────────────────────────────────────┘
```

### Conexiones SKR 1.4 Turbo (modificaciones)

```
SKR 1.4 TURBO
┌─────────────────────────────────────┐
│                                     │
│  USB                                │
│  └─ USB-A/C    → Cable a EBB42      │
│                                     │
│  POWER OUT (para EBB42)             │
│  └─ FAN2/HE1   → 24V a EBB42        │
│     (always-on)                     │
│                                     │
│  MOTOR E                            │
│  └─ E0         ← Motor extrusor     │
│     (se queda hasta Phase 12)       │
│                                     │
└─────────────────────────────────────┘
```

---

## Plan de Cableado

### Cable 1: USB (SKR → EBB42)

**Especificaciones:**
- Tipo: USB-C a USB-A/C
- Longitud: Según cable chain (~1.5-2m)
- Accesorios: 2x anillos ferrita
- Ruta: Electronics box → cable chain → toolhead

**Instalación:**
1. Medir longitud necesaria con holgura para movimientos
2. Instalar anillos ferrita cerca de cada extremo
3. Pasar por cable chain existente
4. Conectar a SKR (extremo fijo)
5. Conectar a EBB42 (extremo móvil)

---

### Cable 2: 24V Power (SKR → EBB42)

**Especificaciones:**
- Tipo: 2 conductores 1.5mm² (mismo que alimentación SKR)
- Longitud: ~1.5-2m (según cable chain)
- Identificación: Termorretráctil rojo (+24V) y azul (GND)
- Conectores:
  - SKR: Terminales tornillo FAN2/HE1
  - EBB42: JST-XH 2-pin (o tornillo)

**Fabricación:**
1. Cortar cable según longitud medida
2. Pelar extremos (~5mm)
3. Aplicar termorretráctil rojo en conductor +24V
4. Aplicar termorretráctil azul en conductor GND
5. Crimpar/soldar conector JST-XH para extremo EBB42
6. Verificar continuidad y no cortocircuito

**Instalación:**
1. Conectar a FAN2/HE1 de SKR (extremo fijo)
2. Pasar por cable chain con USB
3. Conectar a VIN de EBB42 (extremo móvil)
4. Verificar polaridad con multímetro ANTES de energizar

---

### Cable 3: Hotend Heater

**Especificaciones:**
- Conectores: JST-XH 2-pin en ambos extremos
- Cable: 2 conductores calibre apropiado (actual stock)
- Longitud: Stock o extendido según necesidad

**Migración desde stock:**
1. Desconectar de placa stock
2. Verificar continuidad del heater (multímetro)
3. Adaptar/crimpar conector JST-XH si necesario
4. Conectar a EBB42 HE+/HE-
5. **CRÍTICO:** Verificar resistencia heater (~12-20Ω para 24V/40W)

---

### Cable 4: Thermistor

**Especificaciones:**
- Conectores: JST-XH 2-pin
- Cable: 2 conductores señal (delgado, stock)
- Tipo: NTC 100K (típico stock)

**Migración desde stock:**
1. Desconectar de placa stock
2. Medir resistencia a temperatura ambiente (~100kΩ @ 25°C)
3. Adaptar/crimpar conector JST-XH
4. Conectar a EBB42 TH0+/TH0-
5. Verificar lectura temperatura en Klipper

---

### Cable 5: Part Cooling Fan

**Especificaciones:**
- Conectores: Dupont 2-pin + hot glue
- Cable: 2 conductores (stock)
- Voltaje: 24V DC

**Instalación:**
1. Identificar fan actual (4010 o 5015)
2. Adaptar conector Dupont
3. Conectar a EBB42 FAN0
4. Aplicar punto de hot glue para seguridad
5. Test: `SET_FAN_SPEED FAN=fan SPEED=1.0`

---

### Cable 6: Hotend Fan

**Especificaciones:**
- Conectores: Dupont 2-pin + hot glue
- Cable: 2 conductores (stock)
- Voltaje: 24V DC
- Tipo: 4010 típico

**Instalación:**
1. Identificar fan hotend actual
2. Adaptar conector Dupont
3. Conectar a EBB42 FAN1
4. Aplicar punto de hot glue
5. Configurar como heater_fan (auto cuando T>50°C)

---

### Cable 7: Probe Omron (NC fail-safe)

**Especificaciones:**
- Conectores: JST-XH 3-pin (NC, C, NO)
- Solo usar NC y C (NO sin conectar)
- Cable: 3 conductores (stock)

**Instalación:**
1. Identificar cables Omron: NC (marrón), C (azul), NO (negro) - verificar con multímetro
2. Conectar:
   - NC → EBB42 PB9 (Signal)
   - C → EBB42 GND
   - NO → sin conectar
3. Test con multímetro:
   - Sin trigger: Continuidad NC-C (cerrado)
   - Con trigger: No continuidad NC-C (abierto)
4. Configurar en Klipper con pullup: `^EBBCan:PB9`

---

## Materiales Necesarios

### ✅ Hardware Principal

- [x] BTT EBB42 CAN V1.2
- [x] Sensor Omron TL-Q5MC1-Z (instalado)
- [x] Hotend + thermistor stock
- [x] 2x ventiladores stock (hotend + part cooling)

### 🔧 Cables y Conectores

**Para fabricar:**
- [ ] Cable USB-C a USB-C (o USB-A) longitud ~1.5-2m
- [ ] Cable 2x1.5mm² para 24V (~1.5-2m)
- [ ] 2x anillos ferrita

**Conectores:**
- [ ] JST-XH 2-pin macho/hembra (x4 sets: 24V, heater, thermistor, fans)
- [ ] JST-XH 3-pin macho/hembra (x1 set: probe Omron)
- [ ] Dupont 2-pin macho/hembra (x2 sets: fans)
- [ ] Pines JST-XH y Dupont (extras)

**Accesorios:**
- [ ] Termorretráctil rojo/azul (varios tamaños)
- [ ] Hot glue / pistola de silicona
- [ ] Bridas (zip ties) de varios tamaños
- [ ] Cinta doble cara resistente (montaje temporal EBB42)
- [ ] Velcro adhesivo (opcional, gestión cables)

### 🛠️ Herramientas

- [ ] Multímetro digital
- [ ] Crimpadora JST-XH y Dupont
- [ ] Pelacables / wire strippers
- [ ] Destornilladores: Phillips, plano, Allen
- [ ] Tijeras / cutter
- [ ] Pistola calor (termorretráctil)
- [ ] Soldador + estaño (opcional, backup)

---

## Configuración Klipper

### Archivo: `printer.cfg` - Sección EBB42

```ini
#####################################################################
# EBB42 CAN Toolhead Board
#####################################################################

[mcu EBBCan]
# USB connection to EBB42
serial: /dev/serial/by-id/usb-Klipper_stm32g0b1xx_XXXXXXXX-if00
# (ID se obtiene con: ls /dev/serial/by-id/)

#####################################################################
# Hotend
#####################################################################

[extruder]
# NOTE: Motor E está en SKR E0 (Phase 3-11)
# Migra a EBB42 en Phase 12
step_pin: PB4        # SKR E0 step
dir_pin: PB3         # SKR E0 dir
enable_pin: !PD2     # SKR E0 enable
microsteps: 16
rotation_distance: 33.5  # Ajustar según extrusor
nozzle_diameter: 0.400
filament_diameter: 1.750
# Heater en EBB42
heater_pin: EBBCan:PA2  # EBB42 HE0
# Thermistor en EBB42
sensor_type: NTC 100K MGB18-104F39050L32  # Ajustar según stock
sensor_pin: EBBCan:PA3  # EBB42 TH0
control: pid
pid_Kp: 22.2   # Calibrar con PID_CALIBRATE
pid_Ki: 1.08
pid_Kd: 114
min_temp: 0
max_temp: 300
max_extrude_only_distance: 150.0

#####################################################################
# Fans
#####################################################################

[fan]
# Part cooling fan - FAN0
pin: EBBCan:PA0
kick_start_time: 0.5
off_below: 0.10

[heater_fan hotend_fan]
# Hotend fan - FAN1
pin: EBBCan:PA1
max_power: 1.0
kick_start_time: 0.5
heater: extruder
heater_temp: 50.0
fan_speed: 1.0

#####################################################################
# Probe - Omron NC (fail-safe)
#####################################################################

[probe]
pin: ^EBBCan:PB9  # NC configuration with pullup
x_offset: 0       # Ajustar según montaje físico
y_offset: 25.0    # Ajustar según montaje físico
z_offset: 0       # Calibrar con PROBE_CALIBRATE
speed: 5.0
samples: 3
samples_result: median
sample_retract_dist: 2.0
samples_tolerance: 0.01
samples_tolerance_retries: 3

#####################################################################
# Bed Leveling (requiere probe funcional)
#####################################################################

[bed_mesh]
speed: 120
horizontal_move_z: 5
mesh_min: 30, 30      # Ajustar según área imprimible
mesh_max: 270, 270    # Ajustar según área imprimible
probe_count: 5, 5
algorithm: bicubic
fade_start: 1
fade_end: 10
fade_target: 0

[safe_z_home]
home_xy_position: 150, 150  # Centro de cama (ajustar)
speed: 50
z_hop: 10
z_hop_speed: 5

#####################################################################
# SKR - 24V Power para EBB42 (always-on)
#####################################################################

[output_pin ebb42_power]
# FAN2 o HE1 usado como fuente 24V fija
pin: PB11  # Ejemplo: FAN2 de SKR (verificar pinout)
pwm: False
value: 1   # Always ON
shutdown_value: 0

```

### Comandos de Verificación

```bash
# 1. Verificar comunicación con EBB42
ls /dev/serial/by-id/
# Debe aparecer: usb-Klipper_stm32g0b1xx_XXXXXXXX-if00

# 2. Test hotend heater (CUIDADO - supervisar)
# En consola Klipper:
SET_HEATER_TEMPERATURE HEATER=extruder TARGET=50
# Verificar que temperatura sube
# Apagar:
SET_HEATER_TEMPERATURE HEATER=extruder TARGET=0

# 3. Test fans
SET_FAN_SPEED FAN=fan SPEED=1.0        # Part cooling 100%
SET_FAN_SPEED FAN=fan SPEED=0          # Part cooling OFF
# Hotend fan: se activa automáticamente cuando extruder > 50°C

# 4. Test probe
QUERY_PROBE
# Debe responder: "probe: open" o "probe: triggered"
# Trigger manual y verificar cambio de estado

# 5. Probe calibration
PROBE_CALIBRATE
# Seguir instrucciones en pantalla

# 6. Bed mesh
BED_MESH_CALIBRATE
# Ejecuta sondeo automático de la cama
```

---

## Plan de Implementación

### Fase 1: Preparación (30-45 min)

- [ ] Leer completamente este documento
- [ ] Verificar todos los materiales disponibles
- [ ] Preparar área de trabajo limpia
- [ ] Descargar/compilar firmware Klipper para EBB42
- [ ] Flashear EBB42 con firmware Klipper
- [ ] Verificar ID USB de EBB42: `ls /dev/serial/by-id/`

**Documentación:**
- [ ] Fotografiar toolhead stock ANTES de desmontar (10+ fotos)
- [ ] Identificar todos los cables actuales con etiquetas

---

### Fase 2: Fabricación Cables (1-1.5h)

- [ ] Fabricar cable 24V:
  - Medir longitud según cable chain
  - Cortar y pelar extremos
  - Aplicar termorretráctil rojo/azul
  - Crimpar JST-XH en extremo EBB42
  - Verificar continuidad y no cortocircuito

- [ ] Preparar cable USB:
  - Verificar longitud adecuada
  - Instalar anillos ferrita
  - Test de continuidad

- [ ] Adaptar cables componentes stock:
  - Crimpar JST-XH en hotend heater
  - Crimpar JST-XH en thermistor
  - Crimpar JST-XH en probe Omron
  - Crimpar Dupont en ventiladores

**Verificaciones:**
- [ ] Todos los cables etiquetados claramente
- [ ] Polaridades verificadas
- [ ] Continuidad verificada (multímetro)

---

### Fase 3: Montaje EBB42 en Toolhead (30-60 min)

- [ ] Limpiar área de montaje en toolhead
- [ ] Posicionar EBB42 (acceso a conectores, alejada de hotend)
- [ ] Montaje temporal:
  - Cinta doble cara resistente
  - Bridas como seguridad adicional
- [ ] Verificar que no interfiere con movimientos
- [ ] Verificar acceso a conectores

**Notas:**
- Montaje definitivo en Phase 12 (Stealthburner)
- Priorizar accesibilidad sobre estética

---

### Fase 4: Migración Componentes (1.5-2h)

**IMPORTANTE: Impresora apagada durante todo el proceso**

#### 4.1 Desconexión Stock
- [ ] Fotografiar conexiones stock
- [ ] Etiquetar todos los cables ANTES de desconectar
- [ ] Desconectar de placa stock:
  - Hotend heater
  - Thermistor
  - Part cooling fan
  - Hotend fan
  - Probe Omron

#### 4.2 Conexión a EBB42
- [ ] **24V Power:**
  - Conectar a VIN/GND de EBB42
  - VERIFICAR POLARIDAD con multímetro

- [ ] **Hotend Heater:**
  - Verificar resistencia heater (~12-20Ω)
  - Conectar a HE+/HE-
  - Verificar que no hay cortocircuito a GND

- [ ] **Thermistor:**
  - Medir resistencia (~100kΩ @ 25°C)
  - Conectar a TH0+/TH0-
  - Verificar que no toca partes metálicas

- [ ] **Part Cooling Fan (FAN0):**
  - Conectar con Dupont
  - Aplicar hot glue
  - Verificar polaridad

- [ ] **Hotend Fan (FAN1):**
  - Conectar con Dupont
  - Aplicar hot glue
  - Verificar polaridad

- [ ] **Probe Omron:**
  - Conectar NC → PB9 (Signal)
  - Conectar C → GND
  - NO conectar NO
  - Test con multímetro (continuidad NC-C cuando no triggered)

---

### Fase 5: Cableado SKR y Cable Chain (45-60 min)

#### 5.1 SKR Connections
- [ ] Conectar cable 24V a FAN2/HE1:
  - Rojo a +24V
  - Azul a GND
  - Apretar tornillos firmemente

- [ ] Conectar cable USB a puerto SKR

#### 5.2 Cable Chain
- [ ] Abrir cable chain
- [ ] Pasar cables (sin tensión):
  - USB
  - 24V power
  - (Cables componentes ya conectados a EBB42)
- [ ] Dejar holgura adecuada ambos extremos
- [ ] Cerrar cable chain
- [ ] Test movimiento completo toolhead (X, Y, Z)
- [ ] Verificar que ningún cable tiene tensión excesiva

---

### Fase 6: Configuración Klipper (30-45 min)

- [ ] Copiar configuración EBB42 a `printer.cfg`
- [ ] Actualizar `[mcu EBBCan]` con ID USB correcto
- [ ] Configurar `[output_pin ebb42_power]` (FAN2/HE1 always-on)
- [ ] Verificar pinout según hardware real
- [ ] Guardar backup de configuración anterior
- [ ] Restart Klipper: `RESTART`

**Verificar en consola:**
```
MCU 'EBBCan' connected
```

---

### Fase 7: Testing y Validación (45-60 min)

#### 7.1 Test Comunicación
- [ ] `ls /dev/serial/by-id/` muestra EBB42
- [ ] Klipper conecta sin errores
- [ ] No hay mensajes de error en log

#### 7.2 Test 24V Power
- [ ] Medir voltaje VIN de EBB42 con multímetro: ~24V DC
- [ ] LED de EBB42 encendido

#### 7.3 Test Thermistor
- [ ] Verificar lectura temperatura ambiente razonable (~20-25°C)
- [ ] No hay errores "ADC out of range"

#### 7.4 Test Heater (SUPERVISADO)
- [ ] `SET_HEATER_TEMPERATURE HEATER=extruder TARGET=50`
- [ ] Temperatura sube gradualmente
- [ ] Cuando alcanza 50°C, hotend fan arranca automáticamente
- [ ] `SET_HEATER_TEMPERATURE HEATER=extruder TARGET=0`
- [ ] Temperatura baja
- [ ] Cuando < 50°C, hotend fan se apaga

#### 7.5 Test Part Cooling Fan
- [ ] `SET_FAN_SPEED FAN=fan SPEED=0.5` (50%)
- [ ] Fan gira a media velocidad
- [ ] `SET_FAN_SPEED FAN=fan SPEED=1.0` (100%)
- [ ] Fan gira a máxima velocidad
- [ ] `SET_FAN_SPEED FAN=fan SPEED=0` (OFF)

#### 7.6 Test Probe (CRÍTICO)
- [ ] `QUERY_PROBE` sin trigger → "probe: open"
- [ ] Trigger manual (presionar sensor)
- [ ] `QUERY_PROBE` con trigger → "probe: triggered"
- [ ] Soltar sensor
- [ ] `QUERY_PROBE` → "probe: open"

**Simular fallo (fail-safe):**
- [ ] Desconectar cable probe
- [ ] `QUERY_PROBE` → debe reportar error o "triggered"
- [ ] Reconectar cable

#### 7.7 Test Homing
- [ ] `G28 X Y` (home XY con sensorless)
- [ ] `G28 Z` (home Z con probe)
- [ ] Verificar que Z se detiene suavemente al detectar cama

#### 7.8 Calibraciones
- [ ] `PROBE_CALIBRATE`:
  - Seguir instrucciones pantalla
  - Ajustar z_offset hasta papel justo pasa
  - `SAVE_CONFIG`

- [ ] `BED_MESH_CALIBRATE`:
  - Ejecuta sondeo 5x5 (o según config)
  - Verificar que completa sin errores
  - `SAVE_CONFIG`

#### 7.9 Test Print (pequeño)
- [ ] Imprimir test pequeño (cubo 20mm, primera capa)
- [ ] Verificar:
  - Primera capa adhiere bien (z_offset correcto)
  - Hotend calienta correctamente
  - Part cooling funciona
  - Hotend fan funciona cuando T > 50°C
  - No hay errores durante impresión

---

### Fase 8: Documentación Final (15-30 min)

- [ ] Fotografiar instalación completa (10+ fotos):
  - EBB42 montada
  - Conexiones en EBB42
  - Conexiones en SKR
  - Cable chain
  - Vista general toolhead

- [ ] Actualizar `printer.cfg` con valores calibrados:
  - `z_offset` del probe
  - PID del hotend (si se recalibró)

- [ ] Crear backup de configuración funcional

- [ ] Documentar cualquier issue o modificación realizada

- [ ] Marcar Phase 3 como ✅ COMPLETADA

---

## Troubleshooting

### Problema: EBB42 no aparece en `/dev/serial/by-id/`

**Causas posibles:**
1. Firmware no flasheado correctamente
2. Cable USB defectuoso
3. Puerto USB SKR no funciona

**Soluciones:**
- Re-flashear firmware EBB42
- Probar con otro cable USB
- Probar otro puerto USB en SKR
- Verificar que EBB42 tiene LED encendido (power OK)

---

### Problema: "ADC out of range" en thermistor

**Causas posibles:**
1. Thermistor desconectado o cable roto
2. Thermistor en cortocircuito
3. Tipo de thermistor incorrecto en config

**Soluciones:**
- Verificar continuidad cable thermistor
- Medir resistencia thermistor (~100kΩ @ 25°C)
- Verificar `sensor_type` en config coincide con hardware
- Revisar conexión física en TH0+/TH0-

---

### Problema: Heater no calienta

**Causas posibles:**
1. Heater desconectado
2. Fallo MOSFET en EBB42
3. Heater en cortocircuito

**Soluciones:**
- Medir resistencia heater (~12-20Ω para 24V/40W)
- Verificar continuidad cable heater
- Verificar voltaje en HE+/HE- cuando activado (debe ser ~24V)
- Si MOSFET fallado: usar puerto alternativo o reemplazar EBB42

---

### Problema: Probe siempre triggered o siempre open

**Causas posibles:**
1. Cable invertido (NO en vez de NC)
2. Pullup no configurado
3. Sensor Omron defectuoso

**Soluciones:**
- Verificar conexión NC → Signal, C → GND
- Verificar config: `pin: ^EBBCan:PB9` (con `^` de pullup)
- Test manual con multímetro:
  - Sin trigger: continuidad NC-C
  - Con trigger: no continuidad NC-C
- Si sensor defectuoso: reemplazar

---

### Problema: Crash del nozzle en la cama

**CRÍTICO - DETENER INMEDIATAMENTE**

**Causas posibles:**
1. Probe no funcional
2. z_offset incorrecto
3. Probe invertido (triggering al revés)

**Soluciones:**
- NUNCA hacer G28 Z sin verificar `QUERY_PROBE` funciona
- Re-calibrar z_offset con `PROBE_CALIBRATE`
- Verificar que probe trigger cuando se presiona (no cuando se suelta)
- En caso de duda: usar método manual para primera capa

---

## Reglas de Seguridad

### ⚠️ ANTES de Energizar

1. **Triple verificación:**
   - ✅ Polaridad 24V correcta (multímetro)
   - ✅ No hay cortocircuitos (multímetro resistencia)
   - ✅ Todos los conectores firmes

2. **Primera energización:**
   - ⚡ Energizar solo SKR primero
   - ⚡ Verificar voltaje 24V en cable a EBB42
   - ⚡ Si OK, conectar EBB42
   - ⚡ Verificar LED EBB42 enciende

### 🔥 Durante Testing de Heater

1. **NUNCA dejar heater sin supervisión**
2. **SIEMPRE** tener forma de apagar rápido (interruptor PSU)
3. **Temperatura máxima de test:** 50°C inicial, luego 200°C
4. **Thermal runaway:** Si temperatura sube fuera de control → APAGAR PSU

### 🛡️ Durante Calibración Probe

1. **SIEMPRE** tener mano en interruptor emergency stop
2. **NUNCA** confiar en probe sin test manual previo
3. **Velocidad Z lenta** durante primeras pruebas
4. **Baby steps:** Usar `Z_OFFSET_APPLY_PROBE` con valores conservadores

### 📸 Documentación

1. **Fotografiar ANTES de modificar** (permite rollback)
2. **Backup configs ANTES de cambiar** Klipper
3. **Etiquetar cables ANTES de desconectar**

---

## Checklist Final

### Hardware
- [ ] EBB42 firmemente montada (temporal OK)
- [ ] Todos los cables conectados según pinout
- [ ] Cable chain cierra correctamente
- [ ] No hay cables con tensión excesiva
- [ ] Hotend fan con espacio libre (no obstruido)

### Eléctrico
- [ ] 24V power medido: ~24V DC
- [ ] No hay cortocircuitos (multímetro)
- [ ] Polaridades verificadas
- [ ] Hotglue aplicado en conectores Dupont

### Klipper
- [ ] EBB42 detectado: `ls /dev/serial/by-id/`
- [ ] MCU 'EBBCan' connected en log
- [ ] No hay errores en consola
- [ ] Configuración guardada y backup creado

### Funcionalidad
- [ ] Thermistor lee temperatura razonable
- [ ] Heater calienta correctamente (test 50°C OK)
- [ ] Hotend fan arranca cuando T > 50°C
- [ ] Part cooling fan controlable (PWM OK)
- [ ] Probe responde: `QUERY_PROBE` OK
- [ ] Probe fail-safe: cable suelto = error
- [ ] Homing Z funciona sin crash
- [ ] Bed mesh calibration completa

### Calibración
- [ ] `z_offset` calibrado y guardado
- [ ] Bed mesh generado y guardado
- [ ] PID hotend calibrado (opcional)
- [ ] Primera capa test OK

### Documentación
- [ ] Fotos ANTES (stock)
- [ ] Fotos DESPUÉS (EBB42 instalada)
- [ ] Configuración Klipper documentada
- [ ] Issues/modificaciones anotadas

---

## Métricas Estimadas

| Métrica | Estimación |
|---------|------------|
| Tiempo total | 4-6 horas |
| Cables fabricados | 2 (USB + 24V) |
| Cables adaptados | 5 (heater, thermistor, probe, 2x fans) |
| Componentes migrados | 6 (heater, thermistor, probe, 2x fans, EBB42) |
| Fotos recomendadas | 20+ |
| Tests funcionales | 8 |

---

## Rollback Plan

Si algo falla y necesitas volver a configuración Phase 2:

1. **Hardware:**
   - Desconectar todos los cables de EBB42
   - Re-conectar componentes a placa stock (usar fotos ANTES)
   - Cables stock etiquetados permiten reconexión rápida

2. **Software:**
   - Restaurar backup de `printer.cfg` anterior a Phase 3
   - `RESTART` Klipper
   - Verificar funcionalidad básica

3. **Reversibilidad:**
   - ✅ No se cortaron cables originales
   - ✅ Placa stock conservada
   - ✅ Configuración Klipper backup disponible
   - ✅ Fotos permiten replicar conexión original

---

**Estado:** ✅ Documento completo, listo para implementación

**Última actualización:** 2025-12-21

**Autor:** mjcuadrado + Claude Code

**Versión:** 1.0
