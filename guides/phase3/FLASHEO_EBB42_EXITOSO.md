# Flasheo Exitoso de BTT EBB42 CAN V1.2 en Modo USB

**Fecha:** 2025-12-21
**Proyecto:** x5sa-skr-klipper - Conversión Tronxy X5SA a Klipper
**Hardware:** BigTreeTech EBB42 CAN V1.2 (STM32G0B1)
**Modo:** USB (NO CAN bus)
**Estado:** ✅ COMPLETADO Y FUNCIONANDO

---

## Resumen Ejecutivo

Este documento detalla el proceso completo de flasheo de la placa EBB42 CAN V1.2 en modo USB, incluyendo problemas encontrados y sus soluciones. La EBB42 ahora funciona correctamente junto con la SKR 1.4 Turbo en una configuración dual-MCU de Klipper.

**Resultado Final:** Ambas MCUs (SKR 1.4 Turbo + EBB42) detectadas y funcionando en Klipper.

---

## 1. Problema Inicial

### 1.1 Síntomas
- La EBB42 no era detectada por `lsusb` cuando solo estaba conectada por USB
- Después del flasheo inicial con DFU, la placa no arrancaba automáticamente
- La placa permanecía en modo DFU después del flasheo
- No aparecía como dispositivo Klipper en `/dev/serial/by-id/`

### 1.2 Hardware Afectado
- **Placa:** BTT EBB42 CAN V1.2
- **MCU:** STM32G0B1CBT6
- **Configuración:** Modo USB (sin CAN bus)
- **Jumper VUSB:** Instalado (requerido para modo USB)

---

## 2. Diagnóstico Realizado

### 2.1 Verificación de Hardware

#### Cable USB
```bash
# Verificar conexión física
lsusb
# La EBB42 NO aparecía sin alimentación 24V
```

**Conclusión:** Cable USB funcionando correctamente, pero insuficiente.

#### Jumper VUSB
- **Estado:** Instalado ✅
- **Ubicación:** Entre pines VUSB en la placa EBB42
- **Propósito:** Permite que la placa use alimentación USB para comunicación
- **Crítico para:** Modo USB (sin este jumper, solo funciona modo CAN)

### 2.2 Descubrimiento Crítico: Alimentación 24V

**Hallazgo clave:** La EBB42 requiere alimentación 24V externa para funcionar, incluso en modo USB.

**Por qué:**
- El USB solo proporciona alimentación para la lógica de comunicación
- Los drivers de motor, ventiladores y otros periféricos requieren 24V
- La MCU necesita la alimentación principal para inicializar completamente
- Sin 24V, la placa entra en un estado de bajo consumo y no se detecta

**Evidencia:**
```bash
# Sin 24V
lsusb  # No muestra la EBB42

# Con 24V conectado
lsusb  # Muestra: Bus 001 Device 0XX: ID 0483:df11 STMicroelectronics STM Device in DFU Mode
```

### 2.3 Problema de Arranque Post-DFU

**Síntoma:** Después de flashear con `dfu-util`, la placa permanecía en modo DFU.

**Causa raíz:**
- Cuando se flashea sin bootloader (offset 0x08000000), el firmware se escribe directamente en la flash
- El proceso DFU no reinicia automáticamente la placa en modo aplicación
- La placa espera un reset manual para salir del modo DFU y ejecutar Klipper

**Solución:** Reset manual presionando el botón RST (sin BOOT) después del flasheo.

---

## 3. Proceso de Solución Paso a Paso

### 3.1 Preparación del Entorno

#### Instalación de Herramientas
```bash
# Instalar dfu-util (si no está instalado)
sudo apt-get update
sudo apt-get install dfu-util

# Verificar instalación
dfu-util --version
```

#### Navegación al Directorio de Klipper
```bash
cd ~/klipper
```

### 3.2 Configuración del Firmware

#### Comando make menuconfig
```bash
make menuconfig
```

#### Configuración Exacta (CRÍTICA)

