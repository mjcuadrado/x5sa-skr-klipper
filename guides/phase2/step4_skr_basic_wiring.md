# Phase 2, Step 4: Cableado Básico SKR

**Estado:** ✅ Completado (2025-12-21)
**Tiempo estimado:** 2-3 horas
**Dificultad:** Media

---

## 🎯 Objetivo

Conectar la alimentación, motores y cama caliente a la SKR 1.4 Turbo, estableciendo las conexiones básicas necesarias para el funcionamiento del sistema.

---

## ⚠️ Precauciones de Seguridad

**ANTES de conectar cualquier cable:**

- [x] **Impresora apagada**
- [x] **Cable de alimentación desconectado de la pared**
- [x] **Verificar polaridad** en conexiones de potencia
- [x] **NO forzar conectores** - si no entra fácilmente, verificar orientación
- [x] **Cables sin tensión mecánica** - dejar holgura suficiente

**Durante el cableado:**
- Trabajar de forma ordenada (un cable a la vez)
- Verificar cada conexión antes de pasar a la siguiente
- Gestionar cables para evitar enredos

---

## 📋 Material Necesario

- [x] SKR 1.4 Turbo montada (Step 3)
- [x] Cables ya preparados:
  - Alimentación 24V (50cm con termorretráctil rojo/azul)
  - Motor Z2 con extensión (60cm)
  - Cables stock de motores X, Y, Z1
  - Cables cama caliente (power + termistor)
- [x] Destornillador plano pequeño (para terminales de tornillo)
- [x] Bridas/velcro para gestión de cables (opcional pero recomendado)

---

## 🔌 Esquema de Conexiones SKR 1.4 Turbo

### Mapa de Conectores (vista de placa)

```
┌─────────────────────────────────────┐
│  SKR 1.4 TURBO                      │
│                                     │
│  [X] [Y] [Z] [E0] [E1]  ← Motores  │
│                                     │
│  [DCIN]  ← Alimentación 24V         │
│                                     │
│  [HB]    ← Heated Bed Power         │
│  [TB]    ← Thermistor Bed           │
│                                     │
│  [X-] [Y-] [Z-]  ← Endstops         │
│  (NO usados - sensorless)           │
│                                     │
│  [TFT] [USB]  ← Comunicación        │
└─────────────────────────────────────┘
```

---

## 🔧 Procedimiento de Cableado

### Conexión 1: Alimentación 24V → DCIN

**Componente:** Cable 24V desde PSU (preparado en Step 3)

**Conector SKR:** **DCIN** (barrel jack connector, parte superior de la placa)

**Procedimiento:**
1. Localizar el conector DCIN en la SKR (barrel jack, centro positivo)
2. Verificar el cable de alimentación:
   - Conductor con termorretráctil **ROJO** → centro (+24V)
   - Conductor con termorretráctil **AZUL** → exterior (GND)
3. Insertar el conector barrel jack en DCIN
4. Verificar que encaja completamente
5. Verificar que el cable tiene holgura (sin tensión)

**Especificaciones DCIN:**
- Tipo: Barrel jack 5.5mm x 2.5mm
- Polaridad: Centro positivo (+24V), exterior negativo (GND)
- Máximo: 20A continuo

**Verificación:**
- [ ] Conector insertado completamente
- [ ] Polaridad correcta (rojo=centro, azul=exterior)
- [ ] Cable con holgura suficiente
- [ ] Conexión firme (no se mueve al tirar suavemente)

**Estado:** ✅ Alimentación conectada

**Foto:** `photos/phase2/33_skr_dcin_power_connector.jpg`

---

### Conexión 2: Motores Stepper

**Componentes:** 4 motores NEMA17

**Conectores SKR:**
- **X** → Motor X (CoreXY, superior izquierdo)
- **Y** → Motor Y (CoreXY, superior derecho)
- **Z** → Motor Z1 (leadscrew izquierdo)
- **E1** → Motor Z2 (leadscrew derecho, CON extensión)

**Nota importante E0 vs E1:**
- **E0:** Reservado para motor extrusor (irá en EBB42, Phase 3)
- **E1:** Usado como segundo motor Z (Z2)

---

#### Motor X (CoreXY)

**Procedimiento:**
1. Localizar cable del Motor X (superior izquierdo del frame)
2. Identificar conector JST-XH 4-pin del motor
3. Localizar puerto **X** en la SKR (junto a driver TMC2209 X)
4. Conectar respetando orientación del conector (solo entra de una forma)
5. Verificar que encaja con "click"

**Estado:** ✅ Motor X conectado

---

#### Motor Y (CoreXY)

**Procedimiento:**
1. Localizar cable del Motor Y (superior derecho del frame)
2. Identificar conector JST-XH 4-pin del motor
3. Localizar puerto **Y** en la SKR (junto a driver TMC2209 Y)
4. Conectar respetando orientación del conector
5. Verificar que encaja con "click"

**Estado:** ✅ Motor Y conectado

---

#### Motor Z1 (Leadscrew Izquierdo)

