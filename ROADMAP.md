# ROADMAP v3.0 - Metodología 4-8-12
**Proyecto:** Migración Tronxy X5SA a Klipper (SKR 1.4 Turbo + EBB42)
**Filosofía:** INSTALAR → CALIBRAR → VALIDAR (4-8-12 hrs)
**Versión:** 3.0
**Fecha:** 2025-12-27

---

## 🎯 FILOSOFÍA DEL PROYECTO

### **Patrón de cada grupo de fases:**

```
┌─────────────┐
│  INSTALAR   │  Hardware físico montado y funcionando básicamente
└──────┬──────┘
       │
       ▼
┌─────────────┐
│  CALIBRAR   │  Software configurado, perfiles creados, sistema optimizado
└──────┬──────┘
       │
       ▼
┌─────────────┐
│  VALIDAR    │  Impresiones progresivas 4-8-12 horas SIN FALLOS
│  (4-8-12)   │  Valida que todo funciona en producción real
└─────────────┘
```

### **Regla 4-8-12:**

Cada validación consiste en **3 impresiones progresivamente más largas:**

1. **4 horas:** Primera validación, detectar problemas básicos
2. **8 horas:** Validación media, estabilidad térmica
3. **12 horas:** Validación larga, confiabilidad completa

**Criterio de éxito:** Las 3 impresiones se completan **sin intervención, sin fallos, con calidad consistente**.

---

## 📊 RESUMEN VISUAL DE FASES

```
GRUPO 1: HARDWARE STOCK (Phases 0-5)
──────────────────────────────────────
Phase 0-2: ✅ Base electrónica
Phase 3:   📋 INSTALAR Eddy → G28 funcional
Phase 4:   ⏳ CALIBRAR Stock → Perfiles funcionales
Phase 5:   ⏳ VALIDAR 4-8-12 → Stock 100% ✅

GRUPO 2: HARDWARE VORON (Phases 6-8)
──────────────────────────────────────
Phase 6:   🔮 INSTALAR Voron → Toolhead nuevo
Phase 7:   🔮 CALIBRAR Voron → Perfiles Orbiter
Phase 8:   🔮 VALIDAR 4-8-12 → Voron 100% ✅

GRUPO 3: MULTICOLOR (Phases 9-11)
──────────────────────────────────────
Phase 9:   🔮 INSTALAR Multicolor → MMU/AMS
Phase 10:  🔮 CALIBRAR Multicolor → Perfiles multi
Phase 11:  🔮 VALIDAR 4-8-12 → Multicolor 100% ✅

FINALIZACIÓN (Phase 12)
──────────────────────────────────────
Phase 12:  🔮 Optimización final → PROYECTO COMPLETO 🏆
```

---

## ✅ PHASES 0-2: BASE ELECTRÓNICA (COMPLETADAS)

### **Phase 0: Baseline** ✅
- **Objetivo:** Documentación estado inicial
- **Duración:** -
- **Estado:** Completada
- **Tag:** `phase0-baseline`

### **Phase 1: SKR Preparación** ✅
- **Objetivo:** SKR con drivers instalados, sin cables
- **Duración:** 2 horas
- **Estado:** Completada 2025-12-20
- **Resultado:** SKR 1.4 Turbo + 5× TMC2209 listos

### **Phase 2: SKR Wiring** ✅
- **Objetivo:** SKR cableada (motores, cama, PSU)
- **Duración:** 6 horas
- **Estado:** Completada 2025-12-21
- **Resultado:** SKR operativa, motores conectados

---

## 📋 PHASE 3: INSTALAR EDDY → G28 FUNCIONAL

**Objetivo:** Máquina hace home correctamente con nueva electrónica

**Hardware objetivo:**
- SKR 1.4 Turbo ✅ (ya instalada)
- EBB42 CAN V1.2 ✅ (ya flasheada, montada detrás)
- Eddy Coil V1.0 ⏳ (pendiente instalación física)
- **Todo lo demás STOCK:** Titan clone, E3D V6 clone, fans stock

