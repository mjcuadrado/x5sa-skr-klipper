# Log de Instalación y Calibración Phase 3 - Eddy Coil
**Fecha inicio:** 2025-12-27
**Hardware:** Tronxy X5SA + SKR 1.4 Turbo + EBB42 V1.2 + Eddy Coil V1.0
**Objetivo:** Instalación física y calibración completa del sistema

---

## 📋 Estado General

| Fase | Tarea | Estado | Tiempo | Notas |
|------|-------|--------|--------|-------|
| **1. INSTALACIÓN** | Montaje físico Eddy Coil | ⏳ PENDIENTE | - | 2-3mm sobre nozzle |
| **1. INSTALACIÓN** | Verificar I2C connection | ⏳ PENDIENTE | - | QUERY_PROBE test |
| **2. THERMAL CAL** | PID tuning hotend | ⏳ PENDIENTE | ~15 min | TARGET=210°C |
| **2. THERMAL CAL** | PID tuning bed | ⏳ PENDIENTE | ~20 min | TARGET=60°C |
| **3. MECHANICAL** | E-steps calibration | ⏳ PENDIENTE | ~10 min | rotation_distance |
| **3. MECHANICAL** | Eddy drive current | ⏳ PENDIENTE | ~5 min | LDC_CALIBRATE |
| **3. MECHANICAL** | Z offset calibration | ⏳ PENDIENTE | ~15 min | PROBE_EDDY_CURRENT |
| **3. MECHANICAL** | Z-Tilt adjustment | ⏳ PENDIENTE | ~5 min | Dual Z leveling |
| **3. MECHANICAL** | Bed mesh generation | ⏳ PENDIENTE | ~15 seg | rapid_scan 5×5 |
| **4. QUALITY** | Pressure advance | ⏳ PENDIENTE | ~30 min | Ellis' pattern |
| **4. QUALITY** | Retraction tuning | ⏳ PENDIENTE | ~20 min | Si hay stringing |
| **5. FIRST PRINT** | OrcaSlicer setup | ⏳ PENDIENTE | ~10 min | Import profiles |
| **5. FIRST PRINT** | Test print (cubo 20mm) | ⏳ PENDIENTE | ~30 min | PLA Standard |

**Tiempo estimado total:** 2.5 - 4 horas

---

## 🔧 FASE 1: INSTALACIÓN FÍSICA

### 1.1 Montaje Eddy Coil en Toolhead

**Referencias:**
- [EDDY_COIL_INSTALLATION.md](EDDY_COIL_INSTALLATION.md) - Guía completa instalación
- printer.cfg:298-329 - Configuración Eddy Coil

**Checklist pre-instalación:**
- [ ] Impresora apagada (PSU OFF)
- [ ] Eddy Coil disponible (modelo BTT V1.0)
- [ ] Soporte/bracket para montar (impreso o metal)
- [ ] Tornillos M3 disponibles
- [ ] Herramientas: destornilladores, calibre/regla

**Procedimiento:**

**Paso 1.1.1: Montar Eddy Coil en toolhead**
```
Fecha/Hora: _________________
Acción: Montar Eddy Coil con bracket
Altura sobre nozzle: _______ mm (objetivo: 2-3mm)

✅ COMPLETADO / ❌ PROBLEMA
Notas:
_________________________________________
_________________________________________
```

**Paso 1.1.2: Conectar cables I2C a EBB42**
```
Fecha/Hora: _________________
Conexiones verificadas:
- [ ] VCC (rojo) → EBB42 VCC (verificar voltaje)
- [ ] GND (negro) → EBB42 GND
- [ ] SCL → EBB42 PB3 (pin configurado en printer.cfg:308)
- [ ] SDA → EBB42 PB4 (pin configurado en printer.cfg:309)

⚠️ ADVERTENCIA GROUND LOOP:
- Todo alimentado desde EBB42 (VCC + GND + I2C)
- NUNCA mezclar grounds entre SKR y EBB42

✅ COMPLETADO / ❌ PROBLEMA
Notas:
_________________________________________
_________________________________________
```