**Procedimiento:**
1. Localizar cable del Motor Z1 (leadscrew lado izquierdo)
2. Identificar conector JST-XH 4-pin del motor
3. Localizar puerto **Z** en la SKR (junto a driver TMC2209 Z)
4. Conectar respetando orientación del conector
5. Verificar que encaja con "click"

**Estado:** ✅ Motor Z1 conectado

---

#### Motor Z2 (Leadscrew Derecho) CON EXTENSIÓN

**Procedimiento:**
1. Localizar cable del Motor Z2 (leadscrew lado derecho)
2. **Conectar primero la extensión:**
   - Conector hembra JST-XH 4-pin de la extensión → cable original del motor
   - Verificar que encaja correctamente
3. **Conectar extensión a SKR:**
   - Conector macho JST-XH 4-pin de la extensión → puerto **E1** en SKR
   - Localizar puerto E1 (junto a driver TMC2209 E1)
   - Conectar respetando orientación
   - Verificar que encaja con "click"
4. Gestionar el cable de extensión para evitar enredos

**Notas:**
- Motor Z2 usa puerto E1 (no E0)
- E0 queda libre para el extrusor (que irá en EBB42)
- La configuración de Klipper designará E1 como segundo motor Z

**Estado:** ✅ Motor Z2 conectado (con extensión a E1)

---

**Foto de motores conectados:** `photos/phase2/34_motors_connected_to_skr.jpg`

**Resumen conexiones motores:**
```
Motor X  (CoreXY izq)      → SKR puerto X
Motor Y  (CoreXY der)      → SKR puerto Y
Motor Z1 (leadscrew izq)   → SKR puerto Z
Motor Z2 (leadscrew der)   → SKR puerto E1 (con extensión 60cm)
Motor E0 (extrusor)        → (Futuro: EBB42 CAN, Phase 3)
```

---

### Conexión 3: Cama Caliente (Heated Bed)

La cama caliente requiere 2 conexiones independientes:
1. **Alimentación** (cables gruesos de potencia)
2. **Termistor** (cable fino de sensor)

---

#### 3A: Alimentación Cama → HB

**Componente:** Cables gruesos de la cama caliente (rojo/negro típicamente)

**Conector SKR:** **HB** (Heated Bed, terminal de tornillo)

**Ubicación HB:** Parte inferior izquierda de la SKR

**Procedimiento:**
1. Localizar los cables gruesos de alimentación de la cama caliente
2. Identificar polaridad:
   - Rojo/marrón → Positivo
   - Negro/azul → Negativo
3. Localizar terminal **HB** en la SKR (terminal de tornillo, 2 posiciones)
4. **Aflojar** los tornillos del terminal HB con destornillador plano
5. Insertar cables en el terminal:
   - Cable positivo (rojo) → posición marcada como "+"
   - Cable negativo (negro) → posición marcada como "-"
6. **Importante:** Insertar solo el cobre pelado, NO aislante
7. **Apretar firmemente** los tornillos del terminal
8. **Verificar tracción:** Tirar suavemente del cable - no debe salir

**Especificaciones HB:**
- Tensión: 24V DC
- Corriente máxima: 15A continuo
- Potencia máxima: ~360W

**Verificación:**
- [ ] Cables insertados en posiciones correctas (+/-)
- [ ] Tornillos apretados firmemente
- [ ] NO hay hilos de cobre sueltos fuera del terminal
- [ ] Cables no se salen al tirar suavemente
- [ ] No hay tensión mecánica en los cables

**Estado:** ✅ Alimentación cama conectada a HB

**Foto:** `photos/phase2/35_heated_bed_power_hb.jpg`

---

#### 3B: Termistor Cama → TB

**Componente:** Cable fino del termistor de cama (etiquetado "B TEMP")

**Conector SKR:** **TB** (Temperature Bed, conector 2-pin)

**Ubicación TB:** Cerca del terminal HB, conector pequeño de 2 pines

**Procedimiento:**
1. Localizar cable del termistor de cama (cable fino, etiquetado "B TEMP")
2. Identificar el conector (típicamente JST-XH 2-pin o similar)
3. Localizar conector **TB** en la SKR (2-pin, cerca de HB)
4. Conectar el termistor al conector TB
5. Verificar que encaja correctamente (suave "click")

**Notas sobre termistores:**
- Los termistores NO tienen polaridad (funciona en cualquier dirección)
- Cable delicado - NO tirar con fuerza
- Típicamente termistor 100K NTC (verificar en config Klipper)

**Verificación:**
- [ ] Conector insertado completamente
- [ ] No hay tensión mecánica en el cable
- [ ] Cable del termistor no toca partes calientes/móviles

**Estado:** ✅ Termistor cama conectado a TB

**Foto:** `photos/phase2/36_heated_bed_thermistor_tb.jpg`

---

## 📊 Resumen de Conexiones Completadas

| Componente | Puerto SKR | Estado | Foto |
|------------|------------|--------|------|
| Alimentación 24V | DCIN | ✅ | 33 |
| Motor X (CoreXY) | X | ✅ | 34 |
| Motor Y (CoreXY) | Y | ✅ | 34 |
| Motor Z1 | Z | ✅ | 34 |
| Motor Z2 + extensión | E1 | ✅ | 34 |
| Cama power | HB | ✅ | 35 |
| Cama termistor | TB | ✅ | 36 |

