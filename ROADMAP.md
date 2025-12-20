# ROADMAP COMPLETO: Migración Tronxy X5SA a Klipper con SKR 1.4 Turbo

**Proyecto:** Migración profesional y documentada Tronxy X5SA/Pro → Klipper
**Hardware objetivo:** BTT SKR 1.4 Turbo + TMC2209 + EBB42 CAN
**Filosofía:** Manual "para tontos", paso a paso, sin asumir conocimientos

---

## RESUMEN TÉCNICO DEL HARDWARE

### Impresora Base: Tronxy X5SA / X5SA Pro

**Cinemática:** CoreXY
**Volumen:** 330×330×400mm
**Voltaje:** 24V DC
**Estructura:** Perfil aluminio con guías lineales

**Motores originales:**
- X/Y (CoreXY): 2× motores NEMA17, ~1.1A corriente típica
- Z: 1× motor NEMA17 con tornillo/husillo
- Extrusor: 1× motor NEMA17, ~1.05A

**Problemas conocidos del stock:**
- Firmware Chitu limitado y con bugs
- Z-offset inaccesible después de actualizaciones
- Auto-leveling defectuoso (crash con cama)
- Drivers ruidosos y configuración pobre

### Electrónica Nueva: BTT SKR 1.4 Turbo

**MCU:** NXP LPC1769 ARM Cortex-M3 @ 120MHz
**Drivers:** 5× slots para TMC2209 (UART)
**Voltaje:** 12-24V DC (compatible con 24V stock)
**Ventajas:** 20% más rápida que SKR 1.4, pinout idéntico, excelente soporte Klipper

### Drivers: TMC2209

**Modo:** UART (control completo por software)
**Corriente configurable:** 650mA - 1200mA
**Features:** StealthChop (silencioso), StallGuard (sensorless homing)

### Toolhead: BTT EBB42 CAN V1.2

**MCU:** STM32G0B1 @ 64MHz
**Comunicación:** CAN Bus (4 cables) o USB
**Integrado:** TMC2209, ADXL345, MAX31865
**Ventaja:** Reduce 20-24 cables a solo 4 cables

---

## ESTRUCTURA DE FASES

### PHASE 0 ✅ (Completada)
**Estado:** Baseline, inventario, auditoría
**Documentación:** Estado inicial de impresora stock

### PHASE 1 🔄 (En curso)
**Nombre:** SKR 1.4 Turbo - Preparación de electrónica
**Objetivo:** Placa lista con drivers, SIN cablear
**Duración estimada:** 2-3 horas

**Pasos:**
1. SKR stock documentada
2. Jumpers UART configurados (10 jumpers)
3. Orientación drivers documentada
4. Drivers instalados físicamente
5. Verificación visual final

**Resultado:** SKR con 5× TMC2209 correctamente instalados, sin cables, sin alimentación

### PHASE 2 📋 (Pendiente)
**Nombre:** Cableado básico (motores + endstops + alimentación)
**Objetivo:** Impresora moviendo ejes sin hotend
**Duración estimada:** 4-6 horas

**Pasos:**
1. Documentar pinout completo SKR 1.4 Turbo
2. Etiquetar TODOS los cables del stock
3. Desconectar electrónica stock (fotos antes/después)
4. Cablear alimentación 24V + fusibles
5. Cablear motores X, Y, Z (identificar fase A/B)
6. Cablear endstops X, Y, Z
7. Verificación continuidad (multímetro)
8. Primera alimentación (sin motores conectados)
9. Conectar motores y probar con `STEPPER_BUZZ`

**Resultado:** Ejes X, Y, Z moviéndose correctamente, homing funcional

### PHASE 3 📋 (Pendiente)
**Nombre:** Firmware Klipper + configuración básica
**Objetivo:** Klipper corriendo en SKR 1.4 Turbo
**Duración estimada:** 2-3 horas

**Pasos:**
1. Compilar firmware Klipper (menuconfig para LPC1769 @ 120MHz)
2. Flashear vía SD card (firmware.bin)
3. Configurar printer.cfg base (CoreXY)
4. Configurar TMC2209 en modo UART
5. Tuning corriente de motores
6. Configurar endstops y homing
7. Probar movimientos manuales
8. Calibrar pasos/mm (rotation_distance)

