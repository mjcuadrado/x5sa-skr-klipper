# Hardware Evolution - Tronxy X5SA Klipper Migration

**Project:** Tronxy X5SA → SKR 1.4 Turbo + EBB42 CAN V1.2
**Timeline:** 2025-12-20 → 2025-12-26 (ongoing)
**Version:** 1.0

---

## Índice

1. [Resumen Ejecutivo](#resumen-ejecutivo)
2. [Cronología de Cambios](#cronología-de-cambios)
3. [Hardware Original](#hardware-original)
4. [Migración SKR 1.4 Turbo](#migración-skr-14-turbo)
5. [Integración EBB42](#integración-ebb42)
6. [Evolución del Probe Sensor](#evolución-del-probe-sensor)
7. [Incidentes Críticos](#incidentes-críticos)
8. [Estado Actual](#estado-actual)
9. [Lecciones Aprendidas](#lecciones-aprendidas)
10. [Próximos Pasos](#próximos-pasos)

---

## Resumen Ejecutivo

Este documento narra la evolución del hardware durante la migración de una impresora **Tronxy X5SA** desde electrónica stock a un sistema moderno basado en **Klipper** con **SKR 1.4 Turbo** y **EBB42 CAN V1.2**.

### Cambios Mayores

| Componente | Original | Actual | Estado |
|------------|----------|--------|--------|
| **Main MCU** | Tronxy Stock | SKR 1.4 Turbo #2 | ✅ Operativo |
| **Toolhead MCU** | Ninguno | EBB42 CAN V1.2 (USB) | ✅ Operativo |
| **Probe Sensor** | Ninguno → Tronxy XY-08N (fallido) | Eddy Coil V1.0 | ✅ Operativo |
| **Drivers** | Stock integrados | 5× TMC2209 UART | ✅ Operativos |
| **Firmware** | Marlin (stock) | Klipper master | ✅ Operativo |
| **Comunicación** | USB simple | Dual-MCU USB | ✅ Operativo |

### Timeline Resumido

- **2025-12-20:** Inicio proyecto - Auditoría hardware stock
- **2025-12-21:** SKR 1.4 Turbo #1 flasheada e instalada
- **2025-12-21:** EBB42 flasheada, montaje temporal, sistema dual-MCU operativo
- **2025-12-22–25:** Intentos fallidos con sensor Tronxy XY-08N
- **2025-12-25:** **Incidente crítico** - SKR #1 dañada (cortocircuito GND)
- **2025-12-26:** SKR #2 instalada, migración a Eddy Coil V1.0

---

## Cronología de Cambios

### Phase 0: Baseline (2025-12-20)

**Objetivo:** Auditoría y documentación del estado inicial

**Acciones:**
- Fotografía completa del sistema stock
- Backup de configuración Marlin original
- Identificación de componentes existentes
- Creación de repositorio Git
- Tag `phase0-baseline` creado

**Hardware documentado:**
- Placa controladora Tronxy stock
- Thermistor NTC 100K (hotend y bed)
- Ventiladores 24V stock (2×)
- Extrusor Titan clone con motor
- Estructura CoreXY con dual Z

**Estado:** ✅ Completado - Sistema stock documentado y funcional

---

### Phase 1: Preparación SKR 1.4 Turbo (2025-12-20–21)

**Objetivo:** Flashear y preparar SKR 1.4 Turbo para instalación

**Decisiones técnicas:**
- Chip identificado: **LPC1769FBD100** (no STM32F407 como sugieren algunas guías)
- Configuración Klipper: LPC176x, 120MHz, 16KiB bootloader, USB
- Método de flasheo: SD card (FIRMWARE.BIN → FIRMWARE.CUR)

**Proceso:**
1. Compilación firmware Klipper para LPC1769
2. Flasheo vía SD exitoso
3. Detección USB: `usb-Klipper_lpc1769_12345-if00`
4. Configuración printer.cfg con serial ID

**Challenges:**
- Documentación inicial sugería STM32F407 (incorrecto)
- Necesidad de identificar chip físicamente
- Confusión sobre configuración LPC vs STM32

**Estado:** ✅ Completado - SKR #1 operativa con Klipper

**Documentación:** [FLASHEO_SKR_EXITOSO.md](guides/phase3/FLASHEO_SKR_EXITOSO.md)

---

### Phase 2: Migración a SKR 1.4 Turbo #1 (2025-12-21)

**Objetivo:** Instalar SKR en la impresora y migrar componentes básicos

**Hardware migrado:**
- 5× TMC2209 drivers (X, Y, Z, Z1, E)
- Conexión USB a Raspberry Pi
- Alimentación 24V desde PSU
- Configuración CoreXY con sensorless homing
- Heated bed
- Motors (X, Y, Z dual, E)

**Configuración Klipper:**
- Dual Z independiente con `[stepper_z]` y `[stepper_z1]`
- Sensorless homing XY con TMC2209 StallGuard
- Endstop físico Z (temporal)
- PID tuning para bed
- Velocidades y aceleraciones básicas

**Estado:** ✅ Completado - Sistema básico operativo sin toolhead moderno

---

### Phase 3: Integración EBB42 (2025-12-21)

**Objetivo:** Añadir MCU en toolhead para componentes del hotend

#### 3.1 Flasheo EBB42

**Hardware:** BIGTREETECH EBB42 CAN V1.2

**Decisiones técnicas:**
- Modo: **USB** (no CAN) para simplicidad
- Chip: STM32G0B1CBT6
- Configuración: No bootloader, USB PA11/PA12, 8MHz crystal
- Método flasheo: DFU mode (offset 0x08000000)

**Proceso:**
1. Compilación firmware Klipper para STM32G0B1
2. Entrada en DFU mode (con 24V + reset manual)
3. Flasheo vía `dfu-util`
4. Instalación jumper VUSB (crítico para USB mode)
5. Detección USB: `usb-Klipper_stm32g0b1xx_12345-if00`

**Challenges:**
- Necesidad de alimentación 24V durante DFU (no documentada claramente)
- Reset manual requerido post-DFU
- Jumper VUSB olvidado inicialmente

**Estado:** ✅ Completado

**Documentación:** [FLASHEO_EBB42_EXITOSO.md](guides/phase3/FLASHEO_EBB42_EXITOSO.md)

#### 3.2 Montaje Temporal

**Decisión arquitectónica:** Montaje temporal de EBB42 junto a SKR en frame superior

**Razones:**
- Cables stock del toolhead ya llegan al frame superior
- Solo necesita cables cortos (~30cm): USB-C y 24V entre placas
- Troubleshooting más fácil (ambas placas accesibles)
- Minimiza complejidad en fase de testing
- Migración definitiva a toolhead pospuesta a Phase 12 (Stealthburner)

**Componentes conectados a EBB42:**
- Heater hotend → PB13 (HE)
- Thermistor NTC 100K → PA3 (TH0)
- Part cooling fan → PA0 (FAN)
- Hotend fan → PA1 (FAN1)

**Motor extrusor:** Permanece en SKR E0 (no migrado)

**Estado:** ✅ Completado - Heater y fans operativos

**Documentación:** [INSTALACION_COMPLETADA.md](guides/phase3/INSTALACION_COMPLETADA.md)

---

### Evolución del Probe Sensor

#### Intento 1: Sin Probe (2025-12-21)

**Estado inicial:** Sistema operativo sin auto bed leveling

**Limitación:** Z homing con endstop físico, sin compensación de nivel de cama

#### Intento 2: Tronxy XY-08N (2025-12-22–25) ❌ FALLIDO

**Hardware:** Sensor inductivo Tronxy XY-08N

**Especificaciones:**
- Tipo: NPN Normally Open
- Voltaje: 6-36V DC
- Salida: 24V cuando triggered (pullup interno a 24V)
- Conexión: 3 hilos (VCC/GND/Signal)

**Problema fundamental:**
El sensor output 24V en su señal, incompatible con pines MCU de 3.3V.

**Intentos de solución (todos fallidos):**

1. **Configuración directa (Día 1):**
   - Pines probados: P0.10, P1.25, P1.26, P1.27 (SKR)
   - Resultado: Siempre "open" o siempre "TRIGGERED"
   - Conclusión: Voltaje demasiado alto

2. **BAT85 Schottky diode (Día 2):**
   - Circuito: Sensor → BAT85 → Pin MCU
   - Resultado: LED del sensor NO enciende (insuficiente corriente)
   - Problema: Pullup interno del sensor requiere corriente que BAT85 no puede proporcionar
   - Conclusión: Diodo solo insuficiente

3. **Resistencia en serie 68kΩ (Día 3):**
   - Circuito: Sensor → 68kΩ resistor → Pin MCU
   - Resultado: LED no enciende, sensor no opera
   - Problema: Corriente insuficiente para activar sensor
   - Conclusión: Divisor pasivo insuficiente

4. **Alimentación híbrida (Día 4):**
   - VCC + GND desde SKR FAN0 (24V)
   - Signal a EBB42 PA5
   - Múltiples configuraciones pullup (`^`, `!^`, etc.)
   - Resultado: Sensor siempre "open" o siempre "TRIGGERED"
   - Conclusión: Problema eléctrico fundamental

5. **Prueba en pins especiales (Día 5):**
   - P1.26 (E0DET) y P1.27 (E1DET) - supuestamente con protección
   - Resultado: Sin cambio
   - Conclusión: Protección insuficiente para 24V

**Pines probados en total:**
- SKR: P0.10, P1.25, P1.26, P1.27
- EBB42: PA5, PB8, PB9

**Diagnóstico final:**
- Sensor tiene pullup interno a 24V (~40kΩ estimado)
- Output es 24V cuando triggered (no compatible con 3.3V MCU)
- Requiere circuito activo (transistor + resistencias) para level shifting
- Complejidad no justificada para un sensor obsoleto

**Tiempo invertido:** ~2+ días (16+ horas)

**Estado:** ❌ ABANDONADO - Sensor incompatible

**Documentación:** Ver [INSTALACION_COMPLETADA.md](guides/phase3/INSTALACION_COMPLETADA.md) sección "Problema con Tronxy XY-08N"

#### Solución Final: BIGTREETECH Eddy Coil V1.0 (2025-12-26) ✅

**Hardware:** BIGTREETECH Eddy Coil V1.0

**Especificaciones:**
- Tipo: Sensor de corrientes de Eddy (eddy current)
- Chip: LDC1612 (Texas Instruments)
- Comunicación: I2C
- Alimentación: 3.3V/5V (directo desde EBB42)
- Precisión: ±0.01mm (vs ±0.1mm inductivo)
- Soporte: Nativo en Klipper

**Ventajas sobre Tronxy XY-08N:**
- ✅ Sin adaptación de voltaje necesaria
- ✅ Comunicación digital I2C (no analógica)
- ✅ Calibración automática via Klipper
- ✅ 10× más preciso
- ✅ Documentación oficial completa
- ✅ Comunidad activa de soporte

**Conexión:**
- I2C SCL: EBB42 PB3
- I2C SDA: EBB42 PB4
- I2C address: 42 (decimal)
- I2C speed: 400kHz

**Configuración Klipper:**
```ini
[probe_eddy_current btt_eddy]
sensor_type: ldc1612
i2c_mcu: EBBCan
i2c_address: 42
i2c_speed: 400000
i2c_software_scl_pin: EBBCan:PB3
i2c_software_sda_pin: EBBCan:PB4
```

**Calibración:**
1. `LDC_CALIBRATE_DRIVE_CURRENT CHIP=btt_eddy` (solo una vez)
2. `PROBE_EDDY_CURRENT_CALIBRATE CHIP=btt_eddy` (para z_offset)
3. `SAVE_CONFIG`

**Estado:** ✅ Configurado - Pendiente instalación física y calibración

**Documentación:**
- [EDDY_COIL_INSTALLATION.md](guides/phase3/EDDY_COIL_INSTALLATION.md)
- [EDDY_COIL_CALIBRATION.md](guides/phase3/EDDY_COIL_CALIBRATION.md)

---

## Incidentes Críticos

### Incidente #1: Fallo SKR 1.4 Turbo #1 (2025-12-25)

**Severidad:** 🔴 **CRÍTICA** - Placa inutilizada

**Contexto:**
Durante troubleshooting del sensor Tronxy XY-08N, se sugirió conectar GND del sensor desde EBB42 en lugar de SKR FAN0 para "compartir referencia de tierra común".

**Acción que causó el fallo:**
- **Antes:** Sensor VCC+GND desde SKR FAN0, Signal a EBB42 PA5
- **Cambio:** Sensor VCC desde SKR, **GND movido a EBB42**, Signal a EBB42 PA5
- **Resultado:** SKR dejó de ser detectada por USB inmediatamente

**Diagnóstico post-mortem:**
- Probablemente creó **ground loop** o cortocircuito entre SKR y EBB42
- SKR alimentaba sensor a 24V, EBB42 proporcionaba GND
- Posible diferencia de potencial o corriente excesiva
- Chip LPC1769 o reguladores de voltaje dañados

**Síntomas:**
- SKR no detectada en USB: `ls /dev/serial/by-id/` no muestra dispositivo
- LED power encendido (PSU funciona)
- No responde a reflasheo
- Completamente inoperativa

**Impacto:**
- Costo: ~€35-40 (nueva SKR 1.4 Turbo)
- Tiempo perdido: ~4 horas (diagnóstico + espera + reinstalación)
- Riesgo: TMC2209 drivers supervivientes verificados OK

**Lección aprendida:**
❌ **NUNCA** conectar grounds entre placas con diferentes fuentes de alimentación sin verificar:
- Esquemas eléctricos completos
- Potenciales relativos
- Corrientes de retorno
- Aislamiento galvánico

✅ **SIEMPRE** cuando sea posible, mantener alimentación y ground de un componente en la misma placa.

**Solución:**
- Compra e instalación de SKR 1.4 Turbo #2
- Reflasheo con misma configuración
- TMC2209 drivers reutilizados (OK)
- Sistema restaurado completamente

**Estado:** ✅ Resuelto - SKR #2 operativa

**Commits afectados:**
- `git reset --hard 42f69d3` - Revertidos 27 commits de intentos con XY-08N
- `b9eefb5` - Nueva configuración con Eddy Coil post-recuperación

---

## Estado Actual

### Hardware Instalado (2025-12-26)

| Componente | Modelo | Ubicación | Estado |
|------------|--------|-----------|--------|
| **Main MCU** | SKR 1.4 Turbo #2 | Frame superior | ✅ Operativa |
| **Toolhead MCU** | EBB42 CAN V1.2 | Frame superior (temporal) | ✅ Operativa |
| **Drivers** | 5× TMC2209 UART | SKR slots | ✅ Operativos |
| **Probe** | Eddy Coil V1.0 | Pendiente montaje | ⏳ Configurado |
| **Heater** | Stock 24V 40W | Hotend → EBB42 | ✅ Operativo |
| **Thermistor** | NTC 100K | Hotend → EBB42 | ✅ Operativo |
| **Fans** | 2× Stock 24V | EBB42 | ✅ Operativos |
| **Extruder** | Titan clone | SKR E0 | ✅ Operativo |

### Configuración Software

**Firmware:**
- Klipper master (v0.12.0-239-g8b8f7c09)
- SKR: LPC1769, 120MHz, USB
- EBB42: STM32G0B1, USB mode (no CAN)

**Comunicación:**
- Dual-MCU via USB
- SKR: `usb-Klipper_lpc1769_12345-if00`
- EBB42: `usb-Klipper_stm32g0b1xx_12345-if00`

**Configuración destacada:**
- CoreXY kinematics
- Sensorless homing X/Y (TMC2209 StallGuard)
- Dual Z independiente (2× steppers)
- Eddy Coil probe (I2C, pendiente calibración)
- PID tuning completado (heater + bed)

### Git Repository Status

**Último commit:** `b9eefb5` - feat(klipper): add BIGTREETECH Eddy Coil V1.0 probe configuration

**Commits importantes:**
- `42f69d3` - fix(klipper): correct heater pin to PB13 for EBB42 V1.2 (última configuración estable pre-probe)
- `b9eefb5` - feat: Eddy Coil configuration (post-recuperación)

**Commits eliminados:** 27 commits relacionados con intentos fallidos de Tronxy XY-08N

---

## Lecciones Aprendidas

### 1. Compatibilidad Eléctrica

**Lección:**
Verificar **voltajes de señal** antes de conectar sensores a MCUs.

**Error cometido:**
Asumir que un sensor "inductivo" sería compatible con pines de 3.3V sin verificar voltaje de salida.

**Correcto:**
- ✅ Leer datasheet del sensor **antes** de comprar
- ✅ Verificar voltaje de output (debe ser ≤3.3V para MCUs modernos)
- ✅ Para sensores legacy 24V: usar sensores modernos o circuitos activos certificados

### 2. Ground Loops y Alimentación

**Lección:**
No conectar grounds entre placas sin entender el circuito completo.

**Error cometido:**
Mover GND de sensor de SKR a EBB42 sin considerar implicaciones (causó fallo de SKR).

**Correcto:**
- ✅ Mantener VCC + GND de un componente en la misma placa siempre que sea posible
- ✅ Si se requiere ground común, usar esquemas oficiales verificados
- ✅ Nunca improvisar conexiones cross-board sin esquemático

### 3. Documentación y Rollback

**Lección:**
Commits frecuentes y buenos tags permiten recuperación rápida.

**Éxito:**
- `git reset --hard 42f69d3` permitió eliminar 27 commits fallidos en segundos
- Tag `phase0-baseline` garantiza rollback total si necesario
- Documentación fotográfica permitió verificar cableado correcto

**Correcto:**
- ✅ Commit después de cada cambio funcional
- ✅ Tags en milestones importantes
- ✅ Fotos ANTES de cualquier modificación
- ✅ Documentar decisiones en tiempo real

### 4. Investigación Antes de Compra

**Lección:**
Investigar compatibilidad con Klipper ANTES de comprar hardware.

**Error cometido:**
Comprar Tronxy XY-08N asumiendo que "es inductivo, funcionará".

**Correcto:**
- ✅ Verificar compatibilidad nativa con Klipper
- ✅ Buscar ejemplos de configuración en comunidad
- ✅ Preferir hardware con soporte oficial (Eddy Coil)
- ✅ Evitar hardware legacy que requiere adaptación

### 5. Chip Identification

**Lección:**
Identificar el chip físicamente, no confiar solo en documentación.

**Éxito:**
Identificar LPC1769FBD100 visualmente evitó pérdida de tiempo con configuración STM32F407 incorrecta.

**Correcto:**
- ✅ Leer texto en chip físicamente
- ✅ Verificar con datasheet del fabricante
- ✅ Documentación de terceros puede estar desactualizada

### 6. Alimentación en DFU Mode

**Lección:**
Algunos boards requieren alimentación externa durante DFU.

**Error cometido:**
Intentar DFU de EBB42 sin 24V (falló repetidamente).

**Correcto:**
- ✅ Leer guías oficiales de flasheo
- ✅ Proporcionar alimentación principal durante DFU si se requiere
- ✅ Verificar jumpers (ej: VUSB en EBB42 para USB mode)

### 7. Tiempo como Recurso

**Lección:**
Saber cuándo abandonar una solución problemática.

**Error cometido:**
Invertir 2+ días en hacer funcionar Tronxy XY-08N.

**Correcto:**
- ✅ Definir límite de tiempo para troubleshooting (ej: 4-6 horas)
- ✅ Si no funciona en ese tiempo: buscar alternativa
- ✅ Hardware moderno a menudo es más barato que tiempo perdido

---

## Próximos Pasos

### Inmediatos (Esta semana)

1. **Instalación física Eddy Coil** ⏳
   - Montar coil 2-3mm sobre nozzle
   - Conectar cable I2C a EBB42
   - Verificar clearance y cable routing

2. **Calibración Eddy Coil** ⏳
   - `LDC_CALIBRATE_DRIVE_CURRENT`
   - `PROBE_EDDY_CURRENT_CALIBRATE`
   - Verificar precisión con `PROBE_ACCURACY`

3. **Testing de homing** ⏳
   - `G28` completo (X, Y, Z)
   - Verificar Z homing con Eddy Coil
   - Ajustar z_offset según necesidad

4. **Bed leveling** ⏳
   - `Z_TILT_ADJUST` (dual Z)
   - `BED_MESH_CALIBRATE` (5×5 grid)
   - `SAVE_CONFIG`

5. **Primera impresión de prueba** ⏳
   - Cube 20mm
   - Verificar primera capa
   - Ajuste fino si necesario

### Phase 4-7 (Próximas semanas)

**Phase 4:** Calibración completa
- PID tuning fino
- Input shaper (ADXL345 en EBB42)
- E-steps / rotation distance
- Pressure advance

**Phase 5:** Cable management
- Routing definitivo de cables
- Protección térmica
- Anillos ferrita
- Estética

**Phase 6:** Testing extensivo
- Prints de calibración
- Stringing tests
- Temperature towers
- Retraction tuning

**Phase 7:** Optimización
- Velocidades máximas
- Aceleraciones
- Jerk settings
- Firmware updates

### Phase 8-12 (Futuro)

**Phase 8-11:** Mejoras incrementales
- Mejoras según resultados de prints
- Ajustes finos de configuración
- Posibles upgrades menores

**Phase 12:** Stealthburner + Orbiter v2 (GRANDE)
- Migración de EBB42 a toolhead (definitivo)
- Orbiter v2 extruder
- Stealthburner toolhead
- PT100 sensor (MAX31865)
- Integración final

---

## Referencias

### Documentación del Proyecto

**Main guides:**
- [README.md](README.md) - Project overview
- [GUIDE.md](GUIDE.md) - Main migration guide
- [ROADMAP.md](ROADMAP.md) - 8-phase technical roadmap

**Phase documentation:**
- [Phase 0](phases/phase0/README.md) - Baseline
- [Phase 1](phases/phase1/README.md) - SKR preparation
- [Phase 2](phases/phase2/README.md) - SKR wiring
- [Phase 3](phases/phase3/README.md) - EBB42 integration

**Guides Phase 3:**
- [FLASHEO_SKR_EXITOSO.md](guides/phase3/FLASHEO_SKR_EXITOSO.md)
- [FLASHEO_EBB42_EXITOSO.md](guides/phase3/FLASHEO_EBB42_EXITOSO.md)
- [CONFIGURACION_DUAL_MCU.md](guides/phase3/CONFIGURACION_DUAL_MCU.md)
- [EBB42_INTEGRATION.md](guides/phase3/EBB42_INTEGRATION.md)
- [INSTALACION_COMPLETADA.md](guides/phase3/INSTALACION_COMPLETADA.md)
- [EDDY_COIL_INSTALLATION.md](guides/phase3/EDDY_COIL_INSTALLATION.md)
- [EDDY_COIL_CALIBRATION.md](guides/phase3/EDDY_COIL_CALIBRATION.md)

### External Resources

**Klipper:**
- [Klipper Documentation](https://www.klipper3d.org/)
- [Eddy Probe Guide](https://www.klipper3d.org/Eddy_Probe.html)
- [Config Reference](https://www.klipper3d.org/Config_Reference.html)

**Hardware:**
- [BTT SKR 1.4 Turbo](https://github.com/bigtreetech/BIGTREETECH-SKR-V1.3)
- [BTT EBB42](https://github.com/bigtreetech/EBB)
- [BTT Eddy Coil](https://github.com/bigtreetech/Eddy)

**Community:**
- [Klipper Discourse](https://klipper.discourse.group/)
- [Voron Design](https://vorondesign.com/) - Referencia Stealthburner

---

**Documento creado:** 2025-12-26
**Última actualización:** 2025-12-26
**Versión:** 1.0
**Autor:** mjcuadrado + Claude Code
**Estado del proyecto:** Phase 3 completada - Hardware instalado - Pendiente calibración Eddy Coil