**Conexiones NO realizadas (por diseño):**
- ❌ Endstops X, Y, Z: Sensorless homing con TMC2209
- ⏸️ Extrusor (E0): Irá en EBB42 CAN (Phase 3)
- ⏸️ Termistor hotend: Irá en EBB42 CAN (Phase 3)
- ⏸️ Calentador hotend: Irá en EBB42 CAN (Phase 3)
- ⏸️ Ventiladores: Irán en EBB42 CAN (Phase 3)
- ⏸️ Sensor Z (Omron): Irá en EBB42 CAN (Phase 3)

---

## 🎯 Arquitectura Final Phase 2

```
┌─────────────────────────────────────────┐
│         SKR 1.4 TURBO                   │
│         (Posición Superior)             │
│                                         │
│  DCIN ← 24V desde PSU (compartimento    │
│         inferior, 50cm extensión)       │
│                                         │
│  X ← Motor X (CoreXY izq)               │
│  Y ← Motor Y (CoreXY der)               │
│  Z ← Motor Z1 (leadscrew izq)           │
│  E1 ← Motor Z2 (leadscrew der + ext)    │
│                                         │
│  HB ← Cama caliente (power)             │
│  TB ← Cama caliente (termistor)         │
│                                         │
│  E0, Hotend, Fans, Probe → EBB42 CAN    │
│                            (Phase 3)    │
└─────────────────────────────────────────┘
```

---

## 🧹 Gestión de Cables (Recomendado)

Una vez completadas las conexiones, organizar los cables:

**Técnicas:**
1. **Bridas:** Agrupar cables paralelos
2. **Velcro reutilizable:** Alternativa a bridas (ajustable)
3. **Separar potencia/señal:** Cables gruesos (HB) separados de señales (motores)
4. **Holgura en movimientos:** Cables de cama con suficiente holgura para movimiento Z
5. **Evitar esquinas agudas:** Cables con curvas suaves (radio amplio)

**Puntos de fijación:**
- Usar ranuras del perfil 2020
- Clips impresos (cuando impresora funcione)
- Cable chains (upgrade futuro)

**Estado:** ⏸️ Gestión básica realizada, optimización futura

---

## ✅ Checklist de Verificación Final

Antes de energizar por primera vez, verificar:

**Alimentación:**
- [x] Cable 24V conectado a DCIN
- [x] Polaridad correcta (rojo=centro/+, azul=exterior/-)
- [x] PSU apagada (aún NO encender)

**Motores:**
- [x] 4 motores conectados (X, Y, Z, E1)
- [x] Conectores insertados completamente (click)
- [x] Motor Z2 con extensión funcionando

**Cama Caliente:**
- [x] Cables de potencia en HB (tornillos apretados)
- [x] Termistor en TB
- [x] No hay hilos sueltos

**Seguridad:**
- [x] No hay cables pelados expuestos
- [x] No hay cables tocando partes móviles
- [x] No hay tensión mecánica excesiva
- [x] Cables organizados (no enredados)

**Pendiente para Phase 3:**
- [ ] Toolhead completo (EBB42 CAN)
- [ ] Cable CAN de 4 hilos
- [ ] Configuración firmware

---

## 🔧 Troubleshooting

### Problema: Cable no llega a su conector

**Solución:**
- Verificar que la SKR está en la posición correcta
- Fabricar extensión si es necesario (como hicimos con Z2)
- NO estirar cables con tensión

### Problema: Conector JST no entra

**Solución:**
- Verificar orientación (probar 180° girado)
- NO forzar - debe entrar suavemente
- Verificar que no hay pines doblados

### Problema: Terminal de tornillo no agarra el cable

**Solución:**
- Verificar que solo se inserta cobre (sin aislante)
- Pelar más cable si es necesario
- Apretar más el tornillo (firmemente pero sin romper)
- Considerar estañar la punta del cable si son hilos muy finos

### Problema: No sé si el cable está bien conectado

**Solución:**
- Tirar SUAVEMENTE del cable - no debe salir
- Verificar que el conector hace "click"
- En terminales de tornillo: debe ser imposible sacar el cable tirando con fuerza moderada

---

## 📸 Galería de Fotos

**Fotos de este paso:**
1. `33_skr_dcin_power_connector.jpg` - Alimentación 24V en DCIN
2. `34_motors_connected_to_skr.jpg` - Vista general motores conectados
3. `35_heated_bed_power_hb.jpg` - Cama caliente alimentación HB
4. `36_heated_bed_thermistor_tb.jpg` - Cama caliente termistor TB

---

## ➡️ Siguiente Paso

Cableado básico completado. Continuar con:

**[Phase 2, Step 5: Verificación Final Phase 2](step5_verification.md)**

---

**Completado:** 2025-12-21
**Tiempo real empleado:** ~2 horas
**Incidencias:** Ninguna - todo funcionó a la primera