**Resultado:** Impresora respondiendo a comandos Klipper, movimientos precisos

### PHASE 4 📋 (Pendiente)
**Nombre:** Cama caliente + sensor inductivo
**Objetivo:** Bed heating + Z probing funcional
**Duración estimada:** 2-3 horas

**Pasos:**
1. Cablear cama caliente (verificar potencia <144W o usar MOSFET externo)
2. Cablear termistor de cama
3. Configurar heater_bed en Klipper
4. PID tuning de cama
5. Instalar sensor inductivo Omron TL-Q5MC1-Z
6. Cablear sensor (Marrón=24V, Azul=GND, Negro=Señal)
7. Configurar [probe] en Klipper
8. Calibrar X/Y offset
9. Calibrar Z offset con PROBE_CALIBRATE
10. Crear bed mesh (5×5 o 7×7)

**Resultado:** Cama calentando correctamente, nivelación automática funcional

### PHASE 5 📋 (Pendiente)
**Nombre:** Toolhead stock (temporal)
**Objetivo:** Primera impresión con hotend stock
**Duración estimada:** 3-4 horas

**Pasos:**
1. Cablear motor extrusor
2. Cablear hotend heater
3. Cablear termistor hotend
4. Configurar [extruder] en Klipper
5. PID tuning hotend
6. Calibrar rotation_distance (test de extrusión)
7. Cablear ventiladores (hotend fan + part cooling)
8. Test de temperatura (verificar lecturas)
9. Test de extrusión en frío
10. Primera impresión de prueba (cubo de calibración)

**Resultado:** Impresora funcional con hardware stock, calidad baseline establecida

### PHASE 6 📋 (Pendiente)
**Nombre:** Instalación DC-DC converter + preparación CAN
**Objetivo:** Alimentación estable 24V para EBB42
**Duración estimada:** 2 horas

**Pasos:**
1. Instalar DC-DC XL4015 (24V → 24V regulado)
2. Calibrar voltaje salida (exactamente 24V)
3. Cablear salida a conector dedicado para EBB42
4. Configurar terminación CAN (resistencia 120Ω)
5. Preparar cables CAN (CAN_H, CAN_L, 24V, GND)
6. Documentar pinout CAN en SKR 1.4 Turbo

**Resultado:** Alimentación estable lista para EBB42

### PHASE 7 📋 (Pendiente)
**Nombre:** Instalación física EBB42 en toolhead
**Objetivo:** EBB42 montada, sin configurar
**Duración estimada:** 3-4 horas

**Pasos:**
1. Diseñar/imprimir soporte para EBB42
2. Instalar EBB42 en toolhead
3. Verificar aislamiento eléctrico (no tocar metal)
4. Pasar cables CAN por drag chain
5. Conectar CAN bus (verificar polaridad)
6. Flashear firmware Klipper en EBB42 (modo CAN)
7. Obtener canbus_uuid con `canbus_query.py`
8. Configurar [mcu EBBCan] en printer.cfg

**Resultado:** EBB42 comunicando vía CAN, lista para conectar componentes

### PHASE 8 📋 (Pendiente)
**Nombre:** Migración extrusor a EBB42
**Objetivo:** Motor extrusor controlado por EBB42
**Duración estimada:** 2 horas

**Pasos:**
1. Desconectar motor extrusor de SKR
2. Conectar motor a EBB42
3. Configurar [extruder] con pins EBB42
4. Configurar [tmc2209 extruder] en UART
5. Probar movimiento con `STEPPER_BUZZ`
6. Test de extrusión
7. Recalibrar rotation_distance si es necesario

**Resultado:** Extrusor funcionando desde EBB42

### PHASE 9 📋 (Pendiente)
**Nombre:** Instalación PT100 + MAX31865
**Objetivo:** Sensor de temperatura de alta precisión
**Duración estimada:** 2-3 horas