```
[*] Enable extra low-level configuration options
    Micro-controller Architecture (STMicroelectronics STM32)  --->
    Processor model (STM32G0B1)  --->
    Bootloader offset (No bootloader)  --->
    Clock Reference (8 MHz crystal)  --->
    Communication interface (USB (on PA11/PA12))  --->
    USB ids  --->
```

**Desglose de cada opción:**

1. **Enable extra low-level configuration options**
   - Permite acceder a opciones avanzadas de configuración
   - Necesario para seleccionar el offset del bootloader

2. **Micro-controller Architecture: STM32**
   - La EBB42 V1.2 usa un chip STM32

3. **Processor model: STM32G0B1**
   - Modelo exacto del MCU en la EBB42 V1.2
   - **IMPORTANTE:** No confundir con G0B0 (modelo anterior)

4. **Bootloader offset: No bootloader** ⚠️ CRÍTICO
   - Flash directo a offset 0x08000000
   - Sin CanBoot ni otro bootloader intermedio
   - Requiere DFU mode para actualizar firmware
   - **Por qué esta opción:**
     * Simplifica el proceso de flasheo inicial
     * No requiere configurar CanBoot
     * Acceso directo a modo DFU con botones BOOT+RST

5. **Clock Reference: 8 MHz crystal**
   - Cristal externo de 8 MHz en la placa EBB42
   - Proporciona timing preciso para steppers

6. **Communication interface: USB (on PA11/PA12)**
   - Pines PA11 (D-) y PA12 (D+) del USB
   - **NO CAN bus** para esta configuración
   - Permite conexión directa a Raspberry Pi

#### Guardar Configuración
- Presionar `Q` para salir
- Confirmar con `Y` para guardar

### 3.3 Compilación del Firmware

```bash
make clean
make
```

**Resultado esperado:**
```
  Creating hex file out/klipper.bin
```

**Archivo generado:** `~/klipper/out/klipper.bin`

### 3.4 Conexión de Alimentación 24V

#### CRÍTICO: Alimentación Necesaria

**Cable utilizado:**
- **Calibre:** AWG 20 o AWG 18 (mínimo)
- **Longitud:** Lo más corto posible (< 1 metro recomendado)
- **Conectores:** Fork terminals o conectores apropiados

**Conexión:**
```
PSU 24V (+) ──────> EBB42 VIN (+)
PSU 24V (-) ──────> EBB42 GND (-)
```

**Puntos de conexión en la EBB42:**
- Terminal VIN: Entrada de alimentación principal
- Terminal GND: Tierra común

**Verificación de seguridad:**
1. ✅ Verificar polaridad correcta (VIN=+, GND=-)
2. ✅ Cable bien apretado en terminales
3. ✅ No hay cortocircuitos
4. ✅ PSU configurada a 24V (no 12V)
5. ✅ Jumper VUSB instalado

**Indicadores visuales:**
- LED de alimentación en la EBB42 debe encender
- Si no hay LED, verificar conexiones

### 3.5 Entrada a Modo DFU

#### Procedimiento de Botones

**Secuencia exacta:**
1. Con la placa apagada (sin USB ni 24V)
2. Mantener presionado el botón **BOOT**
3. Mantener presionado el botón **RST** (mientras BOOT sigue presionado)
4. Conectar alimentación 24V
5. Conectar cable USB a Raspberry Pi
6. Soltar el botón **RST**
7. Soltar el botón **BOOT**

**Ubicación de botones en EBB42 V1.2:**
```
┌─────────────────┐
│                 │
│  [BOOT]  [RST]  │  ← Esquina de la placa
│                 │
│     EBB42       │
│                 │
└─────────────────┘
```

#### Verificación de Modo DFU

```bash
lsusb
```

**Salida esperada:**
```
Bus 001 Device 0XX: ID 0483:df11 STMicroelectronics STM Device in DFU Mode
```

**Si no aparece:**
- Repetir secuencia de botones
- Verificar alimentación 24V
- Verificar cable USB
- Probar otro puerto USB

```bash
# Verificar detalles DFU
dfu-util -l
```