### **Tareas:**

**Software (YA COMPLETADO):**
- [x] Firmware SKR flasheado
- [x] Firmware EBB42 flasheado
- [x] printer.cfg base configurado
- [x] Eddy Coil I2C configurado (PB3/PB4)
- [x] Macros básicas creadas

**Hardware (PENDIENTE):**
1. [ ] **Fotografiar estado actual toolhead** (con XY-08N si aún instalado)
2. [ ] **Desmontar XY-08N** (si presente)
3. [ ] **Instalar Eddy Coil físicamente** (2-3mm sobre nozzle)
4. [ ] **Conectar cables I2C** (4 wires: VCC, GND, SCL, SDA)
5. [ ] **Routing cable por drag chain** (desde toolhead → EBB42 detrás)
6. [ ] **Verificar QUERY_PROBE** responde
7. [ ] **Calibrar drive current:** `LDC_CALIBRATE_DRIVE_CURRENT CHIP=btt_eddy`
8. [ ] **Calibrar Z offset básico:** `PROBE_EDDY_CURRENT_CALIBRATE CHIP=btt_eddy`

**Testing:**
- [ ] **G28 X** funciona (sensorless homing)
- [ ] **G28 Y** funciona (sensorless homing)
- [ ] **G28 Z** funciona (con Eddy Coil)
- [ ] **G28** completo funciona

### **Criterio de éxito Phase 3:**
✅ Comando `G28` completa sin errores
✅ Eddy detecta metal correctamente (QUERY_PROBE)
✅ Z homing funciona con Eddy
✅ Máquina puede moverse en XYZ

### **Deliverables:**
- 📦 Fotos completas instalación Eddy
- 📦 LOG_INSTALACION.md completado
- 📦 printer.cfg con Eddy funcional
- 📦 Sistema listo para calibración (Phase 4)

**Duración estimada:** 4-6 horas
**Estado:** 📋 EN CURSO
**Documentación:** [guides/phase3/](guides/phase3/)

---

## ⏳ PHASE 4: CALIBRAR STOCK → PERFILES FUNCIONALES

**Objetivo:** Máquina stock completamente calibrada con perfiles listos

**Hardware:** Extrusor Titan clone stock, E3D V6 clone, fans stock

### **Tareas - Calibración Klipper:**

**Térmica:**
1. [ ] **PID tuning hotend** @ 210°C (PLA)
   - Comando: `PID_TUNE_HOTEND TARGET=210`
   - Resultado: Kp, Ki, Kd
2. [ ] **PID tuning bed** @ 60°C
   - Comando: `PID_TUNE_BED TARGET=60`
   - Resultado: Kp, Ki, Kd

**Mecánica:**
3. [ ] **E-steps calibration** (rotation_distance)
   - Test 100mm extrusion
   - Ajustar rotation_distance (actual: 22.478 Titan clone)
4. [ ] **Z-Tilt adjustment**
   - Comando: `Z_TILT_ADJUST`
   - Dual Z leveling
5. [ ] **Bed mesh rapid_scan** (5×5)
   - Comando: `BED_MESH_CALIBRATE METHOD=rapid_scan`
   - Tiempo: ~15 segundos
   - Analizar rango (ideal <0.5mm)

**Calidad:**
6. [ ] **Pressure advance** (Titan clone direct drive)
   - Ellis' pattern generator
   - Rango típico: 0.03-0.05
7. [ ] **Retraction tuning** (direct drive)
   - Retraction tower
   - Rango típico: 1.0-2.0mm @ 40mm/s
8. [ ] **Flow rate calibration**
   - Starting point: 100%
   - Ajustar por marca de filamento

**Avanzado:**
9. [ ] **Input Shaper** (ADXL345 en EBB42)
   - `TEST_RESONANCES AXIS=X`
   - `TEST_RESONANCES AXIS=Y`
   - Configurar shaper_freq y shaper_type