**Pasos:**
1. Remover termistor stock del hotend
2. Instalar cartucho PT100 (verificar contacto térmico)
3. Cablear PT100 a EBB42 (modo 3-wire recomendado)
4. Configurar MAX31865 en printer.cfg
5. Verificar lecturas de temperatura ambiente
6. PID tuning con PT100
7. Comparar precisión vs termistor stock
8. Test de calentamiento (0°C → 250°C)

**Resultado:** Hotend con sensor PT100, precisión ±1°C

### PHASE 10 📋 (Pendiente)
**Nombre:** ADXL345 + Input Shaper
**Objetivo:** Optimización de vibraciones
**Duración estimada:** 2-3 horas

**Pasos:**
1. Verificar ADXL345 integrado en EBB42
2. Configurar [adxl345] en printer.cfg
3. Probar con `ACCELEROMETER_QUERY`
4. Ejecutar `TEST_RESONANCES AXIS=X`
5. Ejecutar `TEST_RESONANCES AXIS=Y`
6. Generar gráficos de resonancia
7. Analizar frecuencias problemáticas
8. Configurar [input_shaper]
9. Probar impresión a alta velocidad (100-150 mm/s)
10. Ajustar max_accel según resultados

**Resultado:** Input shaping configurado, vibraciones eliminadas

### PHASE 11 📋 (Pendiente)
**Nombre:** Calibraciones finales
**Objetivo:** Sistema completamente calibrado
**Duración estimada:** 4-6 horas

**Pasos:**
1. Pressure Advance tuning
2. Retraction tuning
3. Flow rate calibration
4. Temperature tower (PLA, PETG, ABS)
5. Bed mesh refinado (7×7 o 9×9)
6. Z offset fine-tuning
7. First layer calibration
8. Linear advance (si procede)
9. Macros personalizadas (START_PRINT, END_PRINT)
10. Test prints completos

**Resultado:** Impresora optimizada, lista para producción

### PHASE 12 📋 (Futuro)
**Nombre:** Upgrades "PRO" (opcional)
**Objetivo:** Hardware premium

**Posibles mejoras:**
- Fleje PEI magnético
- Extrusor Orbiter v2 o similar
- Hotend de alta temperatura (Dragon, Rapido)
- Ventiladores Noctua (silenciosos)
- Neopixels / iluminación
- Cámara con timelapse
- Sensores de filamento

**Criterio:** Solo después de estabilidad completa

---

## MATRIZ DE DEPENDENCIAS

| Fase | Depende de | Bloquea a | Reversible |
|------|------------|-----------|------------|
| 0 | - | 1 | ✅ |
| 1 | 0 | 2 | ✅ |
| 2 | 1 | 3 | ⚠️ Requiere desconexión completa |
| 3 | 2 | 4 | ✅ |
| 4 | 3 | 5 | ✅ |
| 5 | 4 | 6 | ✅ |
| 6 | 5 | 7 | ✅ |
| 7 | 6 | 8 | ⚠️ Requiere desmontaje toolhead |
| 8 | 7 | 9 | ✅ |
| 9 | 8 | 10 | ⚠️ Requiere cambio de hotend |
| 10 | 9 | 11 | ✅ |
| 11 | 10 | 12 | ✅ |
| 12 | 11 | - | ✅ |

---

## HERRAMIENTAS NECESARIAS

### Herramientas básicas
- [ ] Destornilladores (Phillips, plano)
- [ ] Juego de llaves Allen/hexagonales
- [ ] Pinzas de punta fina
- [ ] Cortaalambres/pelacables
- [ ] Crimpadora (para Dupont/JST)

### Herramientas de medición
- [ ] Multímetro digital
- [ ] Calibre (pie de rey)
- [ ] Nivel (para verificar estructura)

### Herramientas de soldadura (opcional)
- [ ] Soldador 60W
- [ ] Estaño 60/40
- [ ] Flux
- [ ] Desoldador/trenza

### Materiales consumibles
- [ ] Cables AWG22 (rojo, negro, amarillo, azul)
- [ ] Conectores Dupont
- [ ] Conectores JST-XH
- [ ] Fundas PET expandibles
- [ ] Mangas trenzadas
- [ ] Etiquetas para cables
- [ ] Cinta Kapton
- [ ] Zip ties
- [ ] Tornillería M3, M4, M5

