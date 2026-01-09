# Instalacion BIGTREETECH Eddy Duo (USB)

**Fecha:** 2026-01-09
**Hardware:** BIGTREETECH Eddy Duo (RP2040 + LDC1612)
**Conexion:** USB directo a Raspberry Pi
**Version Guia:** 1.0

---

## Indice

1. [Por que Eddy Duo](#1-por-que-eddy-duo)
2. [Requisitos Previos](#2-requisitos-previos)
3. [Flashear Klipper en Eddy Duo](#3-flashear-klipper-en-eddy-duo)
4. [Conexion USB](#4-conexion-usb)
5. [Configuracion Klipper](#5-configuracion-klipper)
6. [Calibracion](#6-calibracion)
7. [Troubleshooting](#7-troubleshooting)

---

## 1. Por que Eddy Duo

### Diferencias con Eddy Coil V1.0

| Caracteristica | Eddy Coil V1.0 | Eddy Duo |
|----------------|----------------|----------|
| MCU | Ninguno (solo sensor) | RP2040 integrado |
| Conexion | I2C (a traves de EBB42) | USB directo o I2C |
| Sensor temp | No | Si (compensacion termica) |
| Multisuperficie | Basico | Mejorado (mejor para camas hierro) |
| Dependencia | Necesita EBB42/toolhead | Independiente |

### Ventajas para camas de hierro/acero

- **Compensacion termica:** El termistor integrado permite compensar las variaciones de temperatura
- **Mejor deteccion:** Algoritmo mejorado para superficies magneticas/hierro
- **Independencia:** No depende de la EBB42, menos puntos de fallo
- **Cable USB incluido:** Facil instalacion temporal

---

## 2. Requisitos Previos

### Hardware

- BIGTREETECH Eddy Duo (con cable USB incluido)
- Puerto USB libre en Raspberry Pi
- Soporte de montaje para el toolhead

### Software

- Klipper instalado en Raspberry Pi
- Acceso SSH a la Pi
- Mainsail/Fluidd operativo

---

## 3. Flashear Klipper en Eddy Duo

El Eddy Duo tiene un **RP2040** que necesita firmware Klipper.

### 3.1 Compilar firmware

```bash
cd ~/klipper

make menuconfig
```

**Configuracion:**

```
[*] Enable extra low-level configuration options
    Micro-controller Architecture (Raspberry Pi RP2040)
    Bootloader offset (No bootloader)
    Flash chip (W25Q080 with CLKDIV 2)
    Communication interface (USB)
    USB ids  -->  (usar valores por defecto)
```

**Guardar y salir** (Q, Y)

### 3.2 Compilar

```bash
make clean
make
```

### 3.3 Poner Eddy Duo en modo BOOTSEL

1. **Desconectar** el Eddy Duo del USB (si esta conectado)
2. **Mantener pulsado** el boton BOOT en el Eddy Duo
3. **Mientras mantienes pulsado**, conectar el cable USB a la Pi
4. **Soltar** el boton BOOT despues de 1-2 segundos

El Eddy Duo aparecera como dispositivo de almacenamiento USB.

### 3.4 Verificar modo BOOTSEL

```bash
lsblk
```

Deberia aparecer algo como:
```
sda      8:0    1   128M  0 disk
└─sda1   8:1    1   128M  0 part /media/pi/RPI-RP2
```

O tambien:
```bash
ls /dev/disk/by-id/
```
Busca: `usb-RPI_RP2_...`

### 3.5 Flashear firmware

```bash
# Montar si no esta montado
sudo mount /dev/sda1 /mnt

# Copiar firmware
sudo cp ~/klipper/out/klipper.uf2 /mnt/

# El Eddy Duo se reiniciara automaticamente
```

Alternativa si no se monta automaticamente:
```bash
make flash FLASH_DEVICE=/dev/sda
```

### 3.6 Verificar flasheo exitoso

Despues del reinicio automatico:

```bash
ls /dev/serial/by-id/
```

Deberia aparecer algo como:
```
usb-Klipper_rp2040_E66118604B372A28-if00
```

**Guardar este ID** - lo necesitaras para printer.cfg

---

## 4. Conexion USB

### 4.1 Conexion fisica

1. Conectar cable USB del Eddy Duo a un puerto USB de la Pi
2. Preferir puertos USB 2.0 (mas estables para Klipper)
3. Evitar hubs USB si es posible

### 4.2 Montaje en toolhead

**Altura critica:** El Eddy Duo debe estar **2-3mm por encima del nozzle**

```
Vista lateral:

      [Eddy Duo]
           ║
  ╔════════╩════════╗
  ║   Toolhead      ║
  ╚════════╤════════╝
           │
      [Nozzle]  ← 2-3mm mas bajo
```

### 4.3 Cable routing

- Dejar suficiente holgura para movimiento XY completo
- No tensar el cable
- Fijar con bridas sin apretar demasiado
- El cable USB es mas robusto que el I2C anterior

---

## 5. Configuracion Klipper

### 5.1 Anadir MCU del Eddy Duo

En `printer.cfg`, anadir despues del MCU principal:

```ini
# Probe MCU - BTT Eddy Duo (RP2040) via USB
[mcu eddy]
serial: /dev/serial/by-id/usb-Klipper_rp2040_XXXXX-if00
# Reemplazar XXXXX con tu ID real
```

### 5.2 Configurar el probe

```ini
[probe_eddy_current btt_eddy]
sensor_type: ldc1612
sensor_pin: eddy:gpio26

x_offset: 0.0
y_offset: 0.0
z_offset: 1.0

speed: 120
lift_speed: 60.0
samples: 2
samples_result: average
sample_retract_dist: 2.0
samples_tolerance: 0.050
samples_tolerance_retries: 3
```

### 5.3 Sensor de temperatura (compensacion termica)

```ini
[temperature_sensor eddy_temp]
sensor_type: Generic 3950
sensor_pin: eddy:gpio27
min_temp: 0
max_temp: 85
```

### 5.4 Reiniciar Klipper

```bash
sudo systemctl restart klipper
```

O desde Mainsail: **Power** > **Host** > **Firmware Restart**

---

## 6. Calibracion

### 6.1 Verificar comunicacion

```gcode
QUERY_PROBE
```

Resultado esperado: `probe: open`

Si hay error, ver seccion [Troubleshooting](#7-troubleshooting).

### 6.2 Calibrar drive current

**IMPORTANTE:** Mover toolhead lejos de la cama primero (Z alto)

```gcode
LDC_CALIBRATE_DRIVE_CURRENT CHIP=btt_eddy
```

Guardar:
```gcode
SAVE_CONFIG
```

### 6.3 Calibrar Z offset

Despues del reinicio:

```gcode
G28                                    # Home all
PROBE_EDDY_CURRENT_CALIBRATE CHIP=btt_eddy
```

**Proceso interactivo:**

1. El nozzle se acercara a la cama
2. Usar **TESTZ Z=-0.1** para bajar
3. Usar **TESTZ Z=+0.1** para subir
4. Objetivo: papel desliza con ligera resistencia
5. Cuando este correcto: **ACCEPT**
6. Guardar: **SAVE_CONFIG**

### 6.4 Verificar Z offset

```gcode
G28
G1 X165 Y165 F6000
PROBE
```

Deberia parar cerca de Z=0.

### 6.5 Compensacion termica (opcional)

Para mejor precision con cama caliente:

1. Calentar cama a temperatura de impresion (60-80C)
2. Esperar 10-15 minutos para estabilizar
3. Repetir calibracion de Z offset

```ini
# Activar en printer.cfg si se desea
[temperature_probe btt_eddy]
sensor_type: Generic 3950
sensor_pin: eddy:gpio27
horizontal_move_z: 2
```

---

## 7. Troubleshooting

### Error: "mcu 'eddy': Unable to connect"

**Causas:**
1. Serial ID incorrecto en printer.cfg
2. Firmware no flasheado correctamente
3. Cable USB defectuoso

**Solucion:**
```bash
# Verificar que aparece el dispositivo
ls /dev/serial/by-id/usb-Klipper_rp2040*

# Si no aparece, re-flashear el firmware
```

### Error: "Unknown pin chip name 'eddy'"

**Causa:** MCU del Eddy no definido o no conectado

**Solucion:**
- Verificar seccion `[mcu eddy]` en printer.cfg
- Verificar cable USB conectado
- Reiniciar Klipper

### QUERY_PROBE siempre "triggered"

**Causas:**
1. Eddy Duo demasiado cerca de metal
2. Drive current no calibrado

**Solucion:**
- Mover toolhead lejos de la cama
- Ejecutar `LDC_CALIBRATE_DRIVE_CURRENT CHIP=btt_eddy`

### QUERY_PROBE siempre "open"

**Causas:**
1. Sensor danado
2. Pin incorrecto en configuracion

**Solucion:**
- Verificar `sensor_pin: eddy:gpio26`
- Probar acercar metal al sensor

### Lecturas inconsistentes / derivas

**Causas:**
1. Temperatura variable
2. Montaje con vibracion

**Solucion:**
- Habilitar compensacion termica
- Apretar montaje del Eddy Duo
- Esperar estabilizacion termica antes de calibrar

---

## Comparacion: USB vs I2C (para futuro)

Cuando instales la EBB42 con Stealthburner (Phase 12+), podras elegir:

| Aspecto | USB (actual) | I2C (futuro) |
|---------|--------------|--------------|
| Cables | 1 USB + montaje | Solo I2C al toolhead |
| Simplicidad | Mayor | Menor (cableado) |
| Independencia | Total | Depende de EBB42 |
| Latencia | Similar | Similar |

**Recomendacion:** Mantener USB hasta Phase 12. Evaluar I2C cuando se instale Stealthburner.

---

## Referencias

- [Klipper Eddy Probe Documentation](https://www.klipper3d.org/Eddy_Probe.html)
- [BIGTREETECH Eddy GitHub](https://github.com/bigtreetech/Eddy)
- [BTT Wiki - Eddy](https://bttwiki.com/Eddy.html)

---

**Version:** 1.0
**Fecha:** 2026-01-09
**Nota:** Reemplaza Eddy Coil V1.0 que se dano. Eddy Duo elegido por mejor compatibilidad con cama de hierro.