**Salida esperada:**
```
Found DFU: [0483:df11] ver=2200, devnum=XX, cfg=1, intf=0, path="X-X", alt=1, name="@Option Bytes  /0x1FFF7800/01*016 e", serial="XXXXXXXXXX"
Found DFU: [0483:df11] ver=2200, devnum=XX, cfg=1, intf=0, path="X-X", alt=0, name="@Internal Flash  /0x08000000/128*02Kg", serial="XXXXXXXXXX"
```

**Importante:** Debe aparecer `@Internal Flash /0x08000000`

### 3.6 Flasheo del Firmware

#### Comando de Flasheo

```bash
sudo dfu-util -a 0 -d 0483:df11 --dfuse-address 0x08000000 -D ~/klipper/out/klipper.bin
```

**Desglose del comando:**
- `sudo`: Permisos de administrador
- `dfu-util`: Herramienta de flasheo DFU
- `-a 0`: Alternate setting 0 (Internal Flash)
- `-d 0483:df11`: Vendor:Product ID del dispositivo STM32 en DFU
- `--dfuse-address 0x08000000`: Dirección de inicio de flash (sin bootloader)
- `-D ~/klipper/out/klipper.bin`: Archivo de firmware a flashear

**Por qué 0x08000000:**
- Es la dirección base de la flash interna del STM32
- Al no usar bootloader, el firmware Klipper inicia directamente aquí
- Coincide con la configuración "No bootloader" de make menuconfig

#### Salida del Comando

```
dfu-util 0.9

Copyright 2005-2009 Weston Schmidt, Harald Welte and OpenMoko Inc.
Copyright 2010-2016 Tormod Volden and Stefan Schmidt
This program is Free Software and has ABSOLUTELY NO WARRANTY
Please report bugs to http://sourceforge.net/p/dfu-util/tickets/

dfu-util: Invalid DFU suffix signature
dfu-util: A valid DFU suffix will be required in a future dfu-util release!!!
Opening DFU capable USB device...
ID 0483:df11
Run-time device DFU version 011a
Claiming USB DFU Interface...
Setting Alternate Setting #0 ...
Determining device status: state = dfuIDLE, status = 0
dfuIDLE, continuing
DFU mode device DFU version 011a
Device returned transfer size 2048
DfuSe interface name: "Internal Flash  "
Downloading to address = 0x08000000, size = 27648
Download	[=========================] 100%        27648 bytes
Download done.
File downloaded successfully
Transitioning to dfuMANIFEST state
```

**Indicadores de éxito:**
- ✅ `Download [=========================] 100%`
- ✅ `File downloaded successfully`
- ✅ `Transitioning to dfuMANIFEST state`

**Advertencias esperadas (no son errores):**
- `Invalid DFU suffix signature` - Normal con archivos .bin de Klipper
- `A valid DFU suffix will be required in future` - Advertencia informativa

### 3.7 Reset Manual (CRÍTICO)

#### Por Qué Es Necesario

Después del flasheo DFU sin bootloader, la placa NO arranca automáticamente porque:
1. El modo DFU no ejecuta reset automático
2. No hay bootloader que detecte el fin del flasheo
3. La MCU permanece en estado DFU esperando más comandos

**Solución:** Reset manual para arrancar el firmware Klipper recién flasheado.

#### Procedimiento de Reset

**Secuencia exacta:**
1. Verificar que el flasheo completó exitosamente
2. **Presionar y soltar el botón RST** (sin tocar BOOT)
3. Esperar 2-3 segundos

**IMPORTANTE:**
- ❌ NO presionar BOOT durante este reset
- ✅ Solo presionar RST brevemente
- ❌ NO desconectar alimentación
- ✅ Mantener USB y 24V conectados

#### Verificación Post-Reset

```bash
# Esperar 2-3 segundos después del reset
lsusb
```

**Antes del reset (modo DFU):**
```
Bus 001 Device 0XX: ID 0483:df11 STMicroelectronics STM Device in DFU Mode
```

**Después del reset (Klipper funcionando):**
```
Bus 001 Device 0XX: ID 1d50:614e OpenMoko, Inc. stm32g0b1xx
```

