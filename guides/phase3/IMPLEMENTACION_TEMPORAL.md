# Phase 3 - Implementación Hardware Stock (EBB42 Detrás del Frame)

**Versión:** 2.0
**Fecha:** 2025-12-27
**Estado:** ✅ Completado - Production-Ready

---

## 🎯 Objetivo

Conectar hardware stock del toolhead a la EBB42, montando la EBB42 **detrás del frame** (no en toolhead), creando una configuración completamente funcional y production-ready con hardware stock.

**Filosofía Phase 3:** Esta es una configuración PRODUCTION-READY estable, NO temporal. El usuario puede permanecer indefinidamente en este estado. La migración a toolhead Voron (Phase 12) es un upgrade opcional para casos de uso específicos.

---

## 📐 Arquitectura Phase 3 (Production-Ready)

```
┌────────────────────────────────────────────────────────┐
│ FRAME SUPERIOR (Nueva ubicación electrónica)          │
│                                                        │
│  ┌──────────────┐  USB-C    ┌───────────────────┐     │
│  │ SKR 1.4 TB   │ ←(30cm)→  │ EBB42 (TEMPORAL)  │     │
│  │              │           │                   │     │
│  │ E0 ← Motor E │  24V      │ VIN/GND ← 24V     │     │
│  │    (lateral) │ ←(30cm)→  │ USB-C   ← USB     │     │
│  │              │           │                   │     │
│  │ FAN2 → 24V ──┼───────────→ HE      ← Heater  │ ←── Cables stock
│  │    always-on │           │ TH0     ← Therm   │ ←── toolhead
│  └──────────────┘           │ FAN0    ← Part fan│ ←── (ya llegan aquí)
│                             │ FAN1    ← HE fan  │ ←──
│                             │ PROBE   ←─────────┼─┐
│                             │ E0 (sin usar)     │ │
│                             └───────────────────┘ │
└────────────────────────────────────────────────────┼─┘
                                                     │
                                    Cable nuevo (~1.5m)
                                    Sensor Omron Z
                                                     ↓
                                            ┌────────────┐
                                            │ TOOLHEAD   │
                                            │ (stock)    │
                                            │            │
                                            │ Sensor Z   │
                                            │ Omron NC   │
                                            └────────────┘
```

---

## ✅ Ventajas Configuración Phase 3 (Stock Production-Ready)

1. **Cables stock utilizables:** Todo el cableado del toolhead ya llega al frame superior
2. **Solo 1 cable nuevo largo:** Sensor Omron al toolhead (~1.5m)
3. **Cables cortos entre placas:** USB-C y 24V solo ~30-50cm
4. **Troubleshooting fácil:** Ambas placas accesibles sin desarmar toolhead
5. **Testing seguro:** Cambios mínimos, rollback simple
6. **Menor inversión:** No necesitas cables largos caros hasta Phase 12

---

## 📦 Material Necesario Específico

### Hardware (Ya Disponible)
- [x] SKR 1.4 Turbo (flasheada, funcionando)
- [x] EBB42 CAN V1.2 (flasheada, funcionando)
- [x] Sensor Omron TL-Q5MC1-Z
- [x] Cables stock toolhead (heater, thermistor, fans)

### Cables a Fabricar

#### 1. Cable USB-C Corto (SKR ↔ EBB42)
- **Longitud:** 30-50cm
- **Tipo:** USB-C a USB-C (o USB-A según puerto SKR disponible)
- **Requisito:** Datos (no solo carga)
- **Opcional:** Anillo ferrita (si tienes disponible)

#### 2. Cable 24V Corto (SKR → EBB42)
- **Longitud:** 30-50cm
- **Cable:** 2x conductores 1.5mm² (AWG 16/18)
- **Identificación:** Termorretráctil rojo (+24V), azul (GND)
- **Conectores:**
  - Extremo SKR: Terminales tornillo en FAN2 o HE1
  - Extremo EBB42: JST-XH 2-pin o terminales tornillo