10. [ ] **Velocidades y aceleraciones**
    - Test max_velocity actual: 300mm/s
    - Test max_accel actual: 3000mm/s²
    - Ajustar según input shaper

### **Tareas - Perfiles OrcaSlicer:**

**Estado actual:**
- ✅ Perfiles ya creados en `orca-slicer-profiles/`
  - Printer: `printer_tronxy_x5sa_klipper.json`
  - Filaments: PLA, PETG, ABS
  - Process: Draft (0.3mm), Standard (0.2mm), Fine (0.1mm)

**Ajustes necesarios:**
11. [ ] **Importar perfiles** en OrcaSlicer
12. [ ] **Validar PRINT_START** funciona con adaptive mesh
13. [ ] **Validar PRINT_END** funciona
14. [ ] **Ajustar PA values** según calibración real
15. [ ] **Ajustar retraction** según calibración real
16. [ ] **Test print inicial** (cubo 20mm)

### **Criterio de éxito Phase 4:**
✅ Primera impresión cubo 20mm exitosa
✅ Primera capa perfecta (adhesión, nivelación)
✅ Dimensiones precisas (20mm ±0.1mm)
✅ Sin stringing, sin warping
✅ Temperatura estable durante impresión

### **Deliverables:**
- 📦 `printer.cfg` completamente calibrado (valores finales documentados)
- 📦 Perfiles OrcaSlicer ajustados y validados
- 📦 `CALIBRACION_COMPLETA.md` con valores reales obtenidos
- 📦 Fotos/videos primera impresión exitosa

**Duración estimada:** 6-8 horas
**Estado:** ⏳ PENDIENTE
**Documentación:** [guides/phase3/CALIBRACION_COMPLETA.md](guides/phase3/CALIBRACION_COMPLETA.md)

---

## ⏳ PHASE 5: VALIDAR 4-8-12 (HARDWARE STOCK)

**Objetivo:** Validar configuración stock con impresiones progresivas

### **Metodología 4-8-12:**

#### **Test 1: 4 horas de impresión**
- **Pieza:** Benchy o modelo funcional pequeño (~3-4 hrs)
- **Material:** PLA (perfil Standard 0.2mm)
- **Observar:**
  - [ ] Primera capa se adhiere correctamente
  - [ ] No hay warping en esquinas
  - [ ] Temperatura estable todo el tiempo
  - [ ] Sin layer shifts
  - [ ] Calidad visual general buena
- **Ajustes permitidos:** Tweaks menores (±2% flow, baby stepping)

#### **Test 2: 8 horas de impresión**
- **Pieza:** Funcional compleja o modelo decorativo (~7-8 hrs)
- **Material:** PETG o PLA (perfil Standard/Fine)
- **Observar:**
  - [ ] Estabilidad térmica prolongada (bed + hotend)
  - [ ] Consistencia de calidad capa a capa
  - [ ] Sin fallos de adhesión a mitad de impresión
  - [ ] Overhangs bien soportados
  - [ ] Puentes limpios
- **Ajustes permitidos:** Solo si algo falla crítico

#### **Test 3: 12 horas de impresión**
- **Pieza:** Grande o muy compleja (~10-12 hrs)
- **Material:** El que prefieras (validar confiabilidad)
- **Observar:**
  - [ ] Completa sin intervención
  - [ ] Sin fallos térmicos (thermal runaway)
  - [ ] Sin fallos mecánicos (belt skip, etc.)
  - [ ] Calidad final excelente
  - [ ] Dimensiones precisas post-print
- **Ajustes permitidos:** NINGUNO (debe funcionar tal cual)

### **Criterio de éxito Phase 5:**
✅ Las 3 impresiones (4-8-12 hrs) completadas SIN FALLOS
✅ Calidad consistente en todas
✅ Sin ajustes mayores necesarios entre prints
✅ Sistema robusto y confiable