**Cambio clave:**
- ID cambió de `0483:df11` (DFU) a `1d50:614e` (Klipper)
- Descripción cambió a `stm32g0b1xx`

### 3.8 Verificación de Detección USB

#### Listar Dispositivos Seriales

```bash
ls /dev/serial/by-id/
```

**Salida esperada:**
```
usb-Klipper_lpc1769_12345-if00      # SKR 1.4 Turbo
usb-Klipper_stm32g0b1xx_12345-if00  # EBB42 CAN V1.2
```

**Ambas MCUs deben aparecer.**

#### Verificar Permisos

```bash
ls -l /dev/serial/by-id/usb-Klipper_stm32g0b1xx_12345-if00
```

**Salida esperada:**
```
lrwxrwxrwx 1 root root 13 Dec 21 XX:XX /dev/serial/by-id/usb-Klipper_stm32g0b1xx_12345-if00 -> ../../ttyACM1
```

#### Obtener Serial ID Exacto

```bash
ls /dev/serial/by-id/ | grep stm32g0b1
```

**Este es el ID que usarás en printer.cfg para [mcu EBBCan]**

---

## 4. Configuración de printer.cfg

### 4.1 Estructura Dual-MCU

```ini
# ============================================
# MCU Principal: BTT SKR 1.4 Turbo (LPC1769)
# ============================================
[mcu]
serial: /dev/serial/by-id/usb-Klipper_lpc1769_12345-if00

# ============================================
# MCU Secundaria: BTT EBB42 CAN V1.2 (STM32G0B1)
# Modo: USB (NO CAN bus)
# ============================================
[mcu EBBCan]
serial: /dev/serial/by-id/usb-Klipper_stm32g0b1xx_12345-if00
```

### 4.2 Uso de la EBB42 en Configuración

**Ejemplo: Extrusor en EBB42**
```ini
[extruder]
step_pin: EBBCan:PD0
dir_pin: EBBCan:PD1
enable_pin: !EBBCan:PD2
microsteps: 16
rotation_distance: 33.500
nozzle_diameter: 0.400
filament_diameter: 1.750
heater_pin: EBBCan:PB13
sensor_type: EPCOS 100K B57560G104F
sensor_pin: EBBCan:PA3
control: pid
pid_Kp: 22.2
pid_Ki: 1.08
pid_Kd: 114
min_temp: 0
max_temp: 250
```

**Ejemplo: Ventilador de capa en EBB42**
```ini
[fan]
pin: EBBCan:PA1
```

**Sintaxis:** `EBBCan:PIN` para referirse a pines en la MCU secundaria.

### 4.3 Verificación en Klipper

```bash
# Reiniciar Klipper
sudo systemctl restart klipper

# Ver estado
sudo systemctl status klipper

# Ver logs
tail -f /tmp/klippy.log
```

**En los logs, buscar:**
```
Starting Klipper...
Start printer at ...
Loaded MCU 'mcu' 110 commands (...)
Loaded MCU 'EBBCan' 110 commands (...)
```

**Ambas MCUs deben cargar comandos exitosamente.**

---

## 5. Resultado Final

### 5.1 Estado del Sistema

**Hardware conectado y funcionando:**
- ✅ BTT SKR 1.4 Turbo (MCU principal - LPC1769)
- ✅ BTT EBB42 CAN V1.2 (MCU secundaria - STM32G0B1)
- ✅ Ambas comunicando por USB
- ✅ Ambas detectadas en Klipper

### 5.2 Verificación Visual

```bash
# Verificar ambas MCUs en lsusb
lsusb | grep Klipper
```

**Salida:**
```
Bus 001 Device 0XX: ID 1d50:614e OpenMoko, Inc. lpc1769
Bus 001 Device 0XX: ID 1d50:614e OpenMoko, Inc. stm32g0b1xx
```

### 5.3 Configuración de Conexión

**Topología física:**
```
┌──────────────┐
│ Raspberry Pi │
└──────┬───────┘
       │ USB Hub
       ├───────────> USB ───> SKR 1.4 Turbo (LPC1769)
       │
       └───────────> USB ───> EBB42 V1.2 (STM32G0B1)
                              └─── 24V ← PSU
```