#### 3. Cable Sensor Omron Largo (EBB42 → Toolhead)
- **Longitud:** 1.5-2m (medir según ruta)
- **Cable:** 3 hilos señal (calibre delgado OK, 22-24AWG)
- **Conectores:** JST-XH 3-pin ambos extremos
- **Pines:**
  - NC (Normally Closed) - marrón típico
  - C (Common) - azul típico
  - NO (Normally Open) - negro típico (NO conectar en EBB42)

### Conectores para Cables Stock

Si los cables stock no tienen conectores compatibles:
- [ ] JST-XH 2-pin (x2 sets: heater, thermistor)
- [ ] Dupont 2-pin (x2 sets: fans)

### Herramientas
- [ ] Multímetro (CRÍTICO)
- [ ] Crimpadora JST-XH/Dupont
- [ ] Destornilladores Phillips y planos
- [ ] Pelacables
- [ ] Pistola calor o encendedor (termorretráctil)
- [ ] Tijeras/cutter
- [ ] Cinta doble cara o bridas (montaje EBB42)

---

## 🚀 Plan de Implementación

### Fase 1: Preparación (15-30 min)

#### 1.1 Documentación Estado Actual
- [ ] **CRÍTICO:** Fotografiar toolhead desde todos los ángulos (10+ fotos)
- [ ] Fotografiar conexiones actuales en frame superior
- [ ] Fotografiar placa stock actual (para rollback)
- [ ] Anotar qué cables llegan desde toolhead

#### 1.2 Identificación Cables Stock
Con etiquetas adhesivas o cinta papel, marcar claramente:
- [ ] Cable hotend heater (2 hilos, típicamente rojo/negro)
- [ ] Cable thermistor (2 hilos delgados)
- [ ] Cable part cooling fan (2 hilos, típicamente rojo/negro)
- [ ] Cable hotend fan (2 hilos, típicamente rojo/negro)
- [ ] **Anotar polaridades con multímetro si es posible**

#### 1.3 Apagar Impresora
- [ ] Apagar impresora (interruptor PSU)
- [ ] Desconectar cable AC (seguridad extra)
- [ ] Esperar 1-2 min (descarga condensadores)

---

### Fase 2: Fabricación Cable 24V (30-45 min)

#### 2.1 Medición
- [ ] Medir distancia SKR FAN2/HE1 → posición temporal EBB42
- [ ] Añadir 10-15cm holgura
- [ ] Longitud típica: ~30-50cm

#### 2.2 Fabricación
- [ ] Cortar cable 2x1.5mm² según medida
- [ ] Pelar extremos (~5-7mm)
- [ ] Aplicar termorretráctil ROJO en conductor +24V
- [ ] Aplicar termorretráctil AZUL en conductor GND
- [ ] Contraer termorretráctil con pistola calor

#### 2.3 Conectores

**Extremo SKR (terminales tornillo):**
- [ ] Pelar 5-7mm
- [ ] Si cable fino: doblar punta o añadir terminal pala

**Extremo EBB42 (JST-XH o tornillo):**
- [ ] Si JST-XH: crimpar pines según polaridad
- [ ] Si tornillo: pelar 5-7mm

#### 2.4 Verificación
- [ ] Multímetro: Continuidad extremo a extremo (rojo-rojo, azul-azul)
- [ ] Multímetro: NO continuidad entre rojo y azul (no cortocircuito)
- [ ] Etiquetar cable: "24V SKR→EBB42"

---

### Fase 3: Preparación Cable USB (10 min)

#### 3.1 Cable USB-C
- [ ] Verificar longitud adecuada (~30-50cm)
- [ ] Si tienes cable largo: doblarlo con bridas (no cortar)
- [ ] Opcional: Instalar anillo ferrita si disponible

#### 3.2 Test
- [ ] Conectar USB entre PC y cualquier dispositivo USB
- [ ] Verificar que es cable de DATOS (no solo carga)
- [ ] `lsusb` debe detectar dispositivo

---

### Fase 4: Fabricación Cable Sensor Omron (30-45 min)

