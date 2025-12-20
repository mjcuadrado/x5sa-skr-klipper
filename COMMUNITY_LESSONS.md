# Lecciones Aprendidas de la Comunidad

**Recopilación de experiencias reales de usuarios que han migrado Tronxy X5SA a Klipper**

---

## 📍 Ubicación de la Placa Controladora

### Opción A: Placa Arriba (encima del frame)
**Ventajas reportadas:**
- Fácil acceso para conexiones y cambios
- Mejor visibilidad de LEDs de estado
- Facilita troubleshooting

**Desventajas:**
- Expuesta a polvo y residuos
- Cables visibles (estética)
- Menos protección física

### Opción B: Placa Abajo (compartimento inferior) ✅ **NUESTRA ELECCIÓN**
**Ventajas reportadas:**
- Protegida de polvo y suciedad
- Estética más limpia
- Separación de zona caliente (cama)
- Mejor organización de cables

**Desventajas:**
- Acceso más complicado
- Requiere enfriamiento activo (ventiladores)

### Recomendaciones de CoreXY (Voron, etc.)

Según documentación de **Voron CoreXY designs**:
- **Ubicación estándar:** Compartimento electrónica en la parte inferior
- **Enfriamiento:** Dual 60mm fans para compartimento electrónico
- **Montaje:** DIN rail estándar
- **Aislamiento:** Electrónica FUERA del volumen calentado

**Conclusión comunidad:** Placa abajo es mejor práctica para CoreXY.

---

## ⚠️ Problemas Comunes Reportados

### 1. Pantalla LCD Stock Inoperativa
**Síntoma:** Pantalla LCD original deja de funcionar después de flashear Klipper
**Causa:** Incompatibilidad firmware Klipper con LCD Tronxy
**Solución:**
- Comportamiento esperado y normal
- Usar Mainsail/Fluidd en su lugar
- LCD permanecerá inutilizable con Klipper

### 2. Eje Z No Se Mueve
**Síntoma:** X e Y funcionan perfectamente, pero Z no se mueve en ninguna dirección
**Causa reportada:** Problema con placas CXY-V6
**Solución:**
- Verificar configuración de pines en printer.cfg
- Verificar dirección del motor (`dir_pin: !PX.XX` o sin `!`)
- Comprobar endstop Z

### 3. Impresión Falla al 25% Aprox.
**Síntoma:** Impresiones se detienen consistentemente alrededor del 25%
**Causa:** Error general de Klipper mid-print
**Soluciones reportadas:**
- Revisar logs: `journalctl -u klipper -f`
- Verificar temperatura MCU
- Comprobar alimentación estable
- Reducir `max_accel` temporalmente

### 4. Klipper Crashea con Placa CXY-V6-191121
**Síntoma:** Klipper funciona bien cuando funciona, pero crashea frecuentemente
**Causa:** Compatibilidad limitada con placas V6 específicas
**Solución:**
- Considerar reemplazo de placa (SKR 1.4 Turbo, etc.)
- Actualizar a última versión de Klipper
- Verificar configuración UART de drivers

### 5. Pantalla Negra Después de Flash
**Síntoma:** Pantalla negra, sin beep, ventiladores a full speed
**Causa:** Flash incorrecto o archivo firmware.bin corrupto
**Solución:**
- Verificar que archivo se llama exactamente `firmware.bin`
- SD card formateada FAT32
- Firmware compilado para MCU correcto
- Apagar completamente y reintentar

### 6. Error "Unable to open config.cfg"
**Síntoma:** Error al conectar con OctoPrint/Mainsail
**Causa:** Ruta incorrecta a printer.cfg
**Solución:**
- Verificar `~/printer_data/config/printer.cfg` existe
- Permisos correctos: `chmod 644 printer.cfg`
- Servicio Klipper corriendo: `sudo systemctl status klipper`

---

## 🔧 SKR 1.4 Turbo: Recomendaciones Específicas