**Alimentación:**
- SKR 1.4: Alimentación propia desde PSU 24V
- EBB42: Alimentación 24V desde PSU (cable dedicado)
- Ambas: Comunicación USB con Raspberry Pi

---

## 6. Lecciones Aprendidas

### 6.1 Alimentación 24V es OBLIGATORIA

**Descubrimiento clave:**
- La EBB42 NO funciona solo con USB
- Requiere 24V para inicializar la MCU
- Sin 24V, no aparece en `lsusb`

**Aplicación:**
- Siempre conectar 24V antes de intentar flashear
- Usar cable grueso (AWG 20/18 mínimo)
- Verificar LED de alimentación encendido

### 6.2 Reset Manual Post-DFU

**Comportamiento descubierto:**
- Después de flashear con DFU sin bootloader, la placa NO arranca automáticamente
- Permanece en modo DFU esperando comandos
- Necesita reset manual para ejecutar Klipper

**Procedimiento correcto:**
1. Flashear con `dfu-util`
2. Esperar mensaje "File downloaded successfully"
3. Presionar botón RST (sin BOOT)
4. Verificar cambio de ID en `lsusb`

### 6.3 Jumper VUSB Crítico

**Función:**
- Conecta la alimentación USB a la lógica de comunicación
- Sin este jumper, la placa solo funciona en modo CAN
- Necesario para modo USB

**Verificación:**
- Antes de flashear, confirmar jumper instalado
- Ubicado entre pines VUSB en la placa

### 6.4 Configuración make menuconfig Precisa

**Opciones críticas:**
- **Processor model:** STM32G0B1 (no G0B0)
- **Bootloader offset:** No bootloader
- **Communication interface:** USB (on PA11/PA12)

**Consecuencias de error:**
- Firmware incompatible no arrancará
- Placa quedará en estado de boot loop
- Necesitará re-flasheo con configuración correcta

### 6.5 Secuencia de Entrada a DFU

**Orden correcto de acciones:**
1. Botones primero (BOOT + RST presionados)
2. Alimentación después (24V + USB)
3. Soltar RST, luego BOOT

**Si se hace mal:**
- Placa puede arrancar en modo normal (Klipper)
- No entrará en DFU mode
- Necesitará reintentar secuencia

---

## 7. Troubleshooting Común

### 7.1 EBB42 No Aparece en lsusb

**Síntoma:**
```bash
lsusb  # No muestra ningún dispositivo STM32
```

**Posibles causas y soluciones:**

1. **Falta alimentación 24V**
   - ✅ Conectar cable 24V desde PSU
   - ✅ Verificar LED de alimentación encendido
   - ✅ Medir voltaje en terminales VIN/GND

2. **Cable USB defectuoso**
   - ✅ Probar otro cable USB
   - ✅ Verificar que sea cable de datos (no solo carga)
   - ✅ Probar otro puerto USB en Raspberry Pi

3. **Jumper VUSB faltante**
   - ✅ Verificar jumper instalado entre pines VUSB
   - ✅ Re-asentar jumper si está flojo

4. **Placa en estado de error**
   - ✅ Desconectar todo (USB + 24V)
   - ✅ Esperar 10 segundos
   - ✅ Reconectar en orden: 24V primero, USB después

### 7.2 No Entra en Modo DFU

**Síntoma:**
```bash
lsusb  # Muestra ID 1d50:614e (Klipper) en vez de 0483:df11 (DFU)
```

**Solución:**

1. **Repetir secuencia de botones correctamente:**
   ```
   1. Mantener BOOT presionado
   2. Presionar y mantener RST (BOOT sigue presionado)
   3. Conectar alimentación 24V
   4. Conectar USB
   5. Soltar RST
   6. Soltar BOOT
   ```

2. **Verificar timing:**
   - Mantener ambos botones al menos 2 segundos
   - No soltar BOOT antes de conectar alimentación

3. **Si persiste:**
   ```bash
   # Desconectar todo
   # Esperar 10 segundos
   # Reintentar secuencia desde paso 1
   ```