#### 4.1 Medición Ruta
- [ ] Medir ruta: Frame superior (EBB42) → Toolhead (sensor)
- [ ] Añadir 20-30cm holgura para movimientos
- [ ] Longitud típica: ~1.5-2m

#### 4.2 Identificar Cables Sensor Actual
Con multímetro en modo continuidad:
- [ ] Identificar pin NC: continuidad con C cuando sensor NO triggered
- [ ] Identificar pin C: común
- [ ] Identificar pin NO: continuidad con C cuando sensor triggered

**Típicamente:**
- Marrón = NC
- Azul = C (común)
- Negro = NO

#### 4.3 Fabricación
- [ ] Cortar cable 3 hilos según medida
- [ ] Extremo EBB42: crimpar JST-XH 3-pin
  - Pin 1: NC (a EBB42 PROBE signal)
  - Pin 2: C (a EBB42 GND)
  - Pin 3: NO (sin conectar)
- [ ] Extremo sensor: crimpar JST-XH 3-pin o conectar directo a sensor

#### 4.4 Verificación
- [ ] Multímetro continuidad NC-C (cerrado sin trigger)
- [ ] Trigger manual sensor
- [ ] Multímetro: NC-C abierto (triggered), NO-C cerrado
- [ ] Etiquetar cable: "Omron Probe NC-C-NO"

---

### Fase 5: Montaje Físico EBB42 (20-30 min)

#### 5.1 Selección Ubicación
Cerca de SKR en frame superior, considerando:
- [ ] Acceso a conectores EBB42
- [ ] Alejado de fuentes calor
- [ ] No interfiere con movimientos mecánicos
- [ ] Cables SKR→EBB42 alcanzan sin tensión
- [ ] Cables stock toolhead alcanzan sin tensión

#### 5.2 Montaje Temporal

**Opción A: Cinta doble cara**
- [ ] Limpiar superficie frame con alcohol
- [ ] Aplicar cinta doble cara resistente (3M VHB)
- [ ] Presionar firmemente EBB42
- [ ] Añadir 1-2 bridas como seguridad adicional

**Opción B: Bridas directas**
- [ ] Identificar puntos anclaje en frame
- [ ] Pasar bridas por agujeros montaje EBB42
- [ ] Apretar sin exceso (permitir acceso a conectores)

#### 5.3 Verificación
- [ ] EBB42 firmemente montada (no se mueve)
- [ ] Acceso fácil a conectores
- [ ] No toca partes metálicas con GND

---

### Fase 6: Conexiones Eléctricas (45-60 min)

**⚠️ IMPORTANTE: Impresora APAGADA durante todo el proceso**

#### 6.1 Conexión 24V (SKR → EBB42)

**En SKR:**
- [ ] Identificar terminales FAN2 o HE1 (verificar pinout SKR)
- [ ] Aflojar tornillos terminales
- [ ] Insertar cable ROJO en terminal +24V
- [ ] Insertar cable AZUL en terminal GND
- [ ] Apretar tornillos firmemente
- [ ] **Verificar que cables no se sueltan con tirón suave**

**En EBB42:**
- [ ] Identificar terminales VIN (+) y GND (-)
- [ ] Conectar ROJO a VIN (+)
- [ ] Conectar AZUL a GND (-)
- [ ] Apretar tornillos si aplica

**Verificación PRE-energizado:**
- [ ] Multímetro: continuidad SKR GND ↔ EBB42 GND
- [ ] Multímetro: NO continuidad VIN ↔ GND (no cortocircuito)

#### 6.2 Conexión USB (SKR → EBB42)
- [ ] Conectar extremo a puerto USB SKR
- [ ] Conectar extremo USB-C a EBB42
- [ ] Verificar que conectores entran completamente

#### 6.3 Desconexión Componentes Stock de Placa Antigua

**Fotografiar ANTES de desconectar cada cable**
- [ ] Desconectar hotend heater de placa stock
- [ ] Desconectar thermistor de placa stock
- [ ] Desconectar part cooling fan de placa stock
- [ ] Desconectar hotend fan de placa stock
- [ ] **Sensor Omron:** Dejar en toolhead (conectaremos cable nuevo)

