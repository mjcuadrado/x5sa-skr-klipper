# Phase 2 — SKR Cableado Básico

**Objetivo:** Montar SKR 1.4 Turbo en la impresora y cablear conexiones básicas (alimentación, motores, cama caliente). Preparar arquitectura para EBB42 CAN.

**Estado:** ✅ Completada (2025-12-21)

---

## Índice de Guías Paso a Paso

📁 **Guías detalladas en:** [`guides/phase2/`](../../guides/phase2/)

1. [**Documentación Wiring Stock**](../../guides/phase2/step1_documentation.md)
   - 31 fotos completas del sistema stock
   - Dual-board system identificado
   - PSU, motores, cama, toolhead documentados

2. [**Desconexión Electrónica Stock**](../../guides/phase2/step2_stock_disconnection.md)
   - Desconexión segura y ordenada
   - Placas stock almacenadas (reversible)

3. [**Montaje SKR Posición Superior**](../../guides/phase2/step3_skr_mounting.md)
   - **Decisión crítica:** SKR en posición superior (óptima)
   - Fabricación cable extensión Motor Z2 (JST-XH 4-pin, 60cm)
   - Fabricación cable extensión 24V (50cm, termorretráctil)
   - Montaje temporal con bridas

4. [**Cableado Básico SKR**](../../guides/phase2/step4_skr_basic_wiring.md)
   - Alimentación 24V → DCIN
   - 4 motores (X, Y, Z1, Z2 con extensión)
   - Cama caliente (power HB + termistor TB)

5. [**Verificación Final Phase 2**](../../guides/phase2/step5_verification.md)
   - Checklist exhaustivo
   - Sistema listo para Phase 3

📊 **Resumen ejecutivo:** [`guides/phase2/README.md`](../../guides/phase2/README.md)

---

## Requisitos Previos

- [x] Phase 1 completada (SKR con drivers TMC2209)
- [x] Herramientas: destornilladores, multímetro, crimpadora
- [x] Materiales:
  - Conectores JST-XH 4-pin (macho + hembra)
  - Cable 4 conductores (~60cm para Z2)
  - Cable 2 conductores 1.5mm² (~50cm para 24V)
  - Termorretráctil (rojo/azul)
  - Bridas (zip ties)

---

## Decisiones Arquitectónicas Tomadas

### 1. Posición SKR: Superior ✅

**Evaluación:**
- ❌ Inferior: Múltiples cables no llegan
- ✅ Superior: Solo 2 extensiones necesarias (Z2 + 24V)

**Resultado:** SKR montada donde estaba distribution box stock

### 2. EBB42 CAN: Desde Inicio ✅

**Evaluación:**
- ❌ Toolhead stock temporal: 15+ cables, luego recablear
- ✅ EBB42 desde inicio: 4 cables CAN, sin recablear

**Resultado:** Toolhead completo se hará en Phase 3 con EBB42

### 3. Estrategia Endstops: Sensorless ✅

**Evaluación:**
- X, Y: Sensorless homing (TMC2209 StallGuard) - sin endstops físicos
- Z: Sensor Omron TL-Q5MC1-Z en EBB42 (Phase 3)

**Resultado:** No se conectaron endstops físicos a SKR

---

## Resultado al Finalizar Phase 2

### Hardware Instalado

- ✅ SKR 1.4 Turbo montada en frame superior (temporal con bridas)
- ✅ Alimentación 24V: PSU → DCIN (cable 50cm con termorretráctil)
- ✅ Motor X → Puerto X
- ✅ Motor Y → Puerto Y
- ✅ Motor Z1 → Puerto Z
- ✅ Motor Z2 → Puerto E1 (con cable extensión 60cm)
- ✅ Cama caliente power → HB
- ✅ Cama caliente termistor → TB

### NO Conectado (por diseño)

