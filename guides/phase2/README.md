# Phase 2: SKR Cableado Básico

**Estado:** ✅ Completada (2025-12-21)
**Tiempo empleado:** ~6 horas
**Dificultad:** Media

---

## 🎯 Objetivo

Montar y cablear la BTT SKR 1.4 Turbo con las conexiones básicas necesarias: alimentación, motores y cama caliente. Preparar la arquitectura para integración EBB42 CAN en Phase 3.

---

## 📊 Resumen Ejecutivo

### Lo que se hizo:

1. **Documentación exhaustiva sistema stock** (31 fotos)
2. **Desconexión completa electrónica original**
3. **Decisión crítica:** Montar SKR en posición superior (óptima para cables)
4. **Fabricación cables custom:**
   - Extensión Motor Z2: JST-XH 4-pin, 60cm
   - Extensión alimentación 24V: 50cm con termorretráctil rojo/azul
5. **Montaje SKR temporal** con bridas (case definitivo pendiente)
6. **Cableado básico completo:**
   - Alimentación 24V → DCIN
   - 4 motores (X, Y, Z1, Z2)
   - Cama caliente (power + termistor)

### Lo que NO se hizo (por diseño):

- ❌ Endstops X, Y: Sensorless homing con TMC2209
- ⏸️ Endstop Z: Sensor Omron en EBB42 (Phase 3)
- ⏸️ Toolhead completo: EBB42 CAN (Phase 3)

---

## 📁 Estructura de Archivos

```
guides/phase2/
├── README.md                       # ⬅️ Este archivo
├── step1_documentation.md          # Documentación wiring stock
├── step2_stock_disconnection.md    # Desconexión electrónica
├── step3_skr_mounting.md           # Montaje SKR posición superior
├── step4_skr_basic_wiring.md       # Cableado alimentación + motores + cama
└── step5_verification.md           # Verificación final

photos/phase2/
├── 01-31_*.jpg                     # Documentación stock (Step 1)
├── 32_motor_z2_extension_cable.jpg # Cable extensión fabricado
├── 33_skr_dcin_power_connector.jpg # Alimentación 24V
├── 34_motors_connected_to_skr.jpg  # Motores conectados
├── 35_heated_bed_power_hb.jpg      # Cama power (HB)
└── 36_heated_bed_thermistor_tb.jpg # Cama termistor (TB)
```

**Total:** 36 fotos documentadas

---

## 🔧 Pasos Completados

### [Step 1: Documentación Wiring Stock](step1_documentation.md) ✅

**Objetivo:** Documentar fotograficamente todo el sistema stock antes de desconectar

**Fotos:** 01-31
- Compartimento electrónica general
- Dual-board system (placa principal + subplaca distribución)
- PSU P360W24V
- Distribution box interior/exterior
- Motores (CoreXY X/Y, dual Z, heated bed)
- Toolhead completo (extrusor, hotend, ventiladores)

**Resultado:** Sistema stock completamente documentado

---

### [Step 2: Desconexión Electrónica Stock](step2_stock_disconnection.md) ✅

**Objetivo:** Desconectar de forma segura y ordenada toda la electrónica original

**Proceso:**
1. Ribbon cable distribución
2. Cables desde subplaca distribución
3. Motores desde placa principal
4. Cama caliente (power + termistor)
5. Alimentación 24V
6. Extracción física placas stock

**Resultado:** Electrónica stock desmontada y almacenada (reversible)

---

### [Step 3: Montaje SKR Posición Superior](step3_skr_mounting.md) ✅

**Objetivo:** Montar SKR en posición óptima y fabricar cables necesarios

**Decisión arquitectónica crítica:**
- **Evaluación:** Posición inferior vs superior
- **Decisión:** Montar en posición superior (donde estaba distribution box)
- **Razón:** Solo necesita 2 extensiones (Z2 + 24V) vs múltiples si va abajo

**Fabricación cables:**
- **Motor Z2:** Extensión 60cm con JST-XH 4-pin (macho + hembra)
  - NO cortar cable original (reversible)
  - Conecta cable 6-pin stock a puerto 4-pin SKR
- **Alimentación 24V:** Extensión 50cm desde PSU
  - Identificación: Termorretráctil rojo (+24V) y azul (GND)
  - Profesional y bien documentado