#### 6.4 Conexión Componentes a EBB42

**Heater Hotend → EBB42 HE:**
- [ ] Verificar resistencia heater con multímetro (~12-20Ω típico 24V/40W)
- [ ] Adaptar conector JST-XH si necesario
- [ ] Conectar a terminales HE+ y HE- de EBB42
- [ ] Verificar: NO continuidad heater ↔ GND

**Thermistor → EBB42 TH0:**
- [ ] Medir resistencia thermistor (~100kΩ @ 25°C para NTC 100K)
- [ ] Adaptar conector JST-XH si necesario
- [ ] Conectar a terminales TH0+ y TH0-
- [ ] Verificar: cable thermistor no toca partes metálicas

**Part Cooling Fan → EBB42 FAN0:**
- [ ] Identificar polaridad fan (rojo +, negro -)
- [ ] Adaptar conector Dupont si necesario
- [ ] Conectar a FAN0 de EBB42
- [ ] Aplicar pequeño punto de hot glue para asegurar

**Hotend Fan → EBB42 FAN1:**
- [ ] Identificar polaridad fan (rojo +, negro -)
- [ ] Adaptar conector Dupont si necesario
- [ ] Conectar a FAN1 de EBB42
- [ ] Aplicar pequeño punto de hot glue

#### 6.5 Conexión Sensor Omron

**En Toolhead (extremo sensor):**
- [ ] Desconectar sensor Omron actual de cable stock
- [ ] Identificar pines NC, C, NO (verificar con multímetro)
- [ ] Conectar cable nuevo largo fabricado:
  - NC (marrón típico) → cable a EBB42 signal
  - C (azul típico) → cable a EBB42 GND
  - NO (negro típico) → **sin conectar**

**En EBB42:**
- [ ] Conectar NC → PROBE signal (PB9)
- [ ] Conectar C → GND
- [ ] NO: sin conectar

**Test Pre-Energizado:**
- [ ] Multímetro continuidad signal-GND: cerrado (NC sin trigger)
- [ ] Trigger manual sensor
- [ ] Multímetro: abierto (NC triggered)

#### 6.6 Gestión Cables
- [ ] Agrupar cables con bridas suavemente
- [ ] Dejar holgura adecuada
- [ ] Cable sensor Omron: tender por ruta segura a toolhead
- [ ] Verificar que ningún cable tiene tensión

---

### Fase 7: Primera Energización (15-20 min)

#### 7.1 Triple Verificación PRE-Energizado

**CHECKLIST CRÍTICO - NO SALTAR:**
- [ ] Polaridad 24V verificada: ROJO=+24V, AZUL=GND
- [ ] NO cortocircuito 24V: multímetro VIN-GND infinito
- [ ] NO cortocircuito heater-GND: infinito
- [ ] Todos los conectores firmes
- [ ] Ningún cable pelado tocando partes metálicas
- [ ] Motor E conectado a SKR E0 (no a EBB42)

#### 7.2 Energizado Gradual

**Paso 1: Solo SKR**
- [ ] Conectar cable AC
- [ ] Encender PSU
- [ ] LED SKR enciende: ✅
- [ ] Medir voltaje en FAN2/HE1: debe ser ~24V DC
- [ ] Si OK → continuar. Si no → APAGAR y revisar

**Paso 2: Con EBB42**
- [ ] EBB42 ya está conectada a 24V (paso anterior)
- [ ] Verificar LED EBB42 enciende: ✅
- [ ] Medir voltaje VIN de EBB42: debe ser ~24V DC
- [ ] Si OK → continuar. Si no → APAGAR inmediatamente

#### 7.3 Verificación USB

**En PC/Raspberry Klipper:**
```bash
ls /dev/serial/by-id/
```

Debe aparecer:
- `usb-Klipper_lpc1769_XXXXX-if00` (SKR)
- `usb-Klipper_stm32g0b1xx_XXXXX-if00` (EBB42)