### **Resultado:**
**🎉 IMPRESORA 100% FUNCIONAL CON NUEVA ELECTRÓNICA + HARDWARE STOCK**

Este es un **MILESTONE CRÍTICO**: la impresora está lista para producción con hardware stock.

**Duración estimada:** ~24-30 horas (tiempo de impresión real)
**Estado:** ⏳ PENDIENTE

---

## 🔮 PHASE 5.5: INSTALACIÓN FLEJE NUEVO PEI (OPCIONAL)

**⚠️ FASE INSERTADA ANTES DE VORON PARA NO DAÑAR FLEJE NUEVO**

**Objetivo:** Instalar superficie de impresión PEI magnética nueva

**Razón de timing:**
- ✅ Hardware stock ya validado completamente (Phase 5)
- ✅ No arriesgar fleje nuevo durante calibraciones iniciales
- ✅ Empezar Voron con superficie nueva (óptima adhesión)

**Hardware:**
- Fleje PEI magnético (doble cara o texturizado)
- Base magnética para cama

### **Tareas:**
1. [ ] Limpiar cama caliente completamente
2. [ ] Instalar base magnética en cama
3. [ ] Colocar fleje PEI
4. [ ] **Re-generar bed mesh** (superficie nueva puede tener diferente planitud)
5. [ ] **Ajustar Z offset** (espesor fleje nuevo)
6. [ ] **Test adhesión** con PLA/PETG/ABS
7. [ ] **Ajustar bed temp** si necesario (PEI puede requerir ±5°C)

### **Criterio de éxito:**
✅ Primera capa se adhiere perfectamente en PEI
✅ Piezas se liberan fácilmente al enfriar
✅ No hay daños en superficie PEI

**Duración estimada:** 2-3 horas
**Estado:** 🔮 FUTURO OPCIONAL (antes de Phase 6)

---

## 🔮 PHASE 6: INSTALAR VORON → TOOLHEAD NUEVO

**Objetivo:** Voron Stealthburner montado y funcionando básicamente

**Hardware nuevo:**
- Voron Stealthburner toolhead (impreso durante Phases 3-5)
- Orbiter 2.5 extruder
- Dragonfly BMO hotend (HF o UHF)
- EBB42 movida DE detrás → DENTRO del toolhead
- Eddy Coil montado en Stealthburner

### **Preparación (durante Phases 3-5):**
- [ ] Imprimir TODAS las piezas Voron (ver `stls/phase12/PRINT_QUEUE_PHASE12.md`)
- [ ] Comprar Orbiter 2.5 (~€80)
- [ ] Comprar Dragonfly BMO (~€80)
- [ ] Verificar tipo de guías lineales (MGN12/MGN9)

### **Tareas - Desmontaje Stock:**
1. [ ] Fotografiar toolhead stock completo (referencia)
2. [ ] Desconectar todos los cables del toolhead
3. [ ] Desmontar toolhead stock completo
4. [ ] Desmontar EBB42 de posición detrás del frame
5. [ ] Guardar piezas stock (reversibilidad)

### **Tareas - Montaje Voron:**
6. [ ] Montar X carriage Voron (según tipo de guías)
7. [ ] Instalar Stealthburner main body
8. [ ] Instalar Orbiter 2.5 en mount
9. [ ] Instalar Dragonfly BMO en cowl/front
10. [ ] Montar EBB42 dentro de Stealthburner
11. [ ] Conectar motor Orbiter a EBB42
12. [ ] Conectar heater Dragonfly a EBB42
13. [ ] Conectar thermistor/PT100 a EBB42
14. [ ] Conectar fans (hotend + part cooling)
15. [ ] Montar Eddy Coil en Stealthburner mount
16. [ ] Conectar Eddy I2C a EBB42
17. [ ] **Cablear todo con cables CORTOS** (EBB42 está EN toolhead ahora)
18. [ ] Routing final de cables por drag chain