**Paso 1.1.3: Verificar altura física con calibre**
```
Fecha/Hora: _________________
Medida exacta nozzle → Eddy Coil: _______ mm
¿Está en rango 2-3mm? [ ] SÍ / [ ] NO

Si NO, ajustar bracket y re-medir.

✅ COMPLETADO / ❌ PROBLEMA
Notas:
_________________________________________
_________________________________________
```

---

### 1.2 Verificación Eléctrica (Pre-Power-On)

**Paso 1.2.1: Inspección visual cables**
```
Fecha/Hora: _________________
- [ ] No hay cables pelados o expuestos
- [ ] Conectores bien insertados en EBB42
- [ ] No hay tensión mecánica en cables
- [ ] Cables alejados de partes móviles y calientes

✅ COMPLETADO / ❌ PROBLEMA
Notas:
_________________________________________
_________________________________________
```

**Paso 1.2.2: Multímetro check (OPCIONAL pero RECOMENDADO)**
```
Fecha/Hora: _________________
Con PSU OFF, medir continuidad:
- VCC → GND: [ ] ∞ Ω (sin cortocircuito)
- SCL → GND: [ ] ∞ Ω (sin cortocircuito)
- SDA → GND: [ ] ∞ Ω (sin cortocircuito)

Si hay cortocircuito (0Ω), NO encender. Revisar cables.

✅ COMPLETADO / ❌ PROBLEMA / ⏭️ OMITIDO
Notas:
_________________________________________
_________________________________________
```

---

### 1.3 First Power-On y Test Conexión I2C

**Paso 1.3.1: Encender sistema**
```
Fecha/Hora: _________________
Acción: PSU ON → esperar boot completo

Klipper status: [ ] Ready / [ ] Error / [ ] Shutdown

Si Error o Shutdown, revisar logs:
- Mainsail/Fluidd: consultar tab "Console"
- SSH: cat /tmp/klippy.log

✅ COMPLETADO / ❌ PROBLEMA
Notas:
_________________________________________
_________________________________________
```

**Paso 1.3.2: Test básico QUERY_PROBE**
```
Fecha/Hora: _________________
Comando ejecutado: QUERY_PROBE

Output esperado:
probe: open

Si muestra "triggered" en el aire, revisar wiring.
Si error I2C, verificar conexiones PB3/PB4.

Output real:
_________________________________________

✅ COMPLETADO / ❌ PROBLEMA
Notas:
_________________________________________
_________________________________________
```

**Paso 1.3.3: Test detección metal**
```
Fecha/Hora: _________________
Acción: Acercar metal (destornillador, cama) al Eddy Coil

Comando ejecutado: QUERY_PROBE
Output esperado: probe: TRIGGERED

Output real:
_________________________________________

¿El sensor detecta metal correctamente? [ ] SÍ / [ ] NO

✅ COMPLETADO / ❌ PROBLEMA
Notas:
_________________________________________
_________________________________________
```

**Paso 1.3.4: Safety check completo con macro**
```
Fecha/Hora: _________________
Comando ejecutado: VERIFY_EDDY_PROBE

Output esperado (5 pasos):
1. "probe: open" (en el aire)
2. "Place metal object near coil now!" (espera 3 seg)
3. "probe: TRIGGERED" (con metal)
4. "Remove metal object" (espera 2 seg)
5. "probe: open" (sin metal)

Output real:
_________________________________________
_________________________________________
_________________________________________

✅ COMPLETADO / ❌ PROBLEMA
Notas:
_________________________________________
_________________________________________
```

---

## 📊 Resumen Fase 1 - Instalación Física

```
Fecha completada: _________________
Tiempo total: _______ minutos

Estado: [ ] ✅ ÉXITO / [ ] ❌ FALLÓ

Problemas encontrados:
_________________________________________
_________________________________________

Soluciones aplicadas:
_________________________________________
_________________________________________

¿Listo para continuar con calibraciones? [ ] SÍ / [ ] NO
```