### 7.3 Placa No Arranca Después de Flasheo

**Síntoma:**
```bash
# Después de dfu-util
lsusb  # Sigue mostrando ID 0483:df11 (modo DFU)
```

**Causa:**
- Placa no salió de modo DFU automáticamente

**Solución (CRÍTICA):**
```bash
# Presionar botón RST (sin BOOT)
# Esperar 2-3 segundos
lsusb  # Ahora debe mostrar ID 1d50:614e (Klipper)
```

**Si aún no funciona:**
1. Desconectar USB
2. Desconectar 24V
3. Esperar 10 segundos
4. Conectar 24V
5. Conectar USB
6. Presionar RST

### 7.4 Firmware Flashea Pero No Funciona

**Síntoma:**
```bash
lsusb  # Muestra ID 1d50:614e
ls /dev/serial/by-id/  # No aparece usb-Klipper_stm32g0b1xx
```

**Posibles causas:**

1. **Configuración incorrecta en make menuconfig**
   ```bash
   cd ~/klipper
   make clean
   make menuconfig
   # Verificar TODAS las opciones:
   # - Processor: STM32G0B1 (no G0B0)
   # - Bootloader: No bootloader
   # - Communication: USB (on PA11/PA12)
   make
   # Re-flashear
   ```

2. **Firmware corrupto**
   ```bash
   # Verificar tamaño del archivo
   ls -lh ~/klipper/out/klipper.bin
   # Debe ser ~27KB para STM32G0B1

   # Si es diferente, recompilar
   make clean
   make
   ```

3. **Dirección de flasheo incorrecta**
   ```bash
   # Debe usar 0x08000000 para "No bootloader"
   sudo dfu-util -a 0 -d 0483:df11 --dfuse-address 0x08000000 -D ~/klipper/out/klipper.bin
   ```

### 7.5 Una MCU Funciona, Otra No

**Síntoma:**
```bash
ls /dev/serial/by-id/
# Solo aparece una de las dos MCUs
```

**Diagnóstico:**

```bash
# Ver cuál falta
lsusb | grep Klipper

# Si falta EBB42:
# - Verificar alimentación 24V
# - Re-flashear EBB42

# Si falta SKR:
# - Verificar cable USB de SKR
# - Re-flashear SKR
```

**Solución:**
- Flashear la MCU faltante
- Verificar conexiones físicas
- Revisar logs de Klipper: `tail -f /tmp/klippy.log`

### 7.6 Error "Permission Denied" en dfu-util

**Síntoma:**
```bash
dfu-util -a 0 -d 0483:df11 ...
# Error: Cannot open DFU device 0483:df11
# Permission denied
```

**Solución:**

1. **Usar sudo:**
   ```bash
   sudo dfu-util -a 0 -d 0483:df11 --dfuse-address 0x08000000 -D ~/klipper/out/klipper.bin
   ```

2. **O crear regla udev (permanente):**
   ```bash
   sudo nano /etc/udev/rules.d/50-stm32.rules
   ```

   Agregar:
   ```
   SUBSYSTEM=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="df11", MODE="0666"
   ```

   Recargar:
   ```bash
   sudo udevadm control --reload-rules
   sudo udevadm trigger
   ```

### 7.7 Klipper No Detecta Segunda MCU

**Síntoma:**
En `/tmp/klippy.log`:
```
Loaded MCU 'mcu' 110 commands
mcu 'EBBCan': Unable to open serial port
```

**Verificar:**

1. **Serial ID correcto en printer.cfg:**
   ```bash
   ls /dev/serial/by-id/ | grep stm32g0b1
   # Copiar ID exacto a printer.cfg [mcu EBBCan]
   ```

2. **Permisos:**
   ```bash
   ls -l /dev/serial/by-id/usb-Klipper_stm32g0b1xx_*
   sudo usermod -a -G tty pi
   sudo usermod -a -G dialout pi
   # Logout y login
   ```

3. **Reiniciar Klipper:**
   ```bash
   sudo systemctl restart klipper
   tail -f /tmp/klippy.log
   ```

