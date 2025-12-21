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
- Z: 2× motores NEMA17 con leadscrew (dual Z independiente)
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

### PHASE 1 ✅ (Completada 2025-12-20)
**Nombre:** SKR 1.4 Turbo - Preparación de electrónica
**Objetivo:** Placa lista con drivers, SIN cablear
**Duración real:** 2 horas

**Pasos completados:**
1. SKR stock documentada
2. Jumpers UART configurados (10 jumpers MS3)
3. Orientación drivers documentada
4. Drivers instalados físicamente
5. Verificación visual final

**Resultado:** SKR con 5× TMC2209 correctamente instalados, disipadores, sin cables, sin alimentación

### PHASE 2 ✅ (Completada 2025-12-21)
**Nombre:** SKR Cableado Básico
**Objetivo:** SKR montada y cableada (motores, cama, alimentación)
**Duración real:** 6 horas

**Decisión arquitectónica crítica:**
- SKR montada en posición superior (frame superior)
- EBB42 CAN integrado desde inicio (no toolhead stock temporal)
- Sensorless homing X/Y (TMC2209 StallGuard, sin endstops físicos)
- Sensor Z en EBB42 (Omron TL-Q5MC1-Z)

**Pasos completados:**
1. Documentación wiring stock completo (31 fotos)
2. Desconexión completa electrónica stock
3. Montaje SKR posición superior + fabricación extensiones:
   - Cable extensión Motor Z2 (JST-XH 4-pin, 60cm)
   - Cable extensión 24V (50cm, termorretráctil rojo/azul)
4. Cableado básico SKR:
   - Alimentación 24V → DCIN
   - Motores X, Y, Z1, Z2 (E1 para segundo Z)
   - Cama caliente (power HB + termistor TB)
5. Verificación final (36 fotos totales)

**Resultado:** SKR cableada y lista para EBB42 CAN (Phase 3)

### PHASE 3 📋 (En curso) ⬅️ SIGUIENTE
**Nombre:** Toolhead EBB42 CAN
**Objetivo:** Toolhead completo con comunicación CAN bus
**Duración estimada:** 4-6 horas

**Pasos planeados:**
1. Documentar toolhead stock actual (fotos)
2. Desconectar cables toolhead de stock
3. Instalar EBB42 en toolhead (diseñar/imprimir soporte si necesario)
4. Conectar componentes a EBB42:
   - Motor extrusor (E0 driver integrado)
   - Calentador hotend
   - Termistor hotend (o PT100 directo)
   - Ventiladores (hotend fan + part cooling)
   - Sensor Omron TL-Q5MC1-Z (probe Z)
5. Fabricar cable CAN (4 hilos):
   - Cat6 para CAN_H/CAN_L (par trenzado)
   - Cable alimentación separado para 24V/GND
   - Identificación con termorretráctil
6. Tender cable CAN desde SKR a toolhead
7. Conectar CAN bus (verificar polaridad, terminación 120Ω)

**Resultado:** Toolhead con EBB42 montado, cableado, listo para firmware

### PHASE 4 📋 (Pendiente)
**Nombre:** Firmware Klipper + Configuración Completa
**Objetivo:** Klipper corriendo en SKR + EBB42, sistema funcional
**Duración estimada:** 3-4 horas

**Pasos:**
1. **Compilar y flashear firmware:**
   - Klipper para SKR 1.4 Turbo (LPC1769 @ 120MHz)
   - Klipper para EBB42 CAN (STM32G0B1, modo CAN)
   - Obtener canbus_uuid con `canbus_query.py`

2. **Configurar printer.cfg base:**
   - MCU principal (SKR) + MCU toolhead (EBB42)
   - Cinemática CoreXY
   - TMC2209 en modo UART (X, Y, Z1, Z2, E0)
   - Sensorless homing X/Y (StallGuard)
   - Dual Z independiente

3. **Configurar toolhead:**
   - Extrusor (motor + heater + termistor/PT100)
   - Ventiladores (hotend + part cooling)
   - Probe Omron (Z homing + bed mesh)

4. **Configurar cama caliente:**
   - Heater_bed + termistor
   - PID tuning cama

5. **Calibraciones iniciales:**
   - Tuning corriente TMC2209
   - Sensorless homing sensitivity
   - Probe Z offset
   - Bed mesh básico (5×5)
   - PID tuning hotend
   - Rotation_distance extrusor

**Resultado:** Sistema completamente funcional, listo para primera impresión

### PHASE 5 📋 (Pendiente)
**Nombre:** Primera Impresión + Calibraciones Básicas
**Objetivo:** Primera impresión exitosa + ajustes básicos
**Duración estimada:** 3-4 horas

**Pasos:**
1. Verificación pre-impresión completa
2. Test de extrusión (100mm → 100mm real)
3. First layer calibration (papel test)
4. Impresión calibración (cubo XYZ 20mm)
5. Ajustes flow rate inicial
6. Retraction tuning básico
7. Temperature tower (material preferido)
8. Bed mesh refinado si necesario
9. Macros básicas (START_PRINT, END_PRINT)
10. Test prints funcionales