### **Testing básico:**
- [ ] `QUERY_PROBE` funciona (Eddy)
- [ ] Heater calienta correctamente
- [ ] Thermistor lee temperatura
- [ ] Fans funcionan
- [ ] **G28** completo funciona

### **Criterio de éxito Phase 6:**
✅ Voron Stealthburner montado completamente
✅ EBB42 integrada en toolhead (no detrás)
✅ G28 funciona con nuevo toolhead
✅ Heater + thermistor + fans operativos
✅ Eddy Coil detecta correctamente

**Duración estimada:** 6-8 horas
**Estado:** 🔮 FUTURO
**Documentación:** A crear en `guides/phase6/`

---

## 🔮 PHASE 7: CALIBRAR VORON → PERFILES ORBITER

**Objetivo:** Voron completamente calibrado con perfiles funcionales

**Hardware:** Orbiter 2.5 + Dragonfly BMO

### **⚠️ IMPORTANTE: Muchos valores CAMBIAN vs stock**

| Parámetro | Stock (Titan) | Voron (Orbiter 2.5) | Cambio |
|-----------|---------------|---------------------|--------|
| rotation_distance | ~22.478 | ~4.637 | Completamente diferente |
| gear_ratio | 66:22 (3:1) | 7.5:1 | Diferente |
| Pressure advance | 0.03-0.05 | 0.03-0.06 | Similar pero re-tune |
| Retraction | 1.0-2.0mm | 0.5-1.5mm | Menor (direct drive mejor) |
| Retraction speed | 40mm/s | 30-50mm/s | Ajustar |
| Max velocity | 300mm/s | 350-400mm/s | Mayor (toolhead más ligero) |
| Max accel | 3000mm/s² | 5000-8000mm/s² | Mucho mayor |

### **Tareas - Calibración Klipper:**

**Térmica:**
1. [ ] **PID tuning Dragonfly BMO**
   - Nuevo hotend = nuevos valores PID
   - Target según material (ej. 210°C PLA)
2. [ ] **Verificar PID bed** (probablemente OK, pero check)

**Mecánica:**
3. [ ] **E-steps Orbiter 2.5**
   - rotation_distance inicial: 4.637
   - gear_ratio: 7.5:1
   - Calibrar con test 100mm
4. [ ] **Z offset con nuevo toolhead**
   - Altura toolhead cambió
   - Re-calibrar con PROBE_EDDY_CURRENT_CALIBRATE
5. [ ] **Bed mesh** (verificar si cambió)
   - Probablemente similar, pero regenerar

**Calidad:**
6. [ ] **Pressure advance Orbiter**
   - Rango típico: 0.03-0.06
   - Ellis' pattern generator
7. [ ] **Retraction Orbiter**
   - Rango típico: 0.5-1.5mm @ 30-50mm/s
   - Mucho menor que Titan
8. [ ] **Flow rate** (Dragonfly BMO)
   - Starting point: 100%
   - Dragonfly puede tener mejor flow

**Avanzado:**
9. [ ] **Input Shaper** (toolhead weight cambió)
   - TEST_RESONANCES X/Y
   - Frecuencias serán DIFERENTES
   - Toolhead Voron más ligero = mayor accel posible
10. [ ] **Velocidades optimizadas**
    - Test max_velocity: 350-400mm/s
    - Test max_accel: 5000-8000mm/s²
    - Voron permite mucho más que stock

### **Tareas - Nuevos Perfiles OrcaSlicer:**

11. [ ] **Crear perfiles filament Orbiter** (copiar de stock, ajustar PA y retraction)
12. [ ] **Crear perfiles process Voron** (mayores velocidades)
13. [ ] **Ajustar aceleraciones** en perfiles
14. [ ] **Test print inicial** (cubo 20mm con Orbiter)