**Si EBB42 no aparece:**
- [ ] Verificar cable USB tiene transferencia datos (no solo carga)
- [ ] Probar otro puerto USB en SKR
- [ ] Verificar LED EBB42 encendido (power OK)
- [ ] Ver troubleshooting en FLASHEO_EBB42_EXITOSO.md

---

### Fase 8: Configuración Klipper (20-30 min)

#### 8.1 Backup Configuración Actual
```bash
cd ~/printer_data/config
cp printer.cfg printer.cfg.backup.pre-ebb42
```

#### 8.2 Actualizar printer.cfg

La configuración dual-MCU ya está en el archivo. Verificar secciones:

**[mcu EBBCan]** - Ya configurado ✅
```ini
[mcu EBBCan]
serial: /dev/serial/by-id/usb-Klipper_stm32g0b1xx_XXXXX-if00
```

**[extruder]** - Ya configurado ✅
```ini
[extruder]
# Motor en SKR E0
step_pin: P2.0
dir_pin: !P0.5
enable_pin: !P2.1
# Heater en EBB42
heater_pin: EBBCan:PB13    # EBB42 V1.2 heater output (V1.0 usaba PA2)
# Thermistor en EBB42
sensor_pin: EBBCan:PA3
```

**[fan]** - Part cooling - Ya configurado ✅
```ini
[fan]
pin: EBBCan:PA0  # FAN0
```

**[heater_fan hotend_fan]** - Ya configurado ✅
```ini
[heater_fan hotend_fan]
pin: EBBCan:PA1  # FAN1
heater: extruder
heater_temp: 50.0
```

**[probe]** - Omron NC - Ya configurado ✅
```ini
[probe]
pin: ^EBBCan:PB9  # NC con pullup
```

**[output_pin ebb42_power]** - 24V always-on
```ini
[output_pin ebb42_power]
pin: P2.4  # FAN2 de SKR (verificar pinout según tu conexión)
pwm: False
value: 1
shutdown_value: 0
```

#### 8.3 Restart Klipper
```bash
# En consola Klipper o SSH
sudo systemctl restart klipper
```

#### 8.4 Verificar Conexión

**En Mainsail/Fluidd console:**
```
# Debe aparecer en log:
MCU 'mcu' connected
MCU 'EBBCan' connected
```

**Si hay errores:**
- [ ] Revisar `/tmp/klippy.log` para detalles
- [ ] Verificar serial IDs correctos en printer.cfg
- [ ] Ver CONFIGURACION_DUAL_MCU.md troubleshooting

---

### Fase 9: Testing Funcional (45-60 min)

**⚠️ SUPERVISIÓN CONSTANTE durante todos los tests**

#### 9.1 Test Thermistor

**Consola Klipper:**
```
# Debe mostrar temperatura razonable
QUERY_THERMISTOR
# Típico: 20-25°C ambiente
```

**Posibles errores:**
- "ADC out of range" → Thermistor desconectado o tipo incorrecto
- Temperatura negativa → Verificar tipo sensor en config
- Temperatura > 300°C → Verificar conexión

#### 9.2 Test Heater (CRÍTICO - Supervisar)

**PASO 1: Test bajo (50°C)**
```
SET_HEATER_TEMPERATURE HEATER=extruder TARGET=50
```

- [ ] Temperatura empieza a subir gradualmente
- [ ] Cuando T > 50°C: hotend fan arranca automáticamente ✅
- [ ] Temperatura se estabiliza cerca de 50°C

**Apagar:**
```
SET_HEATER_TEMPERATURE HEATER=extruder TARGET=0
```

- [ ] Temperatura empieza a bajar
- [ ] Cuando T < 50°C: hotend fan se apaga automáticamente ✅

**PASO 2: Test operativo (200°C)**
Solo si test 50°C fue exitoso:
```
SET_HEATER_TEMPERATURE HEATER=extruder TARGET=200
```

- [ ] Temperatura sube gradualmente sin overshooting excesivo
- [ ] Se estabiliza en ~200°C
- [ ] NO hay errores "thermal runaway"

**Apagar:**
```
SET_HEATER_TEMPERATURE HEATER=extruder TARGET=0
```