**Resultado:** Impresora imprimiendo correctamente, calidad baseline establecida

### PHASE 6 📋 (Pendiente)
**Nombre:** Input Shaper + ADXL345
**Objetivo:** Optimización de vibraciones y alta velocidad
**Duración estimada:** 2-3 horas

**Pasos:**
1. Verificar ADXL345 integrado en EBB42
2. Configurar [adxl345] en printer.cfg
3. Ejecutar `TEST_RESONANCES AXIS=X`
4. Ejecutar `TEST_RESONANCES AXIS=Y`
5. Generar gráficos de resonancia
6. Analizar frecuencias problemáticas
7. Configurar [input_shaper] con valores óptimos
8. Ajustar max_accel y max_velocity
9. Test de ringing/ghosting
10. Impresión a alta velocidad (100-150 mm/s)

**Resultado:** Input shaping configurado, vibraciones eliminadas, alta velocidad

### PHASE 7 📋 (Pendiente)
**Nombre:** Calibraciones Finales + Optimización
**Objetivo:** Sistema completamente optimizado
**Duración estimada:** 4-6 horas

**Pasos:**
1. Pressure Advance tuning fino
2. Retraction tuning avanzado
3. Flow rate calibration por material
4. Bed mesh refinado (7×7 o 9×9)
5. Z offset fine-tuning
6. Sensorless homing optimization
7. Temperature towers múltiples materiales
8. Macros avanzadas (limpieza nozzle, purge, etc.)
9. Test prints de calidad (benchy, torture test)
10. Documentación parámetros finales

**Resultado:** Impresora completamente calibrada y optimizada

### PHASE 8 📋 (Futuro - Opcional)
**Nombre:** Upgrades "PRO"
**Objetivo:** Hardware premium y mejoras opcionales

**Posibles mejoras:**
- Fleje PEI magnético (mejor adherencia)
- Extrusor Orbiter v2 (más ligero, mejor retracción)
- Hotend alta temperatura Dragon/Rapido (250°C+)
- Ventiladores Noctua (silenciosos)
- Neopixels/LED strips (iluminación)
- Cámara + timelapse (Crowsnest/Mainsail)
- Sensores filamento (runout detection)
- Cable chains mejoradas
- Drag chain para cables

**Criterio:** Solo después de estabilidad completa (Phase 7 terminada)

---

## MATRIZ DE DEPENDENCIAS

| Fase | Depende de | Bloquea a | Reversible | Notas |
|------|------------|-----------|------------|-------|
| 0 | - | 1 | ✅ | Documentación |
| 1 | 0 | 2 | ✅ | SKR preparación |
| 2 | 1 | 3 | ⚠️ | Requiere desconexión stock completa |
| 3 | 2 | 4 | ⚠️ | Montaje EBB42, cables stock no reversibles |
| 4 | 3 | 5 | ✅ | Firmware y configuración |
| 5 | 4 | 6 | ✅ | Primera impresión |
| 6 | 5 | 7 | ✅ | Input Shaper (requiere impresora funcional) |
| 7 | 6 | 8 | ✅ | Calibraciones finales |
| 8 | 7 | - | ✅ | Upgrades opcionales

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

| Fase | Tiempo estimado | Tiempo real | Estado |
|------|----------------|-------------|--------|
| 0 | - | - | ✅ Completada |
| 1 | 2-3h | 2h | ✅ Completada |
| 2 | 4-6h | 6h | ✅ Completada |
| 3 | 4-6h | - | 📋 En curso |
| 4 | 3-4h | - | 📋 Pendiente |
| 5 | 3-4h | - | 📋 Pendiente |
| 6 | 2-3h | - | 📋 Pendiente |
| 7 | 4-6h | - | 📋 Pendiente |
| 8 | Variable | - | 📋 Opcional |
| **TOTAL (1-7)** | **22-32h** | **8h completadas** | **14-24h restantes** |

**Tiempo invertido:** 8 horas (Phases 1-2)
**Tiempo restante estimado:** 14-24 horas (Phases 3-7)
**Total proyecto:** 22-32 horas hasta impresora completamente calibrada

**Recomendación:** Planificar 4-6 días adicionales con sesiones de 3-5 horas, permitiendo descansos y troubleshooting.

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

**Última actualización:** 2025-12-21
**Versión:** 2.0 (actualizado tras completar Phase 2)
**Próxima revisión:** Después de completar Phase 3 (EBB42 CAN)

**Cambios v2.0:**
- Corregido: Dual Z (2 motores, no 1)
- Actualizado: Arquitectura EBB42 CAN desde inicio (no toolhead stock temporal)
- Reorganizado: Fases reducidas de 12 a 8 (más coherente)
- Actualizado: Sensorless homing X/Y (sin endstops físicos)
- Eliminado: Phase DC-DC converter (innecesario, PSU ya da 24V estable)
- Fusionado: PT100 + extrusor + probe en Phase 3 (todo en EBB42 de una vez)