### **Criterio de éxito Phase 7:**
✅ Primera impresión Orbiter exitosa
✅ Calidad igual o MEJOR que stock
✅ Aprovecha velocidades mayores (>300mm/s)
✅ Retracciones menores pero limpias
✅ Sin stringing ni oozing

### **Deliverables:**
- 📦 `printer.cfg` actualizado para Orbiter 2.5
- 📦 Perfiles OrcaSlicer Voron (PLA/PETG/ABS)
- 📦 Documentación valores Orbiter vs Titan
- 📦 Fotos/videos primera impresión Voron

**Duración estimada:** 6-8 horas
**Estado:** 🔮 FUTURO

---

## 🔮 PHASE 8: VALIDAR 4-8-12 (VORON)

**Objetivo:** Validar Voron con impresiones progresivas

### **Metodología 4-8-12 (Voron):**

#### **Test 1: 4 horas**
- Material: PLA Standard
- **Aprovechar velocidades Voron** (print time puede reducirse 20-30%)
- Observar calidad a alta velocidad

#### **Test 2: 8 horas**
- Material: PETG o ABS
- Validar thermal performance Dragonfly BMO
- Verificar retracciones Orbiter

#### **Test 3: 12 horas**
- Material: Lo que prefieras
- Confiabilidad sistema completo Voron
- Calidad superior a stock

### **Criterio de éxito Phase 8:**
✅ 3 impresiones (4-8-12) completadas sin fallos
✅ Calidad SUPERIOR a stock (o al menos igual)
✅ Velocidades mayores aprovechadas
✅ Toolhead Voron confiable

### **Resultado:**
**🎉 VORON STEALTHBURNER 100% VALIDADO**

**Duración estimada:** ~24-30 horas (tiempo de impresión)
**Estado:** 🔮 FUTURO

---

## 🔮 PHASE 9: INSTALAR MULTICOLOR → MMU/AMS

**Objetivo:** Sistema multicolor montado y funcionando

**Opciones de hardware:**
- ERCF (Enraged Rabbit Carrot Feeder)
- AMS (Automatic Material System - Bambu Lab)
- MMU2S (Prusa)
- Tradrack
- Otro sistema custom

### **Tareas (depende de sistema elegido):**
1. [ ] **Investigar sistemas multicolor** (ERCF recomendado para Voron)
2. [ ] **Comprar hardware** MMU
3. [ ] **Imprimir piezas** MMU (durante Phase 6-8)
4. [ ] **Montar sistema multicolor** físicamente
5. [ ] **Instalar filament buffers** (4-8 filamentos)
6. [ ] **Instalar sensores filamento** (runout detection)
7. [ ] **Cablear todo a Raspberry Pi** o controladora
8. [ ] **Configurar Klipper** para multicolor
9. [ ] **Purge bucket/tower** setup
10. [ ] **Test cambios de tool** funcionales

### **Criterio de éxito Phase 9:**
✅ Sistema detecta 4+ filamentos correctamente
✅ Cambios de tool funcionan (load/unload)
✅ Purga/limpieza funciona
✅ Sensores detectan runout

**Duración estimada:** 8-12 horas
**Estado:** 🔮 FUTURO

---

## 🔮 PHASE 10: CALIBRAR MULTICOLOR → PERFILES

**Objetivo:** Multicolor calibrado con perfiles funcionales

### **Tareas - Calibración:**
1. [ ] **Calibrar cambios de tool** (velocidad, retraction)
2. [ ] **Ajustar purge volumes** por combinación de colores
3. [ ] **Torre de purga** optimizada (altura, infill)
4. [ ] **Tuning retraction multi-tool**
5. [ ] **Ramming settings** (si usa ERCF)
6. [ ] **Gestión de colores** en OrcaSlicer
7. [ ] **Wipe sequences** (limpieza nozzle entre cambios)

### **Tareas - Perfiles OrcaSlicer:**
8. [ ] **Crear perfiles multicolor**
9. [ ] **Configurar tool change scripts**
10. [ ] **Ajustar purge/wipe settings**