#### 9.3 Test Part Cooling Fan
```
SET_FAN_SPEED FAN=fan SPEED=0.5
# Fan gira a 50%

SET_FAN_SPEED FAN=fan SPEED=1.0
# Fan gira a 100%

SET_FAN_SPEED FAN=fan SPEED=0
# Fan se detiene
```

#### 9.4 Test Probe Omron (CRÍTICO)

**Test 1: Query básico**
```
QUERY_PROBE
```

- [ ] Sin trigger: debe responder "probe: open"
- [ ] Trigger manual (presionar sensor con dedo/destornillador)
- [ ] Con trigger: debe responder "probe: TRIGGERED"
- [ ] Soltar sensor
- [ ] De nuevo: "probe: open"

**Test 2: Fail-safe**
- [ ] Desconectar cable probe de EBB42
- [ ] `QUERY_PROBE` → debe mostrar "TRIGGERED" o error
- [ ] **Esto garantiza que cable suelto = NO imprime**
- [ ] Reconectar cable
- [ ] `QUERY_PROBE` → "probe: open" (OK)

**Si falla:**
- Ver troubleshooting en EBB42_INTEGRATION.md sección probe
- Verificar cableado NC-C (NO desconectado)
- Verificar `pin: ^EBBCan:PB9` tiene pullup `^`

#### 9.5 Test Homing

**XY (sensorless):**
```
G28 X Y
```
- [ ] Homing XY completa sin errores

**Z (con probe):**
```
G28 Z
```
- [ ] Z baja lentamente
- [ ] Se detiene al detectar cama (sensor triggered)
- [ ] NO crash del nozzle

**Si Z crashes:**
- **EMERGENCY STOP inmediatamente**
- Revisar configuración probe (posible inversión lógica)
- Revisar cableado NC/NO

#### 9.6 Calibración Probe Offset

```
PROBE_CALIBRATE
```

Sigue instrucciones en pantalla:
1. Probe mide altura
2. Toolhead se posiciona en centro
3. Ajusta Z con paper test hasta que papel pase justo
4. `ACCEPT`
5. `SAVE_CONFIG`

**Resultado:** `z_offset` guardado en printer.cfg

#### 9.7 Bed Mesh

```
BED_MESH_CALIBRATE
```

- [ ] Ejecuta sondeo de cama (5x5 por defecto)
- [ ] Completa sin errores
- [ ] `SAVE_CONFIG`

---

### Fase 10: Documentación Final (15-20 min)

#### 10.1 Fotografías Post-Instalación
- [ ] EBB42 montada en frame superior (varios ángulos)
- [ ] Conexiones en EBB42 (close-up)
- [ ] Conexiones en SKR (close-up)
- [ ] Cable sensor Omron tendido a toolhead
- [ ] Vista general del frame superior con ambas placas

#### 10.2 Actualizar Configuración
- [ ] Verificar que `printer.cfg` tiene valores calibrados:
  - `z_offset` actualizado
  - Bed mesh guardado
- [ ] Crear backup configuración funcional:
```bash
cp printer.cfg printer.cfg.phase3.working
```

#### 10.3 Documentar Issues
Si hubo problemas durante instalación:
- [ ] Anotar problema
- [ ] Anotar solución aplicada
- [ ] Actualizar troubleshooting si es nuevo

---

## ✅ Checklist Final Verificación

### Hardware
- [ ] EBB42 firmemente montada en frame superior
- [ ] SKR y EBB42 ambas con LED encendido
- [ ] Cable USB-C conectado SKR ↔ EBB42
- [ ] Cable 24V conectado SKR → EBB42
- [ ] Cables stock toolhead conectados a EBB42
- [ ] Cable sensor Omron tendido a toolhead sin tensión
- [ ] Motor E conectado a SKR E0 (NO a EBB42)

### Eléctrico
- [ ] Voltaje 24V medido en VIN EBB42: ~24V DC
- [ ] NO cortocircuitos (verificado con multímetro)
- [ ] Polaridades correctas (rojo=+, azul=GND)
- [ ] Hot glue aplicado en Dupont fans