- ❌ Endstops X, Y, Z (sensorless X/Y, sensor Z en EBB42)
- ⏸️ Toolhead completo (Phase 3 con EBB42 CAN)

### Documentación Generada

- **36 fotos** completas (stock + cableado)
- **5 guías** paso a paso detalladas
- **2 cables custom** fabricados y documentados

---

## Arquitectura Final Phase 2

```
┌─────────────────────────────┐
│   SKR 1.4 TURBO             │
│   (Frame Superior)          │
├─────────────────────────────┤
│ DCIN ← 24V (PSU inferior)   │
│ X    ← Motor X (CoreXY)     │
│ Y    ← Motor Y (CoreXY)     │
│ Z    ← Motor Z1 (leadscrew) │
│ E1   ← Motor Z2 + ext 60cm  │
│ HB   ← Cama power           │
│ TB   ← Cama termistor       │
└─────────────────────────────┘

PENDIENTE PHASE 3:
┌─────────────────────────────┐
│ EBB42 CAN (Toolhead)        │
│ - Motor extrusor            │
│ - Hotend (heater + sensor)  │
│ - Ventiladores              │
│ - Sensor Omron Z            │
│ ↕ Cable CAN (4 hilos)       │
│ ↓ SKR                       │
└─────────────────────────────┘
```

---

## Métricas

| Métrica | Valor |
|---------|-------|
| Tiempo empleado | 6 horas |
| Fotos documentadas | 36 |
| Cables fabricados | 2 (Z2 + 24V) |
| Componentes conectados | 7 |
| Errores | 0 |
| Incidencias | 0 |

---

## Lecciones Aprendidas

1. **Planificar posición de controladora ANTES de cablear**
   - Evaluar alcance de cables nativo ahorra extensiones

2. **No destruir cables originales**
   - Extensiones con conectores JST mantienen reversibilidad

3. **Identificación profesional**
   - Termorretráctil de colores = solución efectiva y documentada

4. **Arquitectura desde inicio**
   - EBB42 desde Phase 3 evita recablear todo después

5. **Documentar exhaustivamente**
   - 36 fotos = seguridad y referencia futura

---

## Reglas de Seguridad Phase 2

1. **NUNCA** energizar sin triple verificación
2. **SIEMPRE** verificar polaridad 24V (termorretráctil rojo/azul)
3. **SIEMPRE** verificar que NO hay cortocircuitos
4. **NUNCA** forzar conectores
5. **SIEMPRE** dejar holgura en cables móviles (cama Z)

---

## Punto de Rollback

Si algo falla en Phase 3+:
- Placas stock almacenadas y funcionales
- Todos los cables stock intactos
- Sistema reversible a estado original

---

## Enlaces Útiles

- **Guías detalladas:** [`guides/phase2/`](../../guides/phase2/)
- **Fotos:** [`photos/phase2/`](../../photos/phase2/)
- **Phase anterior:** [Phase 1](../phase1/README.md)
- **Phase siguiente:** [Phase 3](../phase3/README.md)

---

## Preparación para Phase 3

**Pendiente para Phase 3 - Toolhead EBB42 CAN:**

- [ ] Documentar toolhead stock actual
- [ ] Instalar EBB42 en toolhead
- [ ] Conectar componentes a EBB42:
  - Motor extrusor (E0)
  - Calentador hotend
  - Termistor/PT100 hotend
  - Ventiladores (hotend + part cooling)
  - Sensor Omron TL-Q5MC1-Z
- [ ] Fabricar cable CAN (4 hilos: CAN_H, CAN_L, 24V, GND)
- [ ] Tender cable CAN desde SKR a toolhead
- [ ] Verificación física completa

**Ver planificación:** [`guides/phase3/PLANNING.md`](../../guides/phase3/PLANNING.md)

---

**Completada:** 2025-12-21
**Tiempo real:** 6 horas
**Siguiente:** [Phase 3 - Toolhead EBB42 CAN](../phase3/README.md)
