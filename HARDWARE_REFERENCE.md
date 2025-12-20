# REFERENCIA TÉCNICA COMPLETA: Hardware del Proyecto

**Proyecto:** Migración Tronxy X5SA → Klipper con SKR 1.4 Turbo
**Fecha:** 2025-12-20
**Propósito:** Documento de referencia técnica exhaustivo

---

## ÍNDICE

1. [Tronxy X5SA / X5SA Pro](#tronxy-x5sa--x5sa-pro)
2. [BTT SKR 1.4 Turbo](#btt-skr-14-turbo)
3. [TMC2209 Drivers](#tmc2209-drivers)
4. [BTT EBB42 CAN V1.2](#btt-ebb42-can-v12)
5. [MAX31865 + PT100](#max31865--pt100)
6. [ADXL345](#adxl345)
7. [Omron TL-Q5MC1-Z](#omron-tl-q5mc1-z)
8. [Tablas de Referencia Rápida](#tablas-de-referencia-rápida)

---

## TRONXY X5SA / X5SA PRO

### Especificaciones Mecánicas

| Parámetro | Valor |
|-----------|-------|
| **Cinemática** | CoreXY |
| **Volumen de impresión** | 330×330×400mm |
| **Precisión de posición** | X/Y: 0.00625mm, Z: 0.00125mm |
| **Velocidad impresión** | 20-150 mm/s (recomendado: 60 mm/s) |
| **Resolución capa** | 0.1-0.4mm |
| **Estructura** | Perfil aluminio |
| **Guías** | Lineales (ya instaladas) |

### Sistema Eléctrico Stock

| Componente | Especificación |
|------------|----------------|
| **Voltaje sistema** | 24V DC |
| **Fuente alimentación** | 360W (24V 15A) |
| **Placa controladora** | F-Mini (www.cbd-3d.com) |
| **Drivers stock** | Integrados (ruidosos) |

### Motores Stock

**Configuración CoreXY:**
- **X/Y:** 2× motores NEMA17 trabajando en tándem
- **Corriente típica:** ~1.1A por motor
- **Característica:** Motores fijos, solo toolhead se mueve

**Eje Z:**
- **Motor:** 1× NEMA17
- **Transmisión:** Tornillo/husillo (lead screw)

**Extrusor:**
- **Motor:** 1× NEMA17
- **Corriente típica:** ~1.05A

### Problemas Conocidos del Stock

❌ **Firmware Chitu:**
- Z-offset inaccesible después de actualizaciones
- Auto-leveling defectuoso (crash contra cama)
- Configuración limitada
- Bugs sin resolver

❌ **Calidad de componentes:**
- Drivers ruidosos
- Termistores de baja calidad
- Cableado sin etiquetar
- Configuración pobre de fábrica

### Configuraciones Klipper de Referencia

**Oficial:**
- [Klipper config X5SA Pro 2020](https://github.com/Klipper3d/klipper/blob/master/config/printer-tronxy-x5sa-pro-2020.cfg)
- [Klipper config X5SA V6 2019](https://github.com/Klipper3d/klipper/blob/master/config/printer-tronxy-x5sa-v6-2019.cfg)

**Comunitarias:**
- [markcarroll/tronxy-x5sa](https://github.com/markcarroll/tronxy-x5sa)
- [Darkwulf183/Tronxy-X5SA](https://github.com/Darkwulf183/Tronxy-X5SA-Printer.cfg-and-more-for-Klipper)

---

## BTT SKR 1.4 TURBO

### MCU y Especificaciones

| Característica | Valor |
|----------------|-------|
| **MCU** | NXP LPC1769FBD100 |
| **Arquitectura** | ARM Cortex-M3 32-bit |
| **Frecuencia** | 120 MHz (vs 100 MHz en SKR 1.4) |
| **Flash** | 512 KB |
| **RAM** | 64 KB |
| **Voltaje entrada** | DC 12-24V |
| **Corriente máxima** | 5-15A |
| **Voltaje lógico** | 3.3V |

### Drivers Stepper

**Sockets disponibles:** 5 (X, Y, Z, E0, E1)

**Drivers compatibles:**
- TMC5161, TMC5160
- TMC2209, TMC2225, TMC2208, TMC2130
- ST820, LV8729
- DRV8825, A4988

**Microstepping:** Hasta 256 subdivisiones (según driver)

### Configuración Jumpers UART (TMC2209)

**Por cada eje (5 ejes × 2 jumpers = 10 total):**

```
Posición jumpers debajo de cada socket:

[MS0]  [MS1]  [MS2]
  ✅     ✅     ❌

MS0: Jumper insertado (izquierda)
MS1: Jumper insertado (centro)
MS2: VACÍO (derecha)
```

### Pinout Completo SKR 1.4 Turbo

#### Drivers Stepper

| Eje | STEP | DIR | ENABLE | UART/CS | ENDSTOP |
|-----|------|-----|--------|---------|---------|
| **X** | P2.2 | P2.6 | !P2.1 | P1.10 | !P1.29 (P1.28 conector) |
| **Y** | P0.19 | P0.20 | !P2.8 | P1.9 | !P1.28 (P1.26 conector) |
| **Z** | P0.22 | !P2.11 | !P0.21 | P1.8 | !P1.27 (P1.25 conector) |
| **E0** | P2.13 | !P0.11 | !P2.12 | P1.4 | - |
| **E1** | P1.15 | !P1.14 | !P1.16 | P1.1 | - |

**Nota:** `!` indica activo bajo

#### Heaters

| Salida | Pin | Uso |
|--------|-----|-----|
| **HE0** | P2.7 | Hotend principal |
| **HE1** | P2.4 | Segundo hotend / Auxiliar |
| **HB** | P2.5 | Cama caliente (≤144W) |

#### Thermistors

| Entrada | Pin | Uso |
|---------|-----|-----|
| **TH0** | P0.24 | Termistor hotend |
| **TH1** | P0.23 | Termistor segundo hotend |
| **TB** | P0.25 | Termistor cama |

#### Ventiladores

| Salida | Pin | Uso típico |
|--------|-----|------------|
| **Fan** | P2.3 | Part cooling |
| **HE1** | P2.4 | Hotend fan |

#### Otros

| Función | Pin | Notas |
|---------|-----|-------|
| **Probe** | P0.10 | BLTouch / sensor |
| **Neopixel** | P1.24 | WS2812 |
| **Buzzer** | P1.30 | Alarma |

### Flasheo Klipper

**Menuconfig:**
```
[*] Enable extra low-level configuration options
    Micro-controller Architecture (LPC176x)
    Processor model (lpc1769 (120 Mhz))  ← CRÍTICO
[*] Target board uses Smoothieware bootloader
```

**Proceso:**
1. `make menuconfig` con configuración anterior
2. `make clean && make`
3. Renombrar `out/klipper.bin` → `firmware.bin`
4. Copiar a SD card
5. Insertar SD en SKR y encender
6. Archivo se renombra a `firmware.cur` si exitoso

### Limitaciones Conocidas

⚠️ **Cama caliente:** Máximo 144W (1Ω @ 12A). Para más potencia, usar MOSFET externo.

⚠️ **Regulador 5V:** Inestable para Neopixels largos. Usar fuente externa.

⚠️ **Socket E0:** Más propenso a fallos según reportes. Workaround: remap a E1.

---

## TMC2209 DRIVERS

### Especificaciones Técnicas

| Parámetro | Valor |
|-----------|-------|
| **Fabricante** | Trinamic / BigTreeTech |
| **Corriente máxima** | 1.2-1.4A pico, 800-1200mA RMS |
| **Voltaje** | 4.75V - 29V |
| **Microstepping** | Hasta 256 |
| **Interfaz** | UART / STEP/DIR |
| **Resistencia sensado** | 0.110Ω (SKR 1.4) |

### Modos de Operación

**StealthChop:**
- Silencioso (inaudible)
- Baja velocidad (<120 mm/s típico)
- Menor torque

**SpreadCycle:**
- Más ruidoso
- Alta velocidad
- Mayor torque

**Klipper maneja automáticamente** con `stealthchop_threshold`

### Configuración Klipper (Ejemplo Eje X)

```ini
[tmc2209 stepper_x]
uart_pin: P1.10
run_current: 0.800      # 800mA RMS
hold_current: 0.500     # 500mA en reposo (opcional)
stealthchop_threshold: 999999  # Siempre StealthChop
```

### Valores Típicos de Corriente

| Componente | run_current | hold_current |
|------------|-------------|--------------|
| **X/Y (CoreXY)** | 650-900mA | 400-500mA |
| **Z** | 580-700mA | 300-400mA |
| **Extrusor** | 650-900mA | 300-500mA |

**Regla:** Configurar 10-20% debajo del máximo del motor para evitar sobrecalentamiento.

### Orientación Física

**Identificar PIN 1:**
- Buscar círculo/marca en esquina del chip TMC2209
- Alinear con marca del socket (triángulo/esquina recortada)

**⚠️ CRÍTICO:** Inserción incorrecta destruye el driver permanentemente.

### Troubleshooting

**Error: "Unable to read tmc uart"**

Causas:
1. Jumpers UART mal configurados (verificar MS0+MS1, MS2 vacío)
2. Pin UART incorrecto en config
3. Driver mal insertado / orientación incorrecta
4. Driver defectuoso

Solución:
1. Apagar 30 segundos
2. Verificar jumpers
3. Verificar pins en config
4. Probar en otro socket (ej: E1)
5. Usar `M122` para debug

---

## BTT EBB42 CAN V1.2

### Especificaciones

| Característica | Valor |
|----------------|-------|
| **MCU** | STM32G0B1CBT6 ARM Cortex-M0+ @ 64MHz |
| **Dimensiones** | 40mm × 40mm |
| **Montaje** | 31mm × 31mm, M3 |
| **Voltaje entrada** | DC 12-24V, máx 9A |
| **Calentador (E0)** | Máx 5A |
| **Ventiladores** | 1A por canal, pico 1.5A |
| **Comunicación** | CAN Bus / USB Type-C |

### Componentes Integrados

- ✅ TMC2209 (extrusor) en modo UART
- ✅ ADXL345 acelerómetro
- ✅ MAX31865 para PT100/PT1000
- ✅ Conectores I2C, RGB, Probe

### Pinout EBB42 V1.2

| Función | Pin | Notas |
|---------|-----|-------|
| **ADXL345 CS** | PB12 | |
| **ADXL345 SCLK** | PB10 | |
| **ADXL345 MISO** | PB2 | |
| **ADXL345 MOSI** | PB11 | |
| **TMC2209 STEP** | PD0 | Extrusor |
| **TMC2209 DIR** | PD1 | |
| **TMC2209 EN** | PD2 | |
| **TMC2209 UART** | PA15 | Dirección 00 |
| **Heater H0** | PB13 | ⚠️ Era PA2 en V1.1 |
| **Temp T0** | PA3 | |
| **Fan0** | PA0 | Part cooling |
| **Fan1** | PA1 | Hotend fan |
| **Probe Signal** | PB8 | |
| **Probe Control** | PB9 | BLTouch |
| **CAN_RX** | PB0 | |
| **CAN_TX** | PB1 | |
| **Status LED** | PA13 | |

### CAN Bus vs USB

**Ventajas CAN:**
- ✅ Solo 4 cables (CAN_H, CAN_L, 24V, GND)
- ✅ Reducción 80-85% cableado
- ✅ Inmunidad ruido electromagnético
- ✅ Daisy-chain múltiples dispositivos
- ✅ Tolerancia fallos

**Ventajas USB:**
- ✅ Mayor velocidad (480 Mbps vs 1 Mbps)
- ✅ Menor latencia
- ✅ Setup más simple
- ✅ No requiere transceptor CAN

**Recomendación para toolhead móvil:** CAN Bus

### Configuración Klipper

```ini
[mcu EBBCan]
canbus_uuid: <obtener con canbus_query.py>
canbus_interface: can0

[extruder]
step_pin: EBBCan:PD0
dir_pin: EBBCan:PD1
enable_pin: !EBBCan:PD2
# ... resto de configuración

[tmc2209 extruder]
uart_pin: EBBCan:PA15
run_current: 0.650
stealthchop_threshold: 999999
```

---

## MAX31865 + PT100

### ¿Qué es un PT100?

**PT100 = Sensor RTD (Resistance Temperature Detector) de platino**

- Resistencia: 100Ω @ 0°C
- Principio: Resistencia del platino aumenta linealmente con temperatura
- Alternativa: PT1000 (1000Ω @ 0°C)

### Ventajas vs Termistores

| Característica | PT100 | Termistor |
|----------------|-------|-----------|
| **Precisión** | ±1°C @ 300°C | ±2-10°C |
| **Rango** | -200°C a +850°C | -55°C a +450°C |
| **Linealidad** | Casi lineal | No lineal |
| **Estabilidad** | Excelente | Se degrada |
| **Intercambiabilidad** | Sí (Clase A/B) | No |
| **Costo** | $15-30 | $2-5 |

### MAX31865: Funcionamiento

El MAX31865 es un **convertidor resistencia-digital** con:

- ADC 15 bits
- Resolución: 0.03125°C
- Compensación de cables
- Detección de fallos (cable abierto, cortocircuito)
- Interfaz SPI

### Cableado: 2/3/4 Hilos

**2-Wire:**
- Error: +2.5-10°C (resistencia de cables)
- Uso: Solo cables <30cm

**3-Wire (RECOMENDADO):**
- Error: <1°C
- Compensa resistencia si cables balanceados
- Uso: Impresión 3D estándar

**4-Wire:**
- Error: Mínimo
- Máxima precisión
- Uso: Aplicaciones críticas

### Configuración Klipper

```ini
[extruder]
sensor_type: MAX31865
sensor_pin: EBBCan:PA4
spi_software_sclk_pin: EBBCan:PA5
spi_software_mosi_pin: EBBCan:PA7
spi_software_miso_pin: EBBCan:PA6
rtd_nominal_r: 100          # 100 para PT100, 1000 para PT1000
rtd_reference_r: 430        # Verificar en tu placa
rtd_num_of_wires: 3         # 2, 3 o 4
rtd_use_50Hz_filter: True   # True=50Hz (Europa), False=60Hz (América)
```

**Proceso:**
1. Conectar PT100 según cableado elegido
2. Configurar `printer.cfg`
3. Verificar lectura temperatura ambiente
4. `PID_CALIBRATE HEATER=extruder TARGET=200`
5. `SAVE_CONFIG`

---

## ADXL345

### Propósito

Acelerómetro MEMS de 3 ejes para:

**Input Shaper:**
- Mide vibraciones del sistema
- Identifica frecuencias de resonancia
- Klipper aplica compensación
- **Resultado:** 2-3× más velocidad sin ghosting

### Conexión: SOLO SPI

**⚠️ CRÍTICO:** En Klipper, I2C NO funciona con ADXL345.

**Razón:** Throughput insuficiente para 3200 Hz sampling rate.

**Conexión SPI obligatoria:**
```
RPi / MCU          ADXL345
─────────────      ─────────
3.3V       ────────  VCC
GND        ────────  GND
MOSI       ────────  SDA/SDI
MISO       ────────  SDO
SCLK       ────────  SCL
GPIO (CS)  ────────  CS
```

### Montaje Físico

**Ubicación óptima:** Lo más cerca del nozzle

**⚠️ AISLAMIENTO OBLIGATORIO:**
- NUNCA tocar partes metálicas (ground loop destruye electrónica)
- Usar montaje plástico
- Verificar continuidad con multímetro

**Orientación:** Marcar ejes X, Y, Z del sensor vs impresora

### Proceso de Medición

```bash
# 1. Verificar conexión
ACCELEROMETER_QUERY

# 2. Medir eje X
TEST_RESONANCES AXIS=X

# 3. Medir eje Y
TEST_RESONANCES AXIS=Y

# 4. Generar gráficos
~/klipper/scripts/calibrate_shaper.py /tmp/resonances_x_*.csv -o /tmp/shaper_calibrate_x.png
~/klipper/scripts/calibrate_shaper.py /tmp/resonances_y_*.csv -o /tmp/shaper_calibrate_y.png

# 5. Aplicar valores recomendados
```

### Configuración Klipper (EBB42)

```ini
[adxl345]
cs_pin: EBBCan:PB12
spi_software_sclk_pin: EBBCan:PB10
spi_software_mosi_pin: EBBCan:PB11
spi_software_miso_pin: EBBCan:PB2
axes_map: x,y,z

[resonance_tester]
accel_chip: adxl345
probe_points:
    150,150,20  # Centro de cama

[input_shaper]
# Valores después de TEST_RESONANCES
#shaper_freq_x: 54.2
#shaper_type_x: mzv
#shaper_freq_y: 42.8
#shaper_type_y: ei
```

---

## OMRON TL-Q5MC1-Z

### Especificaciones

| Parámetro | Valor |
|-----------|-------|
| **Tipo** | Inductivo rectangular |
| **Configuración** | DC 3-wire, NPN, NO |
| **Voltaje** | 10-30 VDC |
| **Corriente salida** | 100 mA máx |
| **Distancia sensado** | 5mm ±10% |
| **Superficie** | Metales ferrosos |
| **Repetibilidad** | ±0.01mm (10 micrones) |
| **Dimensiones** | 28×17×17mm |
| **Cable** | 2 metros |
| **Protección** | IP66 |

### Cableado NPN NO

```
TL-Q5MC1-Z          EBB42 / SKR
────────────        ────────────
Marrón (+)   ─────> 24V
Azul (-)     ─────> GND
Negro (señal)─────> Pin sensor (con pullup)
```

**Funcionamiento:**
- **SIN metal:** Señal flotante (pullup → 3.3V) → "OPEN"
- **CON metal:** Señal conecta a GND → LED rojo ON → "TRIGGERED"

### Configuración Klipper

```ini
[probe]
pin: ^EBBCan:PB8  # ^ activa pullup (necesario para NPN NO)
x_offset: 0
y_offset: 25.0    # Medir: distancia sensor → nozzle
#z_offset: 0      # Calibrar con PROBE_CALIBRATE
speed: 5
samples: 2
sample_retract_dist: 2.0
samples_result: average
samples_tolerance: 0.05
samples_tolerance_retries: 3
```

### Calibración

**X/Y Offset:**
1. Mover nozzle a posición conocida
2. Bajar hasta tocar papel
3. Marcar con lápiz
4. `PROBE` para medir
5. Calcular diferencia

**Z Offset:**
```bash
G28
PROBE_CALIBRATE
# Ajustar con TESTZ Z=...
ACCEPT
SAVE_CONFIG
```

**Verificación:**
```bash
PROBE_ACCURACY
```

Resultados esperados:
- Range: <0.050mm
- Desviación: <0.010mm

---

## TABLAS DE REFERENCIA RÁPIDA

### Voltajes del Sistema

| Componente | Voltaje |
|------------|---------|
| PSU principal | 24V DC |
| Lógica MCU | 3.3V |
| Cama caliente | 24V |
| Motores | 24V |
| Ventiladores | 24V (usar DC-DC si 12V) |

### Corrientes Típicas TMC2209

| Motor | run_current | hold_current |
|-------|-------------|--------------|
| X | 800mA | 500mA |
| Y | 800mA | 500mA |
| Z | 650mA | 400mA |
| E0 | 650mA | 300mA |

### Jumpers UART (TMC2209)

```
Por eje:
MS0: ✅ Jumper
MS1: ✅ Jumper
MS2: ❌ VACÍO

Total: 10 jumpers (5 ejes × 2)
```

### Pines SKR 1.4 Turbo (Resumen)

| Eje | STEP | DIR | UART |
|-----|------|-----|------|
| X | P2.2 | P2.6 | P1.10 |
| Y | P0.19 | P0.20 | P1.9 |
| Z | P0.22 | !P2.11 | P1.8 |
| E0 | P2.13 | !P0.11 | P1.4 |
| E1 | P1.15 | !P1.14 | P1.1 |

### Pines EBB42 V1.2 (Resumen)

| Función | Pin |
|---------|-----|
| Extrusor STEP | PD0 |
| Extrusor DIR | PD1 |
| Extrusor UART | PA15 |
| Heater | PB13 |
| Temp | PA3 |
| Fan0 | PA0 |
| Fan1 | PA1 |
| Probe | PB8 |
| ADXL CS | PB12 |

---

## FUENTES Y REFERENCIAS

### Tronxy X5SA
- [X5SA Official Store](https://www.tronxy3d.com/products/x5sa-diy-3d-printer-kit)
- [X5SA Review All3DP](https://all3dp.com/1/2020-tronxy-x5sa-3d-printer-review-the-specs/)
- [Klipper X5SA Pro Config](https://github.com/Klipper3d/klipper/blob/master/config/printer-tronxy-x5sa-pro-2020.cfg)

### BTT SKR 1.4 Turbo
- [SKR V1.4 GitHub](https://github.com/bigtreetech/BIGTREETECH-SKR-V1.3/tree/master/BTT%20SKR%20V1.4)
- [SKR Pinout PDF](https://github.com/bigtreetech/BIGTREETECH-SKR-V1.3/blob/master/BTT%20SKR%20V1.4/Hardware/BTT%20SKR%20V1.4PIN.pdf)
- [BTT Wiki](https://bttwiki.com/SKR%20V1.4.html)

### TMC2209
- [TMC Drivers Klipper](https://www.klipper3d.org/TMC_Drivers.html)
- [Complete Guide SKR TMC2209](https://3dwork.io/en/complete-guide-skr-v1-4-and-tmc2209/)

### EBB42
- [EBB GitHub](https://github.com/bigtreetech/EBB)
- [BTT Wiki EBB Series](https://global.bttwiki.com/EBB%20Series.html)
- [RatOS EBB42 Docs](https://os.ratrig.com/docs/toolboards/btt/ebb-42-12/)

### MAX31865 / PT100
- [Voron PT100 Guide](https://docs.vorondesign.com/community/electronics/xbst_/PT100.html)
- [Adafruit MAX31865](https://learn.adafruit.com/adafruit-max31865-rtd-pt100-amplifier/rtd-wiring-config)
- [DYZE Temperature Sensors Comparison](https://dyzedesign.com/2016/09/comparison-between-temperature-sensors-used-in-3d-printers-part-2/)

### ADXL345 / Input Shaper
- [Klipper Measuring Resonances](https://www.klipper3d.org/Measuring_Resonances.html)
- [Input Shaper Complete Guide](https://thinkrobotics.com/blogs/learn/input-shaper-tuning-with-klipper-complete-calibration-guide)

### Omron TL-Q5MC1-Z
- [TL-Q Series Datasheet](https://docs.rs-online.com/147d/0900766b812c998e.pdf)

---

**Última actualización:** 2025-12-20
**Versión:** 1.0
