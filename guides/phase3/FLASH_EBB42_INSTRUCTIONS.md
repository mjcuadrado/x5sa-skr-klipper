# Flashear EBB42 CAN V1.2 con Klipper (Modo USB)

**Fecha:** 2025-12-21
**Hardware:** BTT EBB42 CAN V1.2 (STM32G0B1)

---

## Paso 1: Compilar Firmware para EBB42

### 1.1 Navegar al directorio de Klipper

```bash
cd ~/klipper
```

### 1.2 Limpiar compilación anterior

```bash
make clean
```

### 1.3 Abrir menú de configuración

```bash
make menuconfig
```

### 1.4 Configuración para EBB42 (Modo USB)

**Selecciona estas opciones en el menú:**

```
[*] Enable extra low-level configuration options
    Micro-controller Architecture (STMicroelectronics STM32)  --->
    Processor model (STM32G0B1)  --->
    Bootloader offset (8KiB bootloader)  --->
    Clock Reference (8 MHz crystal)  --->
    Communication interface (USB (on PA11/PA12))  --->
    USB ids  --->
```

**Explicación:**
- **Micro-controller:** STM32 (familia del chip)
- **Processor model:** STM32G0B1 (chip en EBB42 V1.2)
- **Bootloader offset:** 8KiB bootloader (EBB42 viene con CanBoot/Katapult)
- **Clock:** 8 MHz crystal
- **Communication:** USB on PA11/PA12 (NO seleccionar CAN)

**IMPORTANTE:** Asegúrate de seleccionar **USB**, NO CAN bus.

### 1.5 Guardar configuración y salir

- Presiona `Q` para salir
- Confirma con `Y` para guardar

### 1.6 Compilar firmware

```bash
make
```

**Resultado esperado:**
```
  Creating hex file out/klipper.bin
```

---

## Paso 2: Flashear EBB42 vía DFU Mode

### Método A: Usando DFU Mode (Recomendado para primera vez)

#### 2.1 Instalar herramientas DFU (si no están)

```bash
sudo apt-get update
sudo apt-get install dfu-util
```

#### 2.2 Poner EBB42 en DFU Mode

**Opción 1: Con botón BOOT**
1. **Desconectar EBB42** del USB (si está conectada)
2. **Mantener presionado botón BOOT** en la EBB42
3. **Conectar USB-C** a la EBB42 (mientras mantienes BOOT)
4. **Conectar otro extremo USB** al PC
5. **Soltar botón BOOT** después de 2 segundos

**Opción 2: Puentear BOOT y 3.3V**
1. Con un jumper o cable, puentear pines BOOT y 3.3V en EBB42
2. Conectar USB a PC
3. Quitar jumper después de conectar

#### 2.3 Verificar modo DFU

```bash
lsusb | grep DFU
```

**Resultado esperado:**
```
Bus 001 Device 0XX: ID 0483:df11 STMicroelectronics STM Device in DFU Mode
```

Si NO aparece:
- Re-intentar poner en DFU mode (Paso 2.2)
- Verificar cable USB (debe ser de datos)
- Probar otro puerto USB del PC

#### 2.4 Flashear firmware vía DFU

```bash
sudo dfu-util -a 0 -D ~/klipper/out/klipper.bin --dfuse-address 0x08002000:force:mass-erase:leave -d 0483:df11
```

**Explicación del comando:**
- `-a 0`: Alternate setting 0
- `-D klipper.bin`: Archivo a flashear
- `--dfuse-address 0x08002000`: Offset para 8KiB bootloader
- `:force:mass-erase:leave`: Borrar, flashear, y reiniciar
- `-d 0483:df11`: ID del dispositivo STM32 en DFU

**Resultado esperado:**
```
Downloading to address = 0x08002000, size = XXXXX
Download        [=========================] 100%        XXXXX bytes
Download done.
File downloaded successfully
Transitioning to dfuMANIFEST state
```

#### 2.5 Reiniciar EBB42

La EBB42 debería reiniciar automáticamente. Si no:
1. Desconectar USB
2. Reconectar USB