**Montaje:**
- Temporal con bridas (zip ties) al perfil 2020
- Case definitivo pendiente (se imprimirá cuando impresora funcione)
- Ver `stls/upgrades/README.md` para STL case

**Foto:** 32 (cable extensión Z2)

---

### [Step 4: Cableado Básico SKR](step4_skr_basic_wiring.md) ✅

**Objetivo:** Conectar alimentación, motores y cama caliente a SKR

**Conexiones realizadas:**

**1. Alimentación 24V → DCIN**
- Cable desde PSU compartimento inferior
- Conector barrel jack (centro positivo)
- Termorretráctil rojo/azul para identificación
- Foto: 33

**2. Motores (4)**
- Motor X → Puerto X (CoreXY izq)
- Motor Y → Puerto Y (CoreXY der)
- Motor Z1 → Puerto Z (leadscrew izq)
- Motor Z2 + extensión → Puerto E1 (leadscrew der)
- **Nota:** E0 reservado para extrusor en EBB42
- Foto: 34

**3. Cama Caliente**
- Power (cables gruesos) → Terminal HB
- Termistor (B TEMP) → Conector TB
- Fotos: 35, 36

**Estrategia endstops:**
- X, Y: Sensorless homing (TMC2209 StallGuard)
- Z: Sensor Omron en EBB42 (Phase 3)

---

### [Step 5: Verificación Final](step5_verification.md) ✅

**Objetivo:** Verificar exhaustivamente todo antes de energizar

**Checklist completo:**
- ✅ Hardware SKR y drivers correctos
- ✅ Alimentación con polaridad correcta
- ✅ 4 motores conectados correctamente
- ✅ Cama caliente (power + termistor)
- ✅ Seguridad eléctrica verificada
- ✅ Mecánica de impresora OK

**Estado:** Sistema verificado, listo para Phase 3

---

## 🎯 Arquitectura Final Phase 2

```
                    SKR 1.4 TURBO
                (Posición Superior)
                        │
        ┌───────────────┼───────────────┐
        │               │               │
     DCIN            MOTORS           HEATED BED
        │               │               │
   [24V PSU]    ┌───────┼───────┐   ┌──┴──┐
   (inferior)   │   │   │   │   │   │     │
              [X] [Y] [Z][E1]     [HB]  [TB]
                │   │   │   │      │     │
              Mot Mot Mot Mot    Power Therm
              X   Y   Z1  Z2+ext  Cama  Cama

    PENDIENTE PHASE 3:
    ┌─────────────────────┐
    │   EBB42 CAN         │
    │   (Toolhead)        │
    │   - Motor extrusor  │
    │   - Hotend          │
    │   - Termistor       │
    │   - Ventiladores    │
    │   - Sensor Omron    │
    └─────────────────────┘
            │
        Cable CAN
        (4 hilos)
            │
         ┌──┴──┐
         │ SKR │
```

---

## 📈 Métricas

**Fotos totales:** 36
**Tiempo empleado:** ~6 horas
**Cables fabricados:** 2 (Z2 + 24V)
**Componentes conectados:** 7 (PSU, 4 motores, cama power, cama termistor)
**Errores:** 0
**Incidencias:** 0

---

## 🧠 Decisiones Clave

### 1. Posición SKR: Superior vs Inferior

**Evaluación:**
- Inferior: Múltiples extensiones necesarias, gestión compleja
- Superior: Solo 2 extensiones, cables llegan nativamente

**Decisión:** Superior ✅

**Cita del usuario:**
> "Ahora entiendo el por qué montaban la placa arriba"

---

### 2. EBB42 CAN: Ahora vs Después

**Evaluación:**
- Cablear tradicional ahora: 15+ cables toolhead, luego recablear todo
- EBB42 desde inicio: Solo 4 cables CAN, no recablear

**Decisión:** EBB42 desde inicio ✅

**Cita del usuario:**
> "Lo hacemos bien de una vez"

---

### 3. Cable Extensión Z2: Cortar vs Extensión

**Propuesta inicial:** Cortar cable original 6-pin
**Decisión usuario:** Fabricar extensión con conectores

**Ventaja:** Reversible, no destruye cable original ✅

---

### 4. Identificación Cables: Nuevos vs Termorretráctil

**Propuesta:** Comprar cables de colores
**Decisión usuario:** Usar cable azul + termorretráctil de colores

**Ventaja:** Mismo sistema usado en DCIN, profesional ✅

---

## 🎓 Lecciones Aprendidas