**⚠️ CHECKPOINT:** Si hay problemas en Fase 1, NO continuar. Resolver primero.

---

## 🌡️ FASE 2: CALIBRACIONES TÉRMICAS

**Referencias:**
- [CALIBRACION_COMPLETA.md](CALIBRACION_COMPLETA.md#calibración-pid---hotend)
- printer.cfg:624-638 - Macros PID

**Tiempo estimado:** 35-45 minutos total

---

### 2.1 PID Tuning - Hotend

**Paso 2.1.1: Preparación**
```
Fecha/Hora: _________________
- [ ] Extrusor sin filamento cargado
- [ ] Part cooling fan limpio
- [ ] Área ventilada (sin corrientes de aire)
- [ ] Nozzle limpio (sin plástico residual)

✅ PREPARADO
```

**Paso 2.1.2: Ejecutar PID tuning**
```
Fecha/Hora inicio: _________________
Comando ejecutado: PID_TUNE_HOTEND TARGET=210

Temperatura objetivo: 210°C (PLA estándar)

⏱️ Tiempo de espera: ~15 minutos
   Observar: temperatura debe oscilar y estabilizarse

Output relevante (copiar valores finales):
_________________________________________
_________________________________________
_________________________________________

Valores PID obtenidos:
- Kp: _____________
- Ki: _____________
- Kd: _____________

Fecha/Hora fin: _________________
Tiempo total: _______ minutos

✅ COMPLETADO / ❌ PROBLEMA
Notas:
_________________________________________
```

**Paso 2.1.3: Verificar estabilidad**
```
Fecha/Hora: _________________
Comando: M104 S210
         M109 S210
         (esperar estabilización)

Observar gráfico temperatura en Mainsail/Fluidd:
- Oscilación máxima: _______ °C (esperado: <2°C)
- Tiempo para alcanzar 210°C: _______ segundos

¿Temperatura estable? [ ] SÍ / [ ] NO

Si oscila >3°C, considerar re-tuning.

Comando: M104 S0 (apagar hotend)

✅ VERIFICADO / ❌ PROBLEMA
Notas:
_________________________________________
```

---

### 2.2 PID Tuning - Heated Bed

**Paso 2.2.1: Preparación**
```
Fecha/Hora: _________________
- [ ] Cama limpia (sin objetos encima)
- [ ] Termistor bien sujeto a cama
- [ ] Área sin corrientes de aire

⏱️ Advertencia: Este proceso toma ~20-25 minutos

✅ PREPARADO
```

**Paso 2.2.2: Ejecutar PID tuning**
```
Fecha/Hora inicio: _________________
Comando ejecutado: PID_TUNE_BED TARGET=60

Temperatura objetivo: 60°C (PLA estándar)

⏱️ Tiempo de espera: ~20-25 minutos
   La cama tiene mucha masa térmica, es normal que tarde

Output relevante (copiar valores finales):
_________________________________________
_________________________________________
_________________________________________

Valores PID obtenidos:
- Kp: _____________
- Ki: _____________
- Kd: _____________

Fecha/Hora fin: _________________
Tiempo total: _______ minutos

✅ COMPLETADO / ❌ PROBLEMA
Notas:
_________________________________________
```

**Paso 2.2.3: Verificar estabilidad**
```
Fecha/Hora: _________________
Comando: M140 S60
         M190 S60
         (esperar estabilización)

Observar gráfico temperatura:
- Oscilación máxima: _______ °C (esperado: <1°C)
- Tiempo para alcanzar 60°C: _______ minutos

¿Temperatura estable? [ ] SÍ / [ ] NO

Comando: M140 S0 (apagar cama)

✅ VERIFICADO / ❌ PROBLEMA
Notas:
_________________________________________
```

---

## 📊 Resumen Fase 2 - Calibraciones Térmicas

```
Fecha completada: _________________
Tiempo total: _______ minutos

Estado: [ ] ✅ ÉXITO / [ ] ❌ FALLÓ

PID Hotend: Kp=_____ Ki=_____ Kd=_____
PID Bed: Kp=_____ Ki=_____ Kd=_____

Problemas encontrados:
_________________________________________

¿Listo para calibraciones mecánicas? [ ] SÍ / [ ] NO
```

---

## ⚙️ FASE 3: CALIBRACIONES MECÁNICAS

**Referencias:**
- [CALIBRACION_COMPLETA.md](CALIBRACION_COMPLETA.md)
- [EDDY_COIL_CALIBRATION.md](EDDY_COIL_CALIBRATION.md)

**Tiempo estimado:** 45-90 minutos

---

### 3.1 Calibración E-steps (Rotation Distance)

**Paso 3.1.1: Preparación**
```
Fecha/Hora: _________________
Materiales:
- [ ] Filamento PLA cargado
- [ ] Marcador permanente
- [ ] Regla o calibre (precisión 0.5mm)
- [ ] Papel para anotar medidas

Valor actual rotation_distance: 22.478 mm (printer.cfg:174)
(Titan clone, gear_ratio 66:22)

✅ PREPARADO
```

**Paso 3.1.2: Marcar filamento**
```
Fecha/Hora: _________________
Acción: Marcar filamento a 120mm desde entrada extrusor

Marca realizada: [ ] SÍ
Distancia verificada con regla: _______ mm (debe ser ~120mm)

✅ MARCADO
```

**Paso 3.1.3: Calentar hotend y extruir**
```
Fecha/Hora: _________________
Comandos:
M104 S200        # Calentar a 200°C
M109 S200        # Esperar temperatura
G91              # Modo relativo
G1 E100 F60      # Extruir 100mm a 60mm/min (1mm/s)
G90              # Modo absoluto

Tiempo extrusión: ~1min 40seg

⚠️ Observar que extrusor NO patine ni haga grinding

Extrusión completada: [ ] SÍ / [ ] Grinding detectado

✅ COMPLETADO / ❌ PROBLEMA
Notas:
_________________________________________
```

**Paso 3.1.4: Medir y calcular**
```
Fecha/Hora: _________________

Distancia RESTANTE desde marca hasta extrusor: _______ mm

Cálculo:
Extruido real = 120mm - distancia_restante
Extruido real = 120 - _______ = _______ mm

Fórmula nuevo rotation_distance:
nuevo_RD = actual_RD × (100 / extruido_real)
nuevo_RD = 22.478 × (100 / _______) = _______ mm

¿Diferencia >1%? [ ] SÍ → actualizar / [ ] NO → dejar actual

Si SÍ, actualizar printer.cfg línea 174

✅ CALCULADO
Notas:
_________________________________________
```

**Paso 3.1.5: Aplicar nuevo valor (si necesario)**
```
Fecha/Hora: _________________

Valor anterior: 22.478 mm
Valor nuevo: _______ mm

Actualizado en printer.cfg: [ ] SÍ / [ ] NO necesario

Si actualizado:
Comando: RESTART (reiniciar Klipper)

✅ APLICADO / ⏭️ OMITIDO
```

---

### 3.2 Calibración Eddy Coil - Drive Current

**Paso 3.2.1: Información**
```
Esta calibración se hace UNA SOLA VEZ (a menos que cambies sensor)
Optimiza sensibilidad del LDC1612 para tu instalación específica

Tiempo estimado: ~5 minutos
```

**Paso 3.2.2: Ejecutar calibración automática**
```
Fecha/Hora: _________________
Comando: LDC_CALIBRATE_DRIVE_CURRENT CHIP=btt_eddy

Output esperado:
- Serie de mediciones
- "Recommended drive_current: XX"
- SAVE_CONFIG prompt

Output real:
_________________________________________
_________________________________________

Drive current recomendado: _______

Comando: SAVE_CONFIG (guardar y reiniciar)

✅ COMPLETADO / ❌ PROBLEMA
Notas:
_________________________________________
```

**Paso 3.2.3: Verificar valor guardado**
```
Fecha/Hora: _________________

Verificar printer.cfg línea ~328:
reg_drive_current: _______ (debe estar descomentado)

✅ VERIFICADO
```

---

### 3.3 Calibración Z Offset (Eddy Coil)

**Paso 3.3.1: Preparación CRÍTICA**
```
Fecha/Hora: _________________

⚠️ CHECKLIST DE SEGURIDAD PRE-CALIBRACIÓN:

- [ ] ✅ QUERY_PROBE muestra "open" en el aire
- [ ] ✅ QUERY_PROBE muestra "TRIGGERED" con metal cerca
- [ ] ✅ Nozzle completamente limpio (sin plástico)
- [ ] ✅ Cama limpia (sin objetos, sin adhesivos gruesos)
- [ ] ✅ Papel bond disponible para paper test
- [ ] ✅ Mano en emergency stop
- [ ] ✅ Iluminación adecuada para ver nozzle

✅ PREPARADO / ❌ FALTA algo → NO continuar
```

**Paso 3.3.2: Método automático con macro**
```
Fecha/Hora inicio: _________________

Comando: CALIBRATE_EDDY_CURRENT

Este macro ejecuta:
1. LDC_CALIBRATE_DRIVE_CURRENT (ya hecho, pero verifica)
2. G28 (homing - OBSERVAR que pare correctamente)
3. PROBE_EDDY_CURRENT_CALIBRATE CHIP=btt_eddy

⚠️ DURANTE G28: Mano en emergency stop, observar descenso Z

G28 ejecutado correctamente: [ ] SÍ / [ ] ABORTADO → revisar

✅ INICIADO / ❌ PROBLEMA
```

**Paso 3.3.3: Ajuste Z offset con paper test**
```
Fecha/Hora: _________________

Klipper mostrará prompt interactivo:
"Adjust Z height with TESTZ commands"

Colocar papel bond bajo nozzle

Comandos disponibles:
- TESTZ Z=+0.05  (subir nozzle 0.05mm)
- TESTZ Z=-0.05  (bajar nozzle 0.05mm)
- TESTZ Z=+0.01  (ajuste fino arriba)
- TESTZ Z=-0.01  (ajuste fino abajo)

Objetivo: Fricción leve en papel (puede moverse con resistencia)

Comandos ejecutados (anotar secuencia):
_________________________________________
_________________________________________
_________________________________________

Z offset final: _______ mm

Comando final: ACCEPT (aceptar valor)
Comando: SAVE_CONFIG (guardar y reiniciar)

✅ COMPLETADO / ❌ PROBLEMA
Notas:
_________________________________________
```

**Paso 3.3.4: Verificar valor guardado**
```
Fecha/Hora: _________________

Verificar printer.cfg línea 315:
z_offset: _______ mm

Valor razonable: 0.5 - 3.0mm
Si está fuera de rango, revisar instalación física

✅ VERIFICADO / ⚠️ VALOR EXTRAÑO
Notas:
_________________________________________
```

---

### 3.4 Z-Tilt Adjustment (Nivelar Dual Z)

**Paso 3.4.1: Información**
```
Tu impresora tiene 2 motores Z independientes
Z_TILT_ADJUST alinea ambos para que bed esté perpendicular a X/Y

Se debe ejecutar:
- Después de cambiar Z offset
- Antes de generar bed mesh
- Periódicamente (cada 1-2 semanas)
```

**Paso 3.4.2: Preparación**
```
Fecha/Hora: _________________

- [ ] Printer homeado (G28)
- [ ] Cama a temperatura ambiente (NO caliente aún)

Comando: G28 (home all axes)

✅ PREPARADO
```

**Paso 3.4.3: Ejecutar Z-Tilt**
```
Fecha/Hora: _________________
Comando: Z_TILT_ADJUST

Observar:
- Nozzle se mueve a 2 puntos de probing (izq/der)
- Ajuste automático de motores Z
- Reintentos si diferencia es alta

Output esperado (última línea):
"z_tilt: calibrated"

Output real:
_________________________________________
_________________________________________

Número de iteraciones: _______ (esperado: 1-3)
Desviación final: _______ mm (esperado: <0.02mm)

✅ COMPLETADO / ❌ NO CONVERGE
Notas:
_________________________________________
```

**Paso 3.4.4: Troubleshooting si no converge**
```
Si después de 5 intentos NO converge:

Posibles causas:
- [ ] Z offset muy bajo (nozzle choca con cama)
- [ ] Probe no detecta correctamente
- [ ] Tornillos Z demasiado apretados/flojos
- [ ] Cama mecánicamente mal nivelada

Acción tomada:
_________________________________________
_________________________________________

✅ RESUELTO / ❌ REQUIERE REVISIÓN MECÁNICA
```

---

### 3.5 Generar Bed Mesh con Rapid Scan

**Paso 3.5.1: Preparación**
```
Fecha/Hora: _________________

⚠️ IMPORTANTE: Cama DEBE estar a temperatura de impresión
   La cama se expande al calentarse, el mesh será inválido si está fría

- [ ] Z-Tilt ejecutado y convergido
- [ ] Cama calentando a 60°C

Comandos:
M140 S60         # Set bed temp
M190 S60         # Wait for bed temp

✅ PREPARADO (cama a 60°C)
```

**Paso 3.5.2: Generar mesh con rapid_scan**
```
Fecha/Hora inicio: _________________

Comando: GENERATE_BED_MESH

Este macro ejecuta:
1. G28 (home)
2. Z_TILT_ADJUST (verificar nivelación)
3. BED_MESH_CALIBRATE METHOD=rapid_scan (5×5 en ~15 segundos)
4. SAVE_CONFIG (guardar mesh)

⏱️ Tiempo esperado: ~1-2 minutos total
   (La mayoría es homing/z-tilt, mesh solo ~15 seg)

Observar movimiento rápido durante mesh generation

Fecha/Hora fin: _________________
Tiempo real: _______ segundos

✅ COMPLETADO / ❌ PROBLEMA
```

**Paso 3.5.3: Analizar resultados del mesh**
```
Fecha/Hora: _________________

Acceder a Mainsail/Fluidd → Heightmap / Bed Mesh

Visualización 3D del mesh: [ ] Visible

Valores clave:
- Punto más bajo (min): _______ mm
- Punto más alto (max): _______ mm
- Rango total (max - min): _______ mm

Evaluación:
- [ ] Excelente: <0.2mm → cama muy plana
- [ ] Bueno: 0.2-0.5mm → aceptable, common
- [ ] Regular: 0.5-1.0mm → funcional, considerar tramming
- [ ] Malo: >1.0mm → requiere tramming mecánico

Estado: _______________________

Patrón visible (cóncavo/convexo/irregular): ______________

✅ ANALIZADO
Notas:
_________________________________________
```

**Paso 3.5.4: Guardar screenshot mesh (OPCIONAL)**
```
Captura de pantalla del heightmap guardada: [ ] SÍ / [ ] NO

Si SÍ, ubicación: _________________________

✅ DOCUMENTADO / ⏭️ OMITIDO
```

---

## 📊 Resumen Fase 3 - Calibraciones Mecánicas

```
Fecha completada: _________________
Tiempo total: _______ minutos

Estado: [ ] ✅ ÉXITO / [ ] ❌ FALLÓ

Valores finales:
- Rotation distance: _______ mm
- Eddy drive current: _______
- Z offset: _______ mm
- Z-Tilt convergió: [ ] SÍ en _____ iteraciones
- Bed mesh rango: _______ mm (_______ calidad)

Problemas encontrados:
_________________________________________

¿Listo para calibraciones de calidad? [ ] SÍ / [ ] NO
```

---

## 🎨 FASE 4: CALIBRACIONES DE CALIDAD

**Tiempo estimado:** 50-90 minutos
**Estado:** ⏳ Pendiente implementar

### 4.1 Calibración Pressure Advance

**Paso 4.1.1: Generar patrón de calibración**
```
(A completar durante ejecución)
```

### 4.2 Calibración Retraction

**Paso 4.2.1: Test retraction tower**
```
(A completar durante ejecución)
```

---

## 🖨️ FASE 5: PRIMERA IMPRESIÓN

**Tiempo estimado:** 40-60 minutos
**Estado:** ⏳ Pendiente implementar

### 5.1 Importar Perfiles OrcaSlicer

**Paso 5.1.1: Import profiles**
```
(A completar durante ejecución)
```

### 5.2 Test Print - Cubo 20mm

**Paso 5.2.1: Slice and print**
```
(A completar durante ejecución)
```

---

## 📊 RESUMEN FINAL

```
==============================================
  LOG DE INSTALACIÓN PHASE 3 - COMPLETADO
==============================================

Fecha inicio: _________________
Fecha fin: _________________
Tiempo total: _______ horas _______ minutos

FASES COMPLETADAS:
[ ] Fase 1: Instalación física
[ ] Fase 2: Calibraciones térmicas
[ ] Fase 3: Calibraciones mecánicas
[ ] Fase 4: Calibraciones de calidad
[ ] Fase 5: Primera impresión

ESTADO GENERAL: [ ] ✅ ÉXITO COMPLETO / [ ] ⚠️ PARCIAL / [ ] ❌ FALLÓ

VALORES FINALES CONFIGURACIÓN:
======================================
PID Hotend: Kp=_____ Ki=_____ Kd=_____
PID Bed: Kp=_____ Ki=_____ Kd=_____
Rotation distance: _______ mm
Eddy drive current: _______
Z offset: _______ mm
Bed mesh rango: _______ mm
Pressure advance (PLA): _______
Retraction: _______ mm @ _______ mm/s

PROBLEMAS ENCONTRADOS:
======================================
1. _________________________________________
2. _________________________________________
3. _________________________________________

SOLUCIONES APLICADAS:
======================================
1. _________________________________________
2. _________________________________________
3. _________________________________________

LECCIONES APRENDIDAS:
======================================
_________________________________________
_________________________________________
_________________________________________

PRÓXIMOS PASOS:
======================================
- [ ] Documentar este log en phases/phase3/README.md
- [ ] Actualizar HARDWARE_EVOLUTION.md con valores finales
- [ ] Commit: "Phase 3 installation complete"
- [ ] Comenzar Phase 4 (según roadmap)

```

---

## 📸 ANEXO: Fotos y Evidencias

### Instalación Física
```
Foto 1: Eddy Coil montado en toolhead
Ubicación: _________________________
Descripción: _________________________

Foto 2: Conexiones I2C en EBB42
Ubicación: _________________________
Descripción: _________________________
```

### Calibraciones
```
Screenshot 1: PID tuning hotend graph
Ubicación: _________________________

Screenshot 2: Bed mesh heightmap
Ubicación: _________________________

Screenshot 3: Primera impresión exitosa
Ubicación: _________________________
```

---

## 2025-12-27 - Instalación Física Eddy Completada

**Estado:** Instalación física del Eddy Coil completada con éxito.

**Trabajo realizado:**
- ✅ Eddy Coil montado en toolhead (2-3mm sobre nozzle)
- ✅ Conexiones I2C realizadas (VCC, GND, SCL/PB3, SDA/PB4)
- ✅ Cable routing por drag chain hasta EBB42
- ✅ Documentación fotográfica completada
- ✅ Sensor original mantenido instalado (no desmontado)

**Decisión:** Sensor Z original desconectado eléctricamente pero mantenido físicamente instalado (brazo no desmontado). Solo Eddy Coil operativo - NO hay fallback disponible.

**Próximo paso:** Iniciar calibración Eddy Coil y pruebas G28.

**Tiempo invertido:** ~3 horas (instalación física + routing cables + documentación)

---

**Fin del documento - Actualizar conforme se ejecutan las fases**
