# Configuración Dual-MCU en Klipper: SKR 1.4 Turbo + EBB42

**Fecha:** 2025-12-21
**Proyecto:** x5sa-skr-klipper - Conversión Tronxy X5SA a Klipper
**Hardware:** BTT SKR 1.4 Turbo (LPC1769) + BTT EBB42 CAN V1.2 (STM32G0B1)
**Modo:** Ambas MCUs conectadas por USB
**Estado:** ✅ CONFIGURADO Y FUNCIONANDO

---

## Índice

1. [Introducción](#1-introducción)
2. [Arquitectura del Sistema](#2-arquitectura-del-sistema)
3. [Configuración de printer.cfg](#3-configuración-de-printercfg)
4. [Sintaxis de Pines Multi-MCU](#4-sintaxis-de-pines-multi-mcu)
5. [Obtención de Serial IDs](#5-obtención-de-serial-ids)
6. [Verificación de Conexión](#6-verificación-de-conexión)
7. [Troubleshooting](#7-troubleshooting)
8. [Comandos Útiles de Debugging](#8-comandos-útiles-de-debugging)
9. [Ejemplos de Configuración](#9-ejemplos-de-configuración)
10. [Referencias](#10-referencias)

---

## 1. Introducción

### 1.1 Qué es una Configuración Dual-MCU

Klipper soporta múltiples microcontroladores (MCUs) en una misma impresora 3D. Esto permite:

- **Distribución de carga:** Procesamiento repartido entre varias MCUs
- **Especialización:** Cada MCU controla componentes específicos
- **Flexibilidad:** Agregar periféricos sin cambiar placa principal
- **Escalabilidad:** Toolheads modulares (ej: EBB42 en toolhead)

### 1.2 Nuestra Configuración

**MCU Principal ([mcu]):**
- **Hardware:** BTT SKR 1.4 Turbo
- **Chip:** LPC1769FBD100 (ARM Cortex-M3, 120 MHz)
- **Ubicación:** Frame superior de la impresora
- **Controla:**
  - Motores X, Y, Z (steppers de cinemática)
  - Motor E0 (extrusor, temporalmente hasta Phase 12)
  - Cama caliente (heated bed)
  - Ventiladores auxiliares

**MCU Secundaria ([mcu EBBCan]):**
- **Hardware:** BTT EBB42 CAN V1.2
- **Chip:** STM32G0B1CBT6 (ARM Cortex-M0+)
- **Ubicación:** Toolhead (provisional en Phase 3, definitivo en Phase 12)
- **Controla:**
  - Hotend heater (calentador)
  - Thermistor (sensor de temperatura)
  - Part cooling fan (ventilador de capa)
  - Hotend fan (ventilador de hotend)
  - Probe (sensor Omron para bed leveling)
  - *Futuro:* Motor extrusor (Phase 12 con Orbiter v2)

### 1.3 Modo de Conexión

**USB directo (NO CAN bus)**
- Ambas MCUs conectadas individualmente por USB al Raspberry Pi
- Simplicidad de configuración (no requiere transceiver CAN)
- IDs únicos en `/dev/serial/by-id/`

---

## 2. Arquitectura del Sistema

### 2.1 Topología Física

```
┌─────────────────────────────────────────────────────────────┐
│                       Raspberry Pi                          │
│                    (Klipper Host)                           │
└─────────────┬──────────────────────────┬────────────────────┘
              │                          │
              │ USB                      │ USB
              │                          │
┌─────────────▼──────────────┐  ┌────────▼──────────────────┐
│  SKR 1.4 Turbo (LPC1769)   │  │  EBB42 V1.2 (STM32G0B1)   │
│  [mcu]                     │  │  [mcu EBBCan]             │
├────────────────────────────┤  ├───────────────────────────┤
│ Steppers: X, Y, Z          │  │ Hotend Heater (HE)        │
│ Extruder: E0 (temporal)    │  │ Thermistor (TH0)          │
│ Heated Bed                 │  │ Part Cooling Fan (FAN0)   │
│ Auxiliar Fans              │  │ Hotend Fan (FAN1)         │
│ Endstops                   │  │ Probe (Omron NC)          │
│                            │  │ Extruder Motor E0 (Phase12)│
└────────────────────────────┘  └───────────────────────────┘
         │                                   │
         │                                   │
         └──────> 24V Power Line ────────────┘
                 (from PSU)
```

### 2.2 Flujo de Comunicación

```
G-code Command
      ↓
┌─────────────────┐
│  Raspberry Pi   │
│  klippy.py      │ ← Software Klipper (Python)
└────────┬────────┘
         │
         ├──> USB Serial ──> [mcu] SKR 1.4 Turbo
         │                     ↓
         │                   (ejecuta comandos en tiempo real)
         │
         └──> USB Serial ──> [mcu EBBCan] EBB42
                               ↓
                             (ejecuta comandos en tiempo real)
```

**Proceso:**
1. **Klipper Host** (Raspberry Pi) recibe G-code
2. **klippy.py** planifica movimientos y genera comandos de bajo nivel
3. Comandos se envían a MCUs correspondientes vía USB
4. Cada MCU ejecuta comandos en tiempo real
5. MCUs reportan estado de vuelta a Klipper Host

### 2.3 Ventajas de Esta Arquitectura

**Latencia baja:**
- Comunicación USB directa (más rápida que CAN en esta configuración)
- Sin overhead de protocolo CAN

**Redundancia:**
- Si una MCU falla, la otra puede seguir respondiendo
- Diagnóstico más fácil

**Independencia:**
- Cada MCU se flashea independientemente
- Configuraciones separadas en `make menuconfig`

---

## 3. Configuración de printer.cfg

### 3.1 Estructura Básica

```ini
# ==============================================================================
# CONFIGURACIÓN DE MCUs
# ==============================================================================

# ──────────────────────────────────────────────────────────────────────────────
# MCU Principal: BTT SKR 1.4 Turbo (LPC1769)
# ──────────────────────────────────────────────────────────────────────────────
[mcu]
serial: /dev/serial/by-id/usb-Klipper_lpc1769_12345-if00
# Sustituir "12345" por el ID real obtenido con `ls /dev/serial/by-id/`

# ──────────────────────────────────────────────────────────────────────────────
# MCU Secundaria: BTT EBB42 CAN V1.2 (STM32G0B1) - Modo USB
# ──────────────────────────────────────────────────────────────────────────────
[mcu EBBCan]
serial: /dev/serial/by-id/usb-Klipper_stm32g0b1xx_12345-if00
# Sustituir "12345" por el ID real obtenido con `ls /dev/serial/by-id/`

# ==============================================================================
# COMPONENTES EN MCU PRINCIPAL (SKR 1.4 Turbo)
# ==============================================================================

[stepper_x]
step_pin: P2.2
dir_pin: P2.6
enable_pin: !P2.1
# ... resto de configuración

[stepper_y]
step_pin: P0.19
dir_pin: P0.20
enable_pin: !P2.8
# ... resto de configuración

[stepper_z]
step_pin: P0.22
dir_pin: P2.11
enable_pin: !P0.21
# ... resto de configuración

[extruder]
step_pin: P2.13          # Motor E0 en SKR (temporal hasta Phase 12)
dir_pin: P0.11
enable_pin: !P2.12
# NOTA: heater_pin y sensor_pin están en EBB42 (ver abajo)
heater_pin: EBBCan:PB13  # Heater en EBB42
sensor_pin: EBBCan:PA3   # Thermistor en EBB42
# ... resto de configuración

[heater_bed]
heater_pin: P2.5
sensor_pin: P0.25
# ... resto de configuración

# ==============================================================================
# COMPONENTES EN MCU SECUNDARIA (EBB42)
# ==============================================================================

# NOTA: El [extruder] ya definido arriba usa heater y sensor de EBB42
# No se re-define [extruder], solo se usan pines con prefijo "EBBCan:"

[fan]
# Ventilador de capa (part cooling) - PWM controlado
pin: EBBCan:PA1

[heater_fan hotend_fan]
# Ventilador de hotend - Always-on cuando T > 50°C
pin: EBBCan:PA0
heater: extruder
heater_temp: 50.0
fan_speed: 1.0

[probe]
# Sensor Omron TL-Q5MC1-Z (Normally Closed - Fail-safe)
pin: ^EBBCan:PB9
x_offset: 0
y_offset: 25.0
z_offset: 0.5
speed: 5.0
samples: 2
sample_retract_dist: 2.0
samples_result: average
samples_tolerance: 0.05
samples_tolerance_retries: 3
```

### 3.2 Convenciones de Nombres

**Nombre de MCU secundaria:** `EBBCan`
- Aunque estamos usando USB (no CAN), mantenemos el nombre `EBBCan` por:
  - Compatibilidad con configuraciones estándar de EBB42
  - Documentación y ejemplos de Klipper usan este nombre
  - Facilita migración futura a CAN si se desea
  - Es solo un identificador interno (no afecta funcionamiento)

**Alternativas válidas:**
```ini
[mcu toolhead]      # Genérico
[mcu ebb42]         # Específico
[mcu extruder_mcu]  # Descriptivo
```

**Uso consistente:**
- Una vez elegido el nombre, usarlo en TODOS los pines de esa MCU
- Ejemplo: Si usas `[mcu toolhead]`, los pines serán `toolhead:PA1`, `toolhead:PB13`, etc.

### 3.3 Serial IDs - CRÍTICO

**SIEMPRE usar rutas por ID, NUNCA por dispositivo:**

❌ **INCORRECTO:**
```ini
[mcu]
serial: /dev/ttyACM0  # Puede cambiar entre reinicios

[mcu EBBCan]
serial: /dev/ttyACM1  # Puede cambiar si se conecta primero la otra MCU
```

✅ **CORRECTO:**
```ini
[mcu]
serial: /dev/serial/by-id/usb-Klipper_lpc1769_12345-if00

[mcu EBBCan]
serial: /dev/serial/by-id/usb-Klipper_stm32g0b1xx_12345-if00
```

**Por qué:**
- `/dev/ttyACM*` cambia según orden de conexión USB
- `/dev/serial/by-id/` es un enlace simbólico estable basado en hardware
- Klipper necesita saber qué MCU es cuál consistentemente

---

## 4. Sintaxis de Pines Multi-MCU

### 4.1 Reglas de Sintaxis

**MCU Principal (sin prefijo):**
```ini
[stepper_x]
step_pin: P2.2       # Implícitamente en [mcu] (MCU principal)
```

**MCU Secundaria (con prefijo):**
```ini
[fan]
pin: EBBCan:PA1      # Explícitamente en [mcu EBBCan]
```

**Formato general:**
```
<nombre_mcu>:<pin>
```

### 4.2 Modificadores de Pin

**Inversión (!):**
```ini
enable_pin: !P2.1           # Invertir señal en MCU principal
enable_pin: !EBBCan:PD2     # Invertir señal en EBB42
```

**Pull-up (^):**
```ini
pin: ^PB9                   # Pull-up en MCU principal
pin: ^EBBCan:PB9            # Pull-up en EBB42
```

**Pull-down (v):**
```ini
pin: vPA5                   # Pull-down en MCU principal
pin: vEBBCan:PA5            # Pull-down en EBB42
```

**Combinación:**
```ini
pin: ^!EBBCan:PB9           # Pull-up + Inversión en EBB42
```

**Orden de modificadores:** `^` o `v` primero, luego `!`

### 4.3 Ejemplo Completo de Extrusor Dual-MCU

**Escenario:** Motor en SKR, heater y thermistor en EBB42

```ini
[extruder]
# Motor - SKR 1.4 Turbo (MCU principal)
step_pin: P2.13              # Pin en SKR
dir_pin: P0.11               # Pin en SKR
enable_pin: !P2.12           # Pin en SKR (invertido)
microsteps: 16
rotation_distance: 33.500
nozzle_diameter: 0.400
filament_diameter: 1.750

# Heater - EBB42 (MCU secundaria)
heater_pin: EBBCan:PB13      # Pin en EBB42

# Thermistor - EBB42 (MCU secundaria)
sensor_type: EPCOS 100K B57560G104F
sensor_pin: EBBCan:PA3       # Pin en EBB42

# Control PID
control: pid
pid_Kp: 22.2
pid_Ki: 1.08
pid_Kd: 114
min_temp: 0
max_temp: 250
```

**Klipper automáticamente:**
- Envía comandos de motor a SKR vía USB
- Envía comandos de heater a EBB42 vía USB
- Lee temperatura de EBB42
- Sincroniza todo en tiempo real

### 4.4 Tabla de Referencia de Pines

**SKR 1.4 Turbo (LPC1769):**

| Función | Pin | Uso Actual |
|---------|-----|------------|
| X_STEP | P2.2 | Stepper X |
| X_DIR | P2.6 | Stepper X |
| Y_STEP | P0.19 | Stepper Y |
| Y_DIR | P0.20 | Stepper Y |
| Z_STEP | P0.22 | Stepper Z |
| Z_DIR | P2.11 | Stepper Z |
| E0_STEP | P2.13 | Extruder (temporal) |
| E0_DIR | P0.11 | Extruder (temporal) |
| HE0 | P2.7 | *No usado (heater en EBB42)* |
| TH0 | P0.24 | *No usado (thermistor en EBB42)* |
| HB | P2.5 | Heated Bed |
| TB | P0.25 | Bed Thermistor |

**EBB42 CAN V1.2 (STM32G0B1):**

| Función | Pin | Uso Actual |
|---------|-----|------------|
| HE | PB13 | Hotend Heater |
| TH0 | PA3 | Thermistor (ADC) |
| FAN0 | PA1 | Part Cooling Fan (PWM) |
| FAN1 | PA0 | Hotend Fan (always-on) |
| PROBE | PB9 | Omron Probe (NC) |
| E_STEP | PD0 | *Futuro: Extruder (Phase 12)* |
| E_DIR | PD1 | *Futuro: Extruder (Phase 12)* |
| E_EN | PD2 | *Futuro: Extruder (Phase 12)* |

---

## 5. Obtención de Serial IDs

### 5.1 Método 1: Listar Todos los Dispositivos

```bash
ls /dev/serial/by-id/
```

**Salida esperada:**
```
usb-Klipper_lpc1769_12345-if00
usb-Klipper_stm32g0b1xx_67890-if00
```

**Identificar cuál es cuál:**
- `lpc1769` → SKR 1.4 Turbo
- `stm32g0b1xx` → EBB42 CAN V1.2

### 5.2 Método 2: Filtrar por Tipo

```bash
# Solo SKR
ls /dev/serial/by-id/ | grep lpc1769

# Solo EBB42
ls /dev/serial/by-id/ | grep stm32g0b1
```

### 5.3 Método 3: Ver Detalles con lsusb

```bash
lsusb | grep Klipper
```

**Salida esperada:**
```
Bus 001 Device 005: ID 1d50:614e OpenMoko, Inc. lpc1769
Bus 001 Device 006: ID 1d50:614e OpenMoko, Inc. stm32g0b1xx
```

**Obtener más información:**
```bash
lsusb -v -d 1d50:614e | grep -A 10 "iSerial"
```

### 5.4 Copiar Serial ID Correcto

**Forma segura:**
```bash
# Copiar al portapapeles (si estás en GUI)
ls /dev/serial/by-id/ | grep lpc1769 | xargs echo /dev/serial/by-id/ | tr -d '\n' | xclip -selection clipboard

# O simplemente copiar manualmente del output
ls /dev/serial/by-id/
# Seleccionar texto y Ctrl+Shift+C
```

**Pegar en printer.cfg:**
```ini
[mcu]
serial: /dev/serial/by-id/usb-Klipper_lpc1769_XXXXXXXXXXXXXXXX-if00
#                                       ^^^^^^^^^^^^^^^^^^^^^^^^
#                                       Este es el ID único de tu placa
```

---

## 6. Verificación de Conexión

### 6.1 Antes de Iniciar Klipper

**Verificar ambas MCUs detectadas:**
```bash
ls /dev/serial/by-id/ | wc -l
```

**Resultado esperado:** `2` (dos líneas)

**Verificar permisos:**
```bash
ls -l /dev/serial/by-id/
```

**Resultado esperado:**
```
lrwxrwxrwx 1 root root 13 Dec 21 XX:XX usb-Klipper_lpc1769_... -> ../../ttyACM0
lrwxrwxrwx 1 root root 13 Dec 21 XX:XX usb-Klipper_stm32g0b1xx_... -> ../../ttyACM1
```

### 6.2 Iniciar Klipper

```bash
sudo systemctl restart klipper
```

**Esperar 5-10 segundos para inicialización.**

### 6.3 Verificar Estado del Servicio

```bash
sudo systemctl status klipper
```

**Salida esperada (saludable):**
```
● klipper.service - Klipper 3D Printer Firmware
   Loaded: loaded (/etc/systemd/system/klipper.service; enabled; vendor preset: enabled)
   Active: active (running) since Sat 2025-12-21 XX:XX:XX UTC; 30s ago
 Main PID: XXXX (python)
    Tasks: 2 (limit: 4915)
   Memory: XX.XM
   CGroup: /system.slice/klipper.service
           └─XXXX /usr/bin/python /home/pi/klipper/klippy/klippy.py ...

Dec 21 XX:XX:XX systemd[1]: Started Klipper 3D Printer Firmware.
```

**Indicadores de éxito:**
- ✅ `Active: active (running)`
- ✅ No hay errores en rojo

### 6.4 Revisar Logs de Klipper

```bash
tail -f /tmp/klippy.log
```

**Buscar estas líneas (CRÍTICAS):**
```
Starting Klipper...
Start printer at ...
Loaded MCU 'mcu' 110 commands (v0.12.0-239-g8b8f7c09 / gcc: ...-lpc176x)
Loaded MCU 'EBBCan' 110 commands (v0.12.0-239-g8b8f7c09 / gcc: ...-stm32g0b1)
mcu 'mcu': got {'oid': '...', 'stats': ...}
mcu 'EBBCan': got {'oid': '...', 'stats': ...}
```

**Ambas MCUs deben:**
1. Cargar comandos (`Loaded MCU`)
2. Establecer comunicación (`got {'oid': ...}`)

### 6.5 Verificación en Interfaz Web (Mainsail/Fluidd)

**Ir a la interfaz web (ej: http://mainsail.local)**

**Dashboard → Machine → MCUs**

Debe mostrar:
```
mcu (SKR 1.4 Turbo)
├─ Status: Connected
├─ MCU Version: v0.12.0-239-g8b8f7c09
└─ Frequency: 120 MHz

EBBCan (EBB42 V1.2)
├─ Status: Connected
├─ MCU Version: v0.12.0-239-g8b8f7c09
└─ Frequency: 64 MHz
```

**Si no aparecen ambas MCUs:** Ver sección Troubleshooting.

---

## 7. Troubleshooting

### 7.1 Una MCU No Se Detecta

**Síntoma:**
```bash
ls /dev/serial/by-id/
# Solo aparece una MCU
```

**Diagnóstico:**

1. **Verificar conexión USB:**
   ```bash
   lsusb | grep Klipper
   # Debe mostrar 2 dispositivos
   ```

2. **Si solo aparece 1 en lsusb:**
   - Verificar cable USB de la MCU faltante
   - Probar otro puerto USB en Raspberry Pi
   - Para EBB42: Verificar alimentación 24V conectada
   - Presionar botón RST en la MCU faltante

3. **Si aparecen ambas en lsusb pero solo una en /dev/serial/by-id/:**
   ```bash
   lsusb
   # Buscar dispositivos con ID 0483:df11 (modo DFU)
   ```

   Si la MCU está en modo DFU:
   - SKR: Re-flashear con tarjeta SD
   - EBB42: Presionar RST (sin BOOT) para salir de DFU

### 7.2 Klipper No Inicia - Error de Serial

**Síntoma en /tmp/klippy.log:**
```
mcu 'EBBCan': Unable to open serial port: [Errno 2] No such file or directory: '/dev/serial/by-id/usb-Klipper_stm32g0b1xx_...'
```

**Solución:**

1. **Verificar que el serial ID existe:**
   ```bash
   ls -l /dev/serial/by-id/usb-Klipper_stm32g0b1xx_*
   ```

2. **Si no existe, obtener el correcto:**
   ```bash
   ls /dev/serial/by-id/ | grep stm32g0b1
   # Copiar el ID completo
   ```

3. **Actualizar printer.cfg:**
   ```ini
   [mcu EBBCan]
   serial: /dev/serial/by-id/[ID_CORRECTO_AQUI]
   ```

4. **Guardar y reiniciar Klipper:**
   ```bash
   sudo systemctl restart klipper
   ```

### 7.3 Error "MCU Protocol Error"

**Síntoma en /tmp/klippy.log:**
```
mcu 'EBBCan': command format mismatch
Protocol error connecting to printer
```

**Causa:** Versión de firmware en MCU no coincide con versión esperada por Klipper.

**Solución:**

1. **Recompilar firmware:**
   ```bash
   cd ~/klipper
   make clean
   make menuconfig  # Verificar configuración
   make
   ```

2. **Re-flashear la MCU afectada:**
   - SKR: Copiar `out/klipper.bin` a SD como `firmware.bin`
   - EBB42: `sudo dfu-util -a 0 -d 0483:df11 --dfuse-address 0x08000000 -D ~/klipper/out/klipper.bin`

3. **Reiniciar Klipper:**
   ```bash
   sudo systemctl restart klipper
   ```

### 7.4 MCU Se Desconecta Aleatoriamente

**Síntoma en /tmp/klippy.log:**
```
Lost communication with MCU 'EBBCan'
Timer too close
```

**Posibles causas:**

1. **Cable USB defectuoso:**
   - Probar otro cable USB
   - Verificar longitud < 5 metros
   - Usar cable con ferritas antiinterferencia

2. **Alimentación insuficiente:**
   - Para EBB42: Verificar 24V estable
   - Medir voltaje en terminales VIN/GND
   - Verificar cable de alimentación grueso (AWG 20/18)

3. **Interferencia electromagnética:**
   - Alejar cables USB de cables de alta corriente
   - Usar ferritas en cables USB
   - Separar cables de motor de cables de señal

4. **Sobrecarga de comunicación:**
   ```ini
   # En printer.cfg, reducir frecuencia de polling
   [mcu EBBCan]
   serial: /dev/serial/by-id/usb-Klipper_stm32g0b1xx_...
   # No agregar opciones extra por ahora
   ```

### 7.5 Error "Pin Already Used"

**Síntoma:**
```
Option 'pin' in section 'fan' must be specified
Pin PA1 used by multiple sections
```

**Causa:** El mismo pin se define en múltiples lugares.

**Solución:**

1. **Buscar duplicados en printer.cfg:**
   ```bash
   grep -n "pin: EBBCan:PA1" ~/printer_data/config/printer.cfg
   ```

2. **Eliminar o comentar definiciones duplicadas.**

3. **Reiniciar Klipper:**
   ```bash
   sudo systemctl restart klipper
   ```

### 7.6 Heater No Calienta (Pin en MCU Incorrecta)

**Síntoma:**
- Temperatura reportada correctamente
- Comando `M104 S200` no calienta

**Diagnóstico:**

1. **Verificar sintaxis de pin:**
   ```ini
   [extruder]
   heater_pin: EBBCan:PB13  # ¿Falta el prefijo EBBCan?
   ```

2. **Verificar que el pin es correcto para tu placa:**
   - SKR 1.4: Heater en `P2.7` (HE0)
   - EBB42: Heater en `PB13` (HE)

3. **Test manual de heater:**
   En consola de Klipper:
   ```gcode
   SET_HEATER_TEMPERATURE HEATER=extruder TARGET=50
   # Esperar y verificar si temperatura sube
   ```

4. **Si no funciona:**
   - Verificar conexión física del heater en la placa EBB42
   - Medir resistencia del heater (debe ser ~12-15Ω para 24V 40W)
   - Probar pin con LED o multímetro

---

## 8. Comandos Útiles de Debugging

### 8.1 Verificación de Hardware

**Ver todas las MCUs conectadas:**
```bash
ls /dev/serial/by-id/
```

**Ver dispositivos USB Klipper:**
```bash
lsusb | grep -E "Klipper|1d50:614e"
```

**Seguir logs en tiempo real:**
```bash
tail -f /tmp/klippy.log
```

**Filtrar solo errores:**
```bash
grep -i "error\|warning\|fail" /tmp/klippy.log | tail -20
```

### 8.2 Reiniciar Servicios

**Reiniciar solo Klipper:**
```bash
sudo systemctl restart klipper
```

**Reiniciar Klipper + Moonraker:**
```bash
sudo systemctl restart klipper moonraker
```

**Ver estado de servicios:**
```bash
systemctl status klipper moonraker nginx
```

### 8.3 Comandos G-code de Diagnóstico

**Desde interfaz web (Mainsail/Fluidd) o consola Klipper:**

**Status de MCUs:**
```gcode
STATUS
```

**Firmware versions:**
```gcode
FIRMWARE_RESTART
# Luego revisar logs para ver versiones
```

**Test de probe:**
```gcode
QUERY_PROBE
# Output: probe: OPEN o probe: TRIGGERED
```

**Test de heater:**
```gcode
SET_HEATER_TEMPERATURE HEATER=extruder TARGET=50
GET_TEMPERATURE
# Verificar que temperatura sube
```

**Test de fan:**
```gcode
M106 S128  # 50% speed
# Verificar que ventilador gira
M106 S0    # Apagar
```

### 8.4 Verificar Configuración de Pin

**Desde Python en Raspberry Pi:**
```bash
cd ~/klipper
python scripts/klippy.py ~/printer_data/config/printer.cfg -v
```

**Buscar errores de sintaxis:**
```bash
grep -n "^pin:" ~/printer_data/config/printer.cfg
# Verificar que todos los pines de EBB42 tienen prefijo "EBBCan:"
```

### 8.5 Reiniciar MCU Específica

**FIRMWARE_RESTART reinicia todas las MCUs:**
```gcode
FIRMWARE_RESTART
```

**No hay comando para reiniciar solo una MCU (limitación de Klipper).**

### 8.6 Ver Estadísticas de Comunicación

```bash
tail -f /tmp/klippy.log | grep "stats"
```

**Salida esperada:**
```
Stats 120.0: mcu='mcu' oid=XX bytes_write=XXXX bytes_read=XXXX ...
Stats 120.0: mcu='EBBCan' oid=XX bytes_write=XXXX bytes_read=XXXX ...
```

**Indicadores:**
- `bytes_write` y `bytes_read` aumentando → Comunicación activa
- `retries` o `errors` → Problemas de conexión

---

## 9. Ejemplos de Configuración

### 9.1 Configuración Mínima Dual-MCU

```ini
# printer.cfg mínimo para dual-MCU

[mcu]
serial: /dev/serial/by-id/usb-Klipper_lpc1769_XXXX-if00

[mcu EBBCan]
serial: /dev/serial/by-id/usb-Klipper_stm32g0b1xx_XXXX-if00

[printer]
kinematics: corexy  # O cartesian, según tu impresora
max_velocity: 300
max_accel: 3000
max_z_velocity: 5
max_z_accel: 100
```

**Este es el mínimo para que Klipper inicie.**

### 9.2 Extrusor con Heater y Thermistor en EBB42

```ini
[extruder]
# Motor en SKR (MCU principal)
step_pin: P2.13
dir_pin: P0.11
enable_pin: !P2.12
microsteps: 16
rotation_distance: 33.500
nozzle_diameter: 0.400
filament_diameter: 1.750
max_extrude_only_distance: 100.0

# Heater en EBB42
heater_pin: EBBCan:PB13
max_power: 1.0

# Thermistor en EBB42
sensor_type: EPCOS 100K B57560G104F
sensor_pin: EBBCan:PA3
pullup_resistor: 4700

# Control PID (calibrar con PID_CALIBRATE HEATER=extruder TARGET=200)
control: pid
pid_Kp: 22.2
pid_Ki: 1.08
pid_Kd: 114

min_temp: 0
max_temp: 265
min_extrude_temp: 170
```

### 9.3 Ventiladores en EBB42

```ini
[fan]
# Part cooling fan - Controlado PWM
pin: EBBCan:PA1
max_power: 1.0
shutdown_speed: 0
cycle_time: 0.010
hardware_pwm: False
kick_start_time: 0.100
off_below: 0.0

[heater_fan hotend_fan]
# Hotend fan - Always-on cuando hotend > 50°C
pin: EBBCan:PA0
max_power: 1.0
shutdown_speed: 0
heater: extruder
heater_temp: 50.0
fan_speed: 1.0
```

### 9.4 Probe en EBB42 (Omron NC)

```ini
[probe]
pin: ^EBBCan:PB9      # Pull-up habilitado
#    ^               ← Pull-up (crítico para NC probe)
#     EBBCan:PB9     ← Pin en EBB42

x_offset: 0           # Ajustar según tu montaje
y_offset: 25.0        # Ajustar según tu montaje
z_offset: 0.5         # Calibrar con PROBE_CALIBRATE

speed: 5.0            # Velocidad de approach (mm/s)
samples: 2            # Número de muestras por punto
sample_retract_dist: 2.0
samples_result: average
samples_tolerance: 0.05
samples_tolerance_retries: 3

# Para probe NC (Normally Closed - Fail-safe)
# Cuando probe NO está tocado: pin lee HIGH (cerrado)
# Cuando probe está tocado: pin lee LOW (abierto)
# Esto es opuesto a NO (Normally Open) pero es más seguro
```

### 9.5 Bed Leveling con Probe

```ini
[safe_z_home]
home_xy_position: 150, 150  # Centro de tu cama
speed: 50
z_hop: 10
z_hop_speed: 5

[bed_mesh]
speed: 120
horizontal_move_z: 5
mesh_min: 30, 30
mesh_max: 300, 300
probe_count: 5, 5
algorithm: bicubic
fade_start: 1.0
fade_end: 10.0
```

### 9.6 ADXL345 Acelerómetro en EBB42 (Futuro)

```ini
[adxl345]
cs_pin: EBBCan:PB12
spi_bus: spi1        # Verificar según tu placa EBB42

[resonance_tester]
accel_chip: adxl345
probe_points:
    150, 150, 20     # Centro de tu cama
```

---

## 10. Referencias

### 10.1 Documentación Oficial de Klipper

**Multi-MCU Configuration:**
- [Config Reference - MCU](https://www.klipper3d.org/Config_Reference.html#mcu)
- [Multi-MCU Homing](https://www.klipper3d.org/Config_Reference.html#multi-mcu-homing)

**Comandos G-code:**
- [G-Codes](https://www.klipper3d.org/G-Codes.html)
- [Status Reference](https://www.klipper3d.org/Status_Reference.html)

**Debugging:**
- [Debugging](https://www.klipper3d.org/Debugging.html)
- [FAQ](https://www.klipper3d.org/FAQ.html)

### 10.2 Documentación de Hardware

**BTT SKR 1.4 Turbo:**
- [GitHub - SKR 1.4 Turbo](https://github.com/bigtreetech/BIGTREETECH-SKR-V1.3/tree/master/BTT%20SKR%20V1.4)
- [Pinout Diagram](https://github.com/bigtreetech/BIGTREETECH-SKR-V1.3/blob/master/BTT%20SKR%20V1.4/Hardware/BTT%20SKR%20V1.4-Pin.pdf)

**BTT EBB42 CAN V1.2:**
- [GitHub - EBB Series](https://github.com/bigtreetech/EBB)
- [EBB42 Manual](https://github.com/bigtreetech/EBB/tree/master/EBB%20CAN%20V1.1%20%28STM32G0B1%29)
- [Pinout](https://github.com/bigtreetech/EBB/blob/master/EBB%20CAN%20V1.2/EBB42%20CAN%20V1.2/Hardware/EBB42%20CAN%20V1.2-PIN.png)

### 10.3 Recursos de la Comunidad

**Klipper Discourse:**
- [Multi-MCU discussions](https://klipper.discourse.group/search?q=multi%20mcu)
- [EBB42 USB mode](https://klipper.discourse.group/search?q=ebb42%20usb)

**Reddit:**
- [r/klippers](https://www.reddit.com/r/klippers/)

### 10.4 Documentos de Este Proyecto

- [FLASHEO_SKR_EXITOSO.md](FLASHEO_SKR_EXITOSO.md) - Flasheo de SKR 1.4 Turbo
- [FLASHEO_EBB42_EXITOSO.md](FLASHEO_EBB42_EXITOSO.md) - Flasheo de EBB42 CAN V1.2
- [EBB42_INTEGRATION.md](EBB42_INTEGRATION.md) - Guía completa de integración
- [MATERIALS_CHECKLIST.md](MATERIALS_CHECKLIST.md) - Checklist de materiales

---

## Conclusión

La configuración dual-MCU en Klipper es poderosa y flexible, permitiendo:

- **Distribución de componentes:** Cada MCU controla lo que está físicamente cerca
- **Modularidad:** Cambiar o actualizar toolheads sin afectar MCU principal
- **Escalabilidad:** Agregar más MCUs si es necesario (ej: segunda extrusora)
- **Robustez:** Configuración probada y ampliamente usada en la comunidad

**Puntos clave recordar:**

1. ✅ Siempre usar `/dev/serial/by-id/` (nunca `/dev/ttyACM*`)
2. ✅ Prefijo `EBBCan:` para todos los pines de MCU secundaria
3. ✅ Ambas MCUs deben aparecer en logs: `Loaded MCU 'mcu'` y `Loaded MCU 'EBBCan'`
4. ✅ Verificar permisos de serial ports (usuario en grupo `tty` y `dialout`)
5. ✅ Mantener versiones de firmware sincronizadas entre MCUs

**Estado actual del sistema:** ✅ SKR 1.4 Turbo + EBB42 funcionando en dual-MCU

---

**Última actualización:** 2025-12-21
**Autor:** Proyecto x5sa-skr-klipper
**Hardware validado:** BTT SKR 1.4 Turbo (LPC1769) + BTT EBB42 CAN V1.2 (STM32G0B1)