### Klipper
- [ ] Ambas MCUs detectadas: `ls /dev/serial/by-id/`
- [ ] Klipper conecta sin errores: "MCU 'EBBCan' connected"
- [ ] NO errores en `/tmp/klippy.log`

### Funcionalidad
- [ ] Thermistor lee temperatura razonable (20-25°C ambiente)
- [ ] Heater calienta correctamente (test 50°C + 200°C OK)
- [ ] Hotend fan arranca automáticamente cuando T > 50°C
- [ ] Part cooling fan controlable por PWM (0-100%)
- [ ] Probe responde: `QUERY_PROBE` alterna open/triggered
- [ ] Probe fail-safe: cable suelto = triggered (seguro)
- [ ] Homing XY funciona (sensorless)
- [ ] Homing Z funciona sin crash (probe OK)

### Calibración
- [ ] `z_offset` calibrado y guardado
- [ ] Bed mesh generado y guardado
- [ ] Valores guardados en `printer.cfg`

### Documentación
- [ ] Fotos ANTES (stock) - 10+ fotos
- [ ] Fotos DESPUÉS (EBB42 instalada) - 10+ fotos
- [ ] Backup `printer.cfg.phase3.working` creado
- [ ] Issues/soluciones anotadas

---

## 🚨 Troubleshooting Específico Montaje Temporal

### Problema: Cables stock toolhead no alcanzan EBB42

**Causa:** EBB42 montada demasiado lejos

**Solución:**
1. Re-ubicar EBB42 más cerca del punto donde llegan cables
2. O extender cables stock con empalmes + termorretráctil
3. Mantener heater y thermistor sin empalmes (preferible)

### Problema: Cable sensor Omron demasiado corto

**Solución:**
1. Fabricar cable más largo (añadir 50cm más)
2. O crear empalme intermedio con JST-XH
3. Verificar continuidad post-empalme

### Problema: No hay espacio en frame superior para ambas placas

**Solución:**
1. Considerar montaje vertical (una sobre otra)
2. O montaje lateral extendido
3. Usar soportes impresos 3D para crear espacio adicional
4. Si imposible: rollback a plan original (EBB42 en toolhead)

### Problema: FAN2/HE1 no tiene 24V always-on

**Solución:**
1. Verificar configuración `[output_pin ebb42_power]` en printer.cfg
2. Si puerto no funciona: usar puerto HE1 alternativo
3. O usar salida PSU directa con fusible inline

---

## 📊 Métricas Estimadas Montaje Temporal

| Métrica | Estimación |
|---------|------------|
| Tiempo total | 3-4 horas |
| Cables fabricados | 3 (24V corto, USB corto adaptado, Omron largo) |
| Cables adaptados | 4 (heater, thermistor, 2x fans) |
| Tests funcionales | 7 |
| Ahorro vs montaje toolhead | ~2 horas + €30 materiales |

---

## 🔄 Upgrade Opcional a Voron Toolhead (Phase 12)

Cuando llegue Phase 12 (Stealthburner + Orbiter v2):

**Pasos:**
1. Desmontar EBB42 de frame superior
2. Montar EBB42 en Stealthburner
3. Fabricar/comprar cables largos USB + 24V (~2m)
4. Migrar motor E a EBB42
5. Re-cablear todo con cable chain adecuado
6. Retirar cable sensor Omron largo (sensor integrado en Stealthburner)

**Ventaja configuración actual Phase 3:**
- Ya tienes experiencia con EBB42
- Configuración Klipper validada y production-ready
- Sistema estable y funcional
- Troubleshooting conocido
- Phase 12 es puramente opcional (solo si necesitas multicolor o direct drive)

---

## 🎯 Siguiente Paso

Una vez completado este documento:

**Phase 3 COMPLETADA ✅**

**Próxima phase:** Phase 4 - Calibraciones y Tuning

---

**Preparado por:** mjcuadrado + Claude Code
**Versión:** 1.0
**Fecha:** 2025-12-21
**Estado:** ✅ Listo para implementar