### Seguridad
- [ ] Alfombrilla ESD
- [ ] Pulsera antiestática
- [ ] Gafas de seguridad
- [ ] Guantes (para manipular vidrio/metal)

---

## ESTIMACIÓN TEMPORAL

| Fase | Tiempo mínimo | Tiempo realista | Tiempo máximo |
|------|---------------|-----------------|---------------|
| 1 | 2h | 3h | 5h |
| 2 | 4h | 6h | 10h |
| 3 | 2h | 3h | 5h |
| 4 | 2h | 3h | 5h |
| 5 | 3h | 4h | 6h |
| 6 | 1h | 2h | 3h |
| 7 | 3h | 4h | 6h |
| 8 | 1h | 2h | 3h |
| 9 | 2h | 3h | 5h |
| 10 | 2h | 3h | 5h |
| 11 | 4h | 6h | 10h |
| 12 | Variable | Variable | Variable |
| **TOTAL** | **26h** | **39h** | **63h** |

**Recomendación:** Planificar 5-7 días de trabajo con sesiones de 4-6 horas, permitiendo descansos y troubleshooting.

---

## PUNTOS DE VERIFICACIÓN CRÍTICOS

### Antes de alimentar por primera vez
- [ ] Verificar polaridad 24V (multímetro)
- [ ] Verificar no hay cortocircuitos (continuidad)
- [ ] Todos los drivers correctamente orientados
- [ ] Jumpers UART correctos (10 jumpers)
- [ ] No hay cables sueltos o pelados
- [ ] Fusibles correctos (15A-20A)

### Antes de mover motores
- [ ] Corriente TMC configurada (no exceder spec motor)
- [ ] Endstops funcionando (`M119`)
- [ ] Homing_speed bajo (20-30 mm/s inicial)
- [ ] Verificar dirección de motores

### Antes de calentar
- [ ] Termistores leyendo temperatura ambiente correcta
- [ ] PID configurado
- [ ] Thermal runaway protection habilitado
- [ ] Max_temp conservador (250°C inicial)

### Antes de primera impresión
- [ ] Z offset calibrado (papel test)
- [ ] Bed mesh cargado
- [ ] Flow rate ~90-100%
- [ ] Test de extrusión correcto (100mm → 100mm real)

---

## RECURSOS DE REFERENCIA

### Documentación oficial
- [Klipper Documentation](https://www.klipper3d.org/)
- [BTT SKR 1.4 Turbo GitHub](https://github.com/bigtreetech/BIGTREETECH-SKR-V1.3/tree/master/BTT%20SKR%20V1.4)
- [BTT EBB42 GitHub](https://github.com/bigtreetech/EBB)
- [TMC2209 Datasheet](https://www.trinamic.com/products/integrated-circuits/details/tmc2209-la/)

### Configuraciones de referencia
- [Klipper Tronxy X5SA Pro config](https://github.com/Klipper3d/klipper/blob/master/config/printer-tronxy-x5sa-pro-2020.cfg)
- [Community configs](https://github.com/markcarroll/tronxy-x5sa)

### Guías y tutoriales
- [Voron Wiring Guide](https://docs.vorondesign.com/build/electrical/v2_skr14_wiring.html)
- [Klipper Input Shaper](https://www.klipper3d.org/Measuring_Resonances.html)
- [TMC Drivers Klipper](https://www.klipper3d.org/TMC_Drivers.html)

---

## FILOSOFÍA DEL PROYECTO

1. **Primero estabilidad, luego velocidad**
2. **Documentar TODO (fotos + commits)**
3. **Un paso a la vez, verificar antes de avanzar**
4. **Nunca forzar hardware**
5. **Si algo no funciona: PARAR, analizar, preguntar**
6. **No mezclar fases**
7. **Rollback siempre posible**

---

**Última actualización:** 2025-12-20
**Versión:** 1.0
**Próxima revisión:** Después de completar Phase 1