### Enfriamiento **OBLIGATORIO**
Según comunidad:
- **Ventilador 60x60x10mm** conectado a salida 5V/12V
- Orientar disipadores paralelos al flujo de aire
- **Crítico:** TMC2209 pueden sobrecalentarse sin ventilación

### Montaje DIN Rail
Múltiples usuarios recomiendan:
- **DIN rail mount con ventilador 40mm integrado**
- STL disponible: [Printables - SKR 1.4 DIN mount](https://www.printables.com/model/339348-bigtreetech-skr-14-with-4040-cooling-din-mount)

### Gestión de Cables
- **Cable holders adhesivos** en compartimento electrónica
- Etiquetar TODOS los cables antes de desconectar
- Usar fundas trenzadas para organización

---

## 🌐 EBB42 CAN: Lecciones Críticas

### 1. Diagrama de Cableado Confuso
**Problema reportado:** [GitHub Issue #68](https://github.com/bigtreetech/EBB/issues/68)
- Manual oficial puede ser confuso
- Diagrama corregido disponible en issue

**Pines CAN correctos:**
```
EBB42          SKR / RPi
CAN_H    --->  CAN_H
CAN_L    --->  CAN_L
24V      --->  24V
GND      --->  GND
```

### 2. Jumper VUSB
**Crítico:** VUSB jumper en centro de placa
- **CON jumper:** Alimentar EBB42 desde USB (solo para testing)
- **SIN jumper:** Alimentar desde 24V (modo producción)

**Error común:** Dejar jumper puesto en modo CAN → problemas de alimentación

### 3. Resistencia de Terminación 120Ω
**Obligatorio para CAN bus:**
- Insertar jumper de terminación 120R en EBB42
- **Solo** si es el último dispositivo en bus CAN
- Sin resistencia → comunicación inestable

### 4. Conexión: ¿Octopus o Raspberry Pi?
**Recomendación comunidad:**
- Conectar EBB42 CAN a **Raspberry Pi CAN interface**
- Conexión a placa Octopus puede causar pérdida de conexión
- Usar adaptador CAN hat para RPi

### 5. Alimentación Hotend
**Aclaración importante:**
- Si usas EBB42, **NO** conectar hotend heater a placa principal
- EBB42 maneja el heater localmente
- Evita duplicar conexiones de poder

### 6. Stepper Motor Sobrecalentamiento
**Problema reportado:** NEMA14 en EBB42 sobrecalienta
**Causas:**
- `run_current` demasiado alto
- Falta de ventilación en toolhead
- Driver TMC2209 integrado en EBB42 sin disipador

**Soluciones:**
- Reducir `run_current` a 0.5-0.65A
- Añadir ventilación dirigida a EBB42
- Considerar heatsink en TMC2209

---

## 📦 Configuraciones de Referencia (GitHub)

### Con SKR 1.4 Turbo
- [Darkwulf183 - X5SA + SKR 1.4 Turbo](https://github.com/Darkwulf183/Tronxy-X5SA-Printer.cfg-and-more-for-Klipper)
  - Chitu V6-V9 boards + STM32F103
  - Compatible con SKR 1.4 Turbo

### Con SKR 1.3 (similar a 1.4)
- [it4k4i - X5SA-400 + SKR 1.3 + TMC2208](https://github.com/it4k4i/Tronxy-X5SA-400-Klipper)
  - Dual Z stepper drivers
  - Configuración muy similar a SKR 1.4

### Con SKR Pro
- [grantemsley - X5SA Pro + SKR Pro 1.2](https://github.com/grantemsley/klipper-tronxy-x5sapro-skrpro)
  - Configuración más avanzada
  - Referencia para features adicionales

### Guía de Instalación Detallada
- [cab404 - Installing Klipper on X5SA](https://gist.github.com/cab404/b7bcbb0cd592a14515493694719de59b)
  - Gist con discusiones comunitarias
  - Tips de troubleshooting

---

## 🎯 Decisiones de Diseño para Nuestro Proyecto

### ✅ Confirmadas

| Decisión | Razón |
|----------|-------|
| **Placa abajo** | Best practice CoreXY, protección, estética |
| **Ventilación activa** | Obligatorio según comunidad para SKR |
| **DIN rail mount** | Estándar, fácil acceso, ventilación integrada |
| **EBB42 → RPi CAN** | Más estable que conexión a placa principal |
| **Resistencia 120Ω** | Obligatoria para CAN bus |
| **Etiquetado cables** | Prevención errores, mantenimiento futuro |

### ⏸️ Pendientes de Confirmar

- [ ] Tipo exacto de ventiladores (60mm o 40mm)
- [ ] Ubicación exacta de compartimento electrónica
- [ ] Diseño de DIN rail mount custom o usar STL existente
- [ ] Cable management específico para drag chain

---

## 💡 Tips Generales de la Comunidad

### Antes de Empezar
1. **Hacer backup** de firmware stock (si es posible)
2. **Etiquetar TODOS** los cables antes de desconectar
3. **Fotos exhaustivas** del estado stock
4. **Verificar voltajes** con multímetro antes de alimentar

### Durante la Migración
1. **No saltar pasos** - Verificar cada componente antes de avanzar
2. **Probar incrementalmente** - Un cambio a la vez
3. **Logs siempre a mano** - `journalctl -u klipper -f`
4. **Comunidad es tu amiga** - Preguntar en Discord/Discourse antes de romper algo

### Después de Migrar
1. **PID tuning** de hotend y cama - Obligatorio
2. **Input shaper** cuanto antes - Mejora dramática
3. **Pressure advance** - Game changer para calidad
4. **Backup regular** de printer.cfg

---

## 🔗 Fuentes Consultadas

### Foros y Comunidades
- [Klipper Discourse - Tronxy X5SA](https://klipper.discourse.group/)
- [GitHub Issues - BTT EBB](https://github.com/bigtreetech/EBB/issues)
- [Voron Documentation](https://docs.vorondesign.com/build/electrical/)
- [TronXY Wiki](https://tronxy.fandom.com/wiki/Installing_Klipper)

### Configuraciones de Referencia
- [Darkwulf183/Tronxy-X5SA-Klipper](https://github.com/Darkwulf183/Tronxy-X5SA-Printer.cfg-and-more-for-Klipper)
- [it4k4i/Tronxy-X5SA-400-Klipper](https://github.com/it4k4i/Tronxy-X5SA-400-Klipper)
- [grantemsley/klipper-tronxy-x5sapro-skrpro](https://github.com/grantemsley/klipper-tronxy-x5sapro-skrpro)
- [markcarroll/tronxy-x5sa](https://github.com/markcarroll/tronxy-x5sa)

### Guías y Tutoriales
- [Installing Klipper on X5SA - cab404](https://gist.github.com/cab404/b7bcbb0cd592a14515493694719de59b)
- [Voron V2 SKR 1.4 Wiring](https://github.com/VoronDesign/Voron-2/blob/Voron2.4/firmware/klipper_configurations/SKR_1.4/Voron2_SKR_14_Wiring.pdf)
- [BTT EBB42 Documentation](https://github.com/bigtreetech/docs/blob/master/docs/EBB%2042%20CAN.md)

### Issues y Troubleshooting
- [Klipper Issue #4295 - X5SA Pro Fails at 25%](https://github.com/Klipper3d/klipper/issues/4295)
- [EBB Issue #68 - CAN Wiring Diagram](https://github.com/bigtreetech/EBB/issues/68)
- [Marlin Issue #17639 - CoreXY Homing](https://github.com/MarlinFirmware/Marlin/issues/17639)

### STLs y Modelos
- [SKR 1.4 DIN Mount - Printables](https://www.printables.com/model/339348-bigtreetech-skr-14-with-4040-cooling-din-mount)
- [SKR 1.4 Fan Duct - Thingiverse](https://www.thingiverse.com/thing:4191051)

---

**Última actualización:** 2025-12-20
**Versión:** 1.0
**Estado:** Investigación activa