### **Criterio de éxito Phase 10:**
✅ Primera impresión 2 colores exitosa
✅ Cambios de tool limpios (sin contaminación)
✅ Torre de purga eficiente
✅ Sin oozing entre cambios

**Duración estimada:** 6-8 horas
**Estado:** 🔮 FUTURO

---

## 🔮 PHASE 11: VALIDAR 4-8-12 (MULTICOLOR)

**Objetivo:** Validar multicolor con impresiones progresivas

### **Metodología 4-8-12 (Multicolor):**

#### **Test 1: 4 horas (2-3 colores)**
- Modelo simple multicolor
- Validar cambios de tool básicos

#### **Test 2: 8 horas (3-4 colores)**
- Modelo complejo multicolor
- Validar purge volumes correctos

#### **Test 3: 12 horas (4+ colores)**
- Modelo muy complejo o grande multicolor
- Confiabilidad sistema completo

### **Criterio de éxito Phase 11:**
✅ 3 impresiones multicolor completadas sin fallos
✅ Colores limpios (sin bleeding)
✅ Cambios de tool confiables
✅ Torre de purga eficiente

### **Resultado:**
**🎉 SISTEMA MULTICOLOR 100% FUNCIONAL**

**Duración estimada:** ~24-30 horas
**Estado:** 🔮 FUTURO

---

## 🔮 PHASE 12: OPTIMIZACIÓN FINAL → PROYECTO COMPLETO

**Objetivo:** Sistema completamente optimizado y documentado

### **Tareas:**
1. [ ] Fine-tuning multicolor (ajustes finales)
2. [ ] Backup completo configuración
   - printer.cfg versionado
   - Perfiles OrcaSlicer exportados
   - Macros documentadas
3. [ ] Documentación completa
   - Guías de troubleshooting
   - Valores de referencia
   - Procedimientos mantenimiento
4. [ ] Test prints showcase
   - Portafolio de impresiones exitosas
   - Demostrar capacidades
5. [ ] Optimizaciones finales
   - Tweaks menores
   - Macros avanzadas custom

### **Resultado:**
**🏆 PROYECTO COMPLETO - TRONXY X5SA TOTALMENTE MODERNIZADA**

**Duración:** Variable
**Estado:** 🔮 FUTURO

---

## 📊 TABLA RESUMEN FASES

| Phase | Nombre | Tipo | Hardware | Duración | Estado |
|-------|--------|------|----------|----------|--------|
| 0 | Baseline | - | Stock | - | ✅ |
| 1 | SKR prep | - | SKR + TMC2209 | 2h | ✅ |
| 2 | SKR wiring | - | SKR cableada | 6h | ✅ |
| **3** | **Eddy install** | **INSTALAR** | **+ Eddy Coil** | **4-6h** | **📋** |
| **4** | **Stock calib** | **CALIBRAR** | **Stock** | **6-8h** | **⏳** |
| **5** | **Validar 4-8-12** | **VALIDAR** | **Stock** | **24-30h** | **⏳** |
| 5.5 | Fleje PEI | Opcional | + PEI sheet | 2-3h | 🔮 |
| **6** | **Voron install** | **INSTALAR** | **+ Voron** | **6-8h** | **🔮** |
| **7** | **Voron calib** | **CALIBRAR** | **Voron** | **6-8h** | **🔮** |
| **8** | **Validar 4-8-12** | **VALIDAR** | **Voron** | **24-30h** | **🔮** |
| **9** | **MMU install** | **INSTALAR** | **+ MMU** | **8-12h** | **🔮** |
| **10** | **MMU calib** | **CALIBRAR** | **MMU** | **6-8h** | **🔮** |
| **11** | **Validar 4-8-12** | **VALIDAR** | **MMU** | **24-30h** | **🔮** |
| 12 | Optimización | Final | Todo | Variable | 🔮 |