---

## 8. Referencias y Documentación

### 8.1 Documentación Oficial

**BigTreeTech EBB42:**
- [BTT EBB42 CAN V1.2 GitHub](https://github.com/bigtreetech/EBB)
- [Manual de usuario EBB42](https://github.com/bigtreetech/EBB/tree/master/EBB%20CAN%20V1.1%20%28STM32G0B1%29)
- [Pinout y esquemático](https://github.com/bigtreetech/EBB/blob/master/EBB%20CAN%20V1.2/EBB42%20CAN%20V1.2/Hardware)

**Klipper:**
- [Klipper Documentation](https://www.klipper3d.org/)
- [Multi-MCU Configuration](https://www.klipper3d.org/Config_Reference.html#mcu)
- [Bootloader flashing](https://www.klipper3d.org/Bootloaders.html)

**STM32 DFU:**
- [STM32 DFU Mode](https://www.st.com/resource/en/application_note/cd00264379-usb-dfu-protocol-used-in-the-stm32-bootloader-stmicroelectronics.pdf)
- [dfu-util documentation](http://dfu-util.sourceforge.net/)

### 8.2 Issues y Discusiones Relevantes

**GitHub Issues:**
- [EBB42 USB mode setup](https://github.com/bigtreetech/EBB/issues)
- [Klipper STM32G0B1 support](https://github.com/Klipper3d/klipper/issues)

**Foros:**
- [Klipper Discourse - EBB42](https://klipper.discourse.group/)
- [BTT Community](https://www.facebook.com/groups/bigtreetech)

### 8.3 Comandos de Referencia Rápida

**Verificación de hardware:**
```bash
lsusb                                      # Ver dispositivos USB
ls /dev/serial/by-id/                     # Ver seriales Klipper
dfu-util -l                               # Ver dispositivos DFU
```

**Flasheo:**
```bash
cd ~/klipper
make menuconfig                           # Configurar firmware
make clean && make                        # Compilar
sudo dfu-util -a 0 -d 0483:df11 \
  --dfuse-address 0x08000000 \
  -D ~/klipper/out/klipper.bin            # Flashear
```

**Klipper:**
```bash
sudo systemctl restart klipper            # Reiniciar
sudo systemctl status klipper             # Ver estado
tail -f /tmp/klippy.log                   # Ver logs en tiempo real
```

---

## 9. Próximos Pasos

### 9.1 Configuración Pendiente

1. **Definir pinout completo de EBB42:**
   - Extrusor (motor + heater + thermistor)
   - Ventiladores (part cooling + hotend)
   - Sensor de filamento (si aplica)
   - Probe/BLTouch (si aplica)
   - Neopixels/LEDs (si aplica)

2. **Calibración:**
   - PID tuning del extrusor
   - Rotation_distance del extrusor
   - Pressure advance
   - Input shaper (si se usa acelerómetro en EBB42)

3. **Testing:**
   - Verificar movimiento de extrusor
   - Verificar calentamiento
   - Verificar ventiladores
   - Test de impresión con dual-MCU

### 9.2 Documentación Adicional

Ver también:
- `CONFIGURACION_DUAL_MCU.md` - Guía de configuración multi-MCU
- `guides/phase3/EBB42_INTEGRATION.md` - Integración completa de EBB42
- `phases/phase3/README.md` - Resumen de Phase 3

---

## Conclusión

El flasheo de la EBB42 CAN V1.2 en modo USB fue exitoso después de identificar y resolver dos problemas críticos:

1. **Alimentación 24V obligatoria** para que la placa sea detectada
2. **Reset manual necesario** después del flasheo DFU

Ambas MCUs (SKR 1.4 Turbo + EBB42) ahora funcionan correctamente en configuración dual-MCU de Klipper.

**Estado del sistema:** ✅ COMPLETAMENTE FUNCIONAL

---

**Última actualización:** 2025-12-21
**Autor:** Proyecto x5sa-skr-klipper
**Hardware validado:** BTT EBB42 CAN V1.2 (STM32G0B1)
