# Flashear SKR 1.4 Turbo con Klipper

**Fecha:** 2025-12-21
**Hardware:** SKR 1.4 Turbo (LPC1769FBD100)

---

## Paso 1: Compilar Firmware

### 1.1 Navegar al directorio de Klipper

```bash
cd ~/klipper
```

### 1.2 Limpiar compilación anterior (si existe)

```bash
make clean
```

### 1.3 Abrir menú de configuración

```bash
make menuconfig
```

### 1.4 Configuración para SKR 1.4 Turbo

**Selecciona estas opciones en el menú:**

```
[*] Enable extra low-level configuration options
    Micro-controller Architecture (LPC176x (Smoothieboard))  --->
    Processor model (lpc1769 (120 MHz))  --->
    Bootloader offset (16KiB bootloader (Smoothieware))  --->
    Communication interface (USB)  --->
```

**Explicación:**
- **Micro-controller:** LPC176x (familia del chip NXP en SKR 1.4 Turbo)
- **Processor model:** lpc1769 (120 MHz) (chip exacto en SKR 1.4 Turbo)
- **Bootloader offset:** 16KiB bootloader (Smoothieware) (bootloader pre-instalado)
- **Communication:** USB (conexión directa por USB al host)

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

El archivo `out/klipper.bin` está listo.

---

## Paso 2: Flashear SKR vía Tarjeta SD

### 2.1 Preparar tarjeta SD

**Requisitos:**
- Tarjeta SD/microSD (FAT32)
- Adaptador USB para leer SD en tu PC

### 2.2 Copiar firmware a SD

```bash
# Asume que SD está montada en /media/tu_usuario/SD_CARD
# Ajusta la ruta según tu sistema
cp ~/klipper/out/klipper.bin /media/$USER/*/firmware.bin
```

**IMPORTANTE:** El archivo DEBE llamarse exactamente `firmware.bin` (minúsculas).

### 2.3 Verificar archivo copiado

```bash
ls -lh /media/$USER/*/firmware.bin
```

Debe mostrar el archivo (~25-30 KB).

### 2.4 Desmontar SD de forma segura

```bash
sync
umount /media/$USER/*
```

### 2.5 Flashear SKR

1. **Apagar impresora** (desenchufar PSU)
2. **Sacar tarjeta SD de tu PC**
3. **Insertar SD en SKR 1.4 Turbo** (slot en la placa)
4. **Enchufar PSU** (encender impresora)

**Durante el flasheo (~10 segundos):**
- LED de estado en SKR parpadeará
- Cuando termine, LED queda fijo o apagado

5. **Apagar impresora** nuevamente
6. **Sacar SD de la SKR**

### 2.6 Verificar flasheo exitoso

**Opcional:** Re-insertar SD en tu PC y verificar:
- El archivo `firmware.bin` habrá sido renombrado a `FIRMWARE.CUR`
- Esto confirma que la SKR procesó el archivo

---

## Paso 3: Conectar SKR a PC vía USB

### 3.1 Conectar cable USB

- Cable USB-A (PC) a micro-USB (SKR)
- Puerto en SKR: conector micro-USB en la placa

### 3.2 Encender impresora

Enchufar PSU.

### 3.3 Verificar detección en Linux

```bash
ls /dev/serial/by-id/
```

**Resultado esperado:**
```
usb-Klipper_lpc1769_XXXXXXXXXXXXXXXXXXXXXX-if00
```

Anota el ID completo (lo necesitarás para `printer.cfg`).

### 3.4 Si NO aparece

**Troubleshooting:**

1. Verificar cable USB (debe ser de datos, no solo carga)
2. Verificar que LED en SKR está encendido
3. Revisar dmesg:
   ```bash
   dmesg | tail -20
   ```
   Buscar mensajes como "usb 1-1: new full-speed USB device"

4. Si sigue sin aparecer: re-flashear SKR (repetir Paso 2)

---

## Paso 4: Prueba Básica de Comunicación

### 4.1 Configurar MCU en printer.cfg

Edita tu `printer.cfg`:

```ini
[mcu]
serial: /dev/serial/by-id/usb-Klipper_lpc1769_XXXXXXXXXXXXXXXXXXXXXX-if00
```

Reemplaza `XXXX...` con tu ID real del Paso 3.3.

### 4.2 Restart Klipper

En Mainsail, ejecuta:
```
RESTART
```

O vía terminal:
```bash
sudo systemctl restart klipper
```

### 4.3 Verificar conexión

En Mainsail:
- No debe haber errores rojos
- Estado: "Ready" o "Printer is ready"

En logs de Klipper (`/tmp/klippy.log`):
```
MCU 'mcu' connected
```

---

## ✅ SKR Flasheada Exitosamente

Si ves "MCU 'mcu' connected", la SKR está lista.

**Siguiente paso:** Flashear EBB42 (ver `FLASH_EBB42_INSTRUCTIONS.md`)

---

## 🔙 Rollback (si algo falla)

Para volver a flashear:
1. Apagar impresora
2. Compilar nuevo firmware (ajustar configuración si necesario)
3. Copiar a SD como `firmware.bin`
4. Repetir proceso de flasheo

La SKR acepta re-flasheos ilimitados.

---

**Tiempo estimado:** 15-20 minutos
**Dificultad:** Baja