**Tiempo total estimado:** 120-150 horas (incluyendo impresiones)

---

## 🎯 MILESTONES CRÍTICOS

### **Milestone 1: Hardware Stock Funcional** (fin Phase 5)
✅ Impresora con nueva electrónica 100% validada
✅ Hardware stock confiable
✅ Listo para producción

### **Milestone 2: Hardware Voron Funcional** (fin Phase 8)
✅ Voron Stealthburner 100% validado
✅ Velocidades superiores
✅ Calidad profesional

### **Milestone 3: Multicolor Funcional** (fin Phase 11)
✅ Sistema multicolor completo
✅ 4+ colores simultáneos
✅ Producción multicolor

### **Milestone 4: Proyecto Completo** (fin Phase 12)
🏆 Tronxy X5SA completamente modernizada
🏆 Capacidades nivel Voron + multicolor
🏆 Documentación completa y replicable

---

## 🛠️ HERRAMIENTAS Y MATERIALES

(Esta sección permanece sin cambios desde v2.0 - ver versiones anteriores en historial Git si necesitas el detalle completo)

---

## 📚 DOCUMENTACIÓN POR FASE

### **Phase 3:**
- [guides/phase3/LOG_INSTALACION.md](guides/phase3/LOG_INSTALACION.md)
- [guides/phase3/GUIA_FOTOGRAFICA_INSTALACION.md](guides/phase3/GUIA_FOTOGRAFICA_INSTALACION.md)
- [guides/phase3/EDDY_COIL_INSTALLATION.md](guides/phase3/EDDY_COIL_INSTALLATION.md)
- [guides/phase3/ARQUITECTURA_PHASE3.md](guides/phase3/ARQUITECTURA_PHASE3.md)

### **Phase 4:**
- [guides/phase3/CALIBRACION_COMPLETA.md](guides/phase3/CALIBRACION_COMPLETA.md)
- [guides/phase3/CONFIGURACION_KLIPPER.md](guides/phase3/CONFIGURACION_KLIPPER.md)
- [orca-slicer-profiles/](orca-slicer-profiles/)

### **Phase 5:**
- A crear: `guides/phase5/VALIDACION_4_8_12.md`

### **Phase 6:**
- [stls/phase12/PRINT_QUEUE_PHASE12.md](stls/phase12/PRINT_QUEUE_PHASE12.md)
- A crear: `guides/phase6/VORON_INSTALLATION.md`

### **Phases 7-12:**
- A crear según progreso

---

## 🔄 REVERSIBILIDAD

| Fase | ¿Reversible? | Cómo |
|------|--------------|------|
| 0-2 | ⚠️ Difícil | Reinstalar electrónica stock |
| 3-5 | ⚠️ Difícil | Mantener piezas stock guardadas |
| 6-8 | ✅ Sí | Reinstalar toolhead stock (si guardado) |
| 9-11 | ✅ Sí | Desmontar MMU, volver a Voron single-color |
| 12 | ✅ Sí | Sistema modular |

---

## 📝 FILOSOFÍA DEL ROADMAP v3.0

1. **INSTALAR → CALIBRAR → VALIDAR (4-8-12)**
2. **Cada grupo termina con validación real de producción**
3. **No avanzar hasta validar completamente fase anterior**
4. **Documentar TODO en tiempo real**
5. **Commits frecuentes, rollback siempre posible**
6. **Hardware guardado (reversibilidad)**
7. **Sin prisa, sin errores**

---

**Última actualización:** 2025-12-27
**Versión:** 3.0
**Cambios v3.0:**
- Reorganizado con metodología 4-8-12
- 12 phases claramente definidas
- Grupos lógicos: Stock → Voron → Multicolor
- Validaciones con impresiones reales
- Fleje PEI en Phase 5.5 (antes de Voron)
- Criterios de éxito específicos por fase
- Milestones críticos destacados

**Próxima revisión:** Después de completar Phase 3