---

### Método B: Usando CanBoot/Katapult (Alternativo)

Si la EBB42 ya tiene CanBoot instalado de fábrica:

#### 2.1 Poner en bootloader mode

```bash
# Conectar EBB42 vía USB primero
cd ~/katapult/scripts
python3 flashtool.py -i usb -u
```

#### 2.2 Flashear con katapult

```bash
python3 flashtool.py -f ~/klipper/out/klipper.bin -d /dev/serial/by-id/usb-katapult_stm32g0b1xx_XXXXXX
```

---

## Paso 3: Verificar EBB42 Flasheada

### 3.1 Desconectar y reconectar USB

1. Desconectar EBB42 del PC
2. Esperar 2 segundos
3. Reconectar EBB42 al PC

### 3.2 Verificar detección

```bash
ls /dev/serial/by-id/
```

**Resultado esperado:**
```
usb-Klipper_stm32f407xx_XXXXXX-if00     <- SKR
usb-Klipper_stm32g0b1xx_YYYYYY-if00     <- EBB42
```

**Anota el ID de la EBB42** (el que empieza con `stm32g0b1xx`).

### 3.3 Si NO aparece EBB42

**Troubleshooting:**

1. Verificar LED en EBB42 (debe estar encendido)
2. Revisar dmesg:
   ```bash
   dmesg | tail -30
   ```
   Buscar: "usb 1-X: new full-speed USB device"

3. Re-flashear usando DFU mode (repetir Paso 2)

4. Verificar que el firmware se compiló para **USB**, NO CAN:
   ```bash
   cd ~/klipper
   make menuconfig
   # Verificar "Communication interface (USB on PA11/PA12)"
   ```

---

## Paso 4: Configurar EBB42 en printer.cfg

### 4.1 Añadir MCU EBB42

Edita tu `printer.cfg`:

```ini
[mcu EBBCan]
serial: /dev/serial/by-id/usb-Klipper_stm32g0b1xx_YYYYYY-if00
```

Reemplaza `YYYYYY` con tu ID real del Paso 3.2.

### 4.2 Restart Klipper

En Mainsail:
```
RESTART
```

O vía terminal:
```bash
sudo systemctl restart klipper
```

### 4.3 Verificar conexión

En logs de Klipper (`/tmp/klippy.log`):
```
MCU 'mcu' connected         <- SKR
MCU 'EBBCan' connected      <- EBB42
```

En Mainsail, no debe haber errores.

---

## ✅ Ambas Placas Flasheadas y Conectadas

Si ves ambos MCUs conectados, estás listo para continuar Phase 3.

**Estado:**
- ✅ SKR 1.4 Turbo flasheada (USB serial)
- ✅ EBB42 flasheada (USB)
- ✅ Ambas detectadas en `/dev/serial/by-id/`
- ✅ Klipper conecta a ambas

**Siguiente paso:** Fotografiar toolhead stock y comenzar migración física.

---

## 🔧 Troubleshooting Común

### Problema: "dfu-util: No DFU capable USB device available"

**Causa:** EBB42 no está en DFU mode

**Solución:**
1. Desconectar USB completamente
2. Mantener botón BOOT presionado
3. Conectar USB mientras mantienes BOOT
4. Soltar BOOT después de 2 segundos
5. Verificar: `lsusb | grep DFU`

---

### Problema: "Error during download get_status"

**Causa:** Permisos o conflicto USB

**Solución:**
1. Usar `sudo` con dfu-util
2. Desconectar otros dispositivos USB
3. Reintentar

---

### Problema: EBB42 no aparece después de flashear

**Causa:** Firmware incorrecto (posiblemente compilado para CAN)

**Solución:**
1. Verificar configuración:
   ```bash
   cd ~/klipper
   make menuconfig
   ```
   Confirmar: "Communication interface (USB on PA11/PA12)"
2. Re-compilar: `make clean && make`
3. Re-flashear vía DFU

---

**Tiempo estimado:** 20-30 minutos
**Dificultad:** Media (requiere DFU mode correcto)