1. **Planificar antes de cablear**
   - Evaluar alcance de todos los cables
   - Elegir posición óptima de controladora
   - Ahorra tiempo y trabajo

2. **Documentar exhaustivamente**
   - Fotos ANTES de desconectar
   - Permite reversión si es necesario
   - Ayuda a otros usuarios

3. **No destruir cables originales**
   - Fabricar extensiones cuando sea necesario
   - Mantiene opciones abiertas
   - Sistema reversible

4. **Identificación profesional**
   - Termorretráctil de colores funciona perfectamente
   - No es necesario comprar cables nuevos
   - Documentar en fotos

5. **Arquitectura desde inicio**
   - Implementar EBB42 CAN desde el principio
   - Evita recablear después
   - Trabajo más limpio

---

## 🔧 Material Utilizado

### Hardware
- BTT SKR 1.4 Turbo (con 5x TMC2209 de Phase 1)
- Bridas (zip ties) para montaje temporal
- Conectores JST-XH 4-pin macho + hembra
- Cable 4 conductores 60cm (extensión Z2)
- Cable 2 conductores 1.5mm² 50cm (extensión 24V)
- Termorretráctil rojo/azul (identificación polaridad)

### Herramientas
- Destornilladores (Phillips, plano)
- Crimpadora JST-XH
- Pelacables
- Tijeras para bridas
- Multímetro (verificaciones opcionales)

---

## ✅ Estado de Componentes

| Componente | Estado | Ubicación | Notas |
|------------|--------|-----------|-------|
| SKR 1.4 Turbo | ✅ Montada | Frame superior | Temporal con bridas |
| PSU P360W24V | ✅ Funcional | Compartimento inferior | Sin cambios |
| Motor X | ✅ Conectado | Puerto X | CoreXY izq |
| Motor Y | ✅ Conectado | Puerto Y | CoreXY der |
| Motor Z1 | ✅ Conectado | Puerto Z | Leadscrew izq |
| Motor Z2 | ✅ Conectado | Puerto E1 | Leadscrew der + ext |
| Cama caliente | ✅ Conectada | HB + TB | Power + termistor |
| Toolhead | ⏸️ Pendiente | Phase 3 | EBB42 CAN |
| Endstops | ❌ NO conectados | - | Sensorless X/Y |

---

## 🚧 Pendiente para Phase 3

**Toolhead EBB42 CAN:**
- [ ] Documentar toolhead stock actual
- [ ] Desconectar toolhead de cables stock
- [ ] Instalar EBB42 en toolhead
- [ ] Conectar componentes a EBB42:
  - Motor extrusor (E0)
  - Calentador hotend
  - Termistor hotend
  - Ventiladores (part cooling, hotend)
  - Sensor Omron TL-Q5MC1-Z
- [ ] Fabricar/tender cable CAN (4 hilos):
  - Cat6 para CAN_H/CAN_L (twisted pair)
  - Cable alimentación para 24V/GND
- [ ] Configurar CAN bus en firmware
- [ ] Primera energización sistema completo

---

## 📚 Referencias

**Documentación relacionada:**
- [Phase 1: SKR Preparación](../phase1/README.md)
- [Phase 3: EBB42 CAN](../../phases/phase3/README.md) *(pendiente crear)*
- [GUIDE.md principal](../../GUIDE.md)
- [STLs Upgrades](../../stls/upgrades/README.md)

**Recursos externos:**
- [SKR 1.4 Turbo pinout](https://github.com/bigtreetech/BIGTREETECH-SKR-V1.3/tree/master/BTT%20SKR%20V1.4)
- [TMC2209 datasheet](https://www.trinamic.com/products/integrated-circuits/details/tmc2209-la/)
- [Klipper sensorless homing](https://www.klipper3d.org/TMC_Drivers.html#sensorless-homing)

---

## 🎉 Logros Phase 2

- ✅ **36 fotos** profesionalmente documentadas
- ✅ **Decisión arquitectónica** crítica correcta (SKR superior)
- ✅ **Fabricación custom** de cables extensión
- ✅ **Sistema preparado** para CAN bus
- ✅ **Cero errores** durante todo el proceso
- ✅ **Completamente reversible** (cables stock intactos)

---

**Completada:** 2025-12-21
**Siguiente:** [Phase 3: Toolhead EBB42 CAN](../../phases/phase3/README.md)
