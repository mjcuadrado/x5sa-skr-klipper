# Phase 3: Toolhead EBB42 - Migración Hardware Stock

**Estado:** 📋 En planificación
**Fecha:** 2025-12-21
**Modo comunicación:** USB (no CAN bus)

---

## 🎯 Filosofía del Proyecto

### Phase 3-5: Base Funcional Stock

**Objetivo principal:** Crear una impresora funcional básica usando **TODO** el hardware stock existente.

**¿Por qué hardware stock?**
1. **Rápido:** Migración directa sin cambios de componentes
2. **Seguro:** Hardware conocido y probado
3. **Funcional:** Permite tener la impresora operativa pronto
4. **Estratégico:** Una vez funcional, puedes imprimir mejoras para Phase 12

**Componentes stock que SE USAN en Phase 3:**
- ✅ Motor extrusor stock
- ✅ Termistor stock (100K NTC típico)
- ✅ Cartucho calentador stock
- ✅ Ventilador hotend stock
- ✅ Ventilador part cooling stock
- ✅ Hotend stock completo

**ÚNICA excepción:** Sensor Z Omron (mejora clara de precisión, definitiva)

---

### Phase 12: Upgrade Completo Toolhead

**Objetivo:** Toolhead profesional completo nuevo tipo Voron

**Aquí SÍ usaremos el hardware premium:**
- 🚀 Stealthburner toolhead completo
- 🚀 Orbiter 2.0/2.5 extrusor
- 🚀 Dragonfly BMO hotend
- 🚀 PT100 sensor alta precisión
- 🚀 Ventiladores premium silenciosos
- 🚀 Cartucho calentador nuevo

**¿Por qué esperar a Phase 12?**
1. **Una sola intervención:** Cambio completo toolhead en un solo paso
2. **Sin desperdiciar trabajo:** No desmontar/montar componentes múltiples veces
3. **Aprender primero:** Familiarizarte con Klipper antes de hardware complejo
4. **Imprimir mejoras:** Usar Phase 3-5 funcional para imprimir soportes/mejoras Phase 12

---

## 📋 Documentos Phase 3

### Planificación
- **[PLANNING.md](./PLANNING.md)** - Plan completo Phase 3, decisiones críticas
- **[MATERIAL_INVENTORY.md](./MATERIAL_INVENTORY.md)** - Inventario material necesario
- **[DECISION_CAN_vs_USB.md](./DECISION_CAN_vs_USB.md)** - Decisión comunicación USB (tomada)

### Ejecución (próximos pasos)
- `step1_stock_toolhead_documentation.md` - Documentar toolhead stock actual
- `step2_ebb42_mounting.md` - Montar EBB42 en toolhead
- `step3_component_migration.md` - Migrar componentes stock a EBB42
- `step4_cabling.md` - Tender cables USB y 24V
- `step5_verification.md` - Verificación física completa

---

## 🎯 Objetivo Phase 3

Migrar todos los componentes **STOCK** del toolhead actual a la nueva placa EBB42 CAN V1.2, estableciendo comunicación **USB** con el MiniPC (host Klipper).

**Resultado esperado:**
- ✅ EBB42 montada físicamente en toolhead
- ✅ Todos los componentes stock migrados a EBB42
- ✅ Cable USB tendido y conectado al MiniPC
- ✅ Cable alimentación 24V desde SKR a EBB42
- ✅ Sistema listo para firmware (Phase 4)

**NO incluye (Phase 12):**
- ❌ PT100 (usar termistor stock)
- ❌ Orbiter (usar motor extrusor stock)
- ❌ Dragonfly BMO (usar hotend stock)
- ❌ Ventiladores premium (usar ventiladores stock)

---

## 🚀 Roadmap Simplificado

```
Phase 1 ✅ Documentación impresora stock
Phase 2 ✅ SKR 1.4 Turbo instalación y cableado base
│
Phase 3 📋 Migración toolhead stock a EBB42 (AQUÍ ESTAMOS)
│         └─> Hardware STOCK + sensor Omron
│
Phase 4    Firmware Klipper (SKR + EBB42 modo USB)
Phase 5    Configuración Klipper básica
│
Phase 6-11 Calibración, tuning, mejoras mecánicas
│         └─> IMPRESORA FUNCIONAL, imprimir mejoras
│
Phase 12   Toolhead completo nuevo
          └─> Stealthburner + Orbiter + Dragonfly BMO + PT100
          └─> AQUÍ sí: todo el hardware premium
│
Phase 13+  Mejoras adicionales (cámara, panel LCD, etc.)
```

---

## 💡 Ventajas de Este Enfoque

### Ventajas técnicas:
1. **Menos variables:** Hardware conocido = debugging más fácil
2. **Validación incremental:** EBB42 funciona antes de añadir complejidad
3. **Reversible:** Si algo falla, hardware stock es backup

### Ventajas prácticas:
1. **Más rápido:** Migración directa sin esperar piezas
2. **Menos coste inicial:** No gastar en componentes aún
3. **Impresora funcional pronto:** Empiezas a imprimir en Phase 6-7

### Ventajas estratégicas:
1. **Aprendes Klipper:** Con hardware simple primero
2. **Imprimes mejoras:** Soportes, ductos, etc. para Phase 12
3. **Upgrade inteligente:** Cuando sepas exactamente qué necesitas

---

## 📌 Decisiones Tomadas

### ✅ Comunicación: USB
- Más simple que CAN bus
- Debugging fácil
- Recomendado por comunidad Voron/Klipper actual

### ✅ Hardware: Stock en Phase 3-5
- Termistor stock (PT100 en Phase 12)
- Ventiladores stock (premium en Phase 12)
- Calentador stock (nuevo en Phase 12)
- Motor extrusor stock (Orbiter en Phase 12)

### ⚠️ Decisiones pendientes:
1. Montaje EBB42 (temporal/impreso/adaptado)
2. Cable USB (tipo, longitud, ruta)
3. Cable 24V (longitud, origen, destino)
4. Sensor Omron (ahora vs después con sensorless)
5. Estrategia trabajo (mesa vs in-situ)

---

## 📚 Referencias

### Documentación oficial:
- [BTT EBB42 GitHub](https://github.com/bigtreetech/EBB)
- [Klipper Multi-MCU](https://www.klipper3d.org/Multi_MCU_Homing.html)

### Guías comunidad:
- [Voron Design - EBB USB Setup](https://docs.vorondesign.com/build/electrical/v2_ebb_usb.html)
- [Klipper Discourse](https://klipper.discourse.group/)

---

## 🔄 Estado Actual

**Phase 3 - En planificación**

**Próximo paso:**
1. Revisar y completar decisiones en `PLANNING.md`
2. Completar inventario en `MATERIAL_INVENTORY.md`
3. Documentar toolhead stock actual (Step 1)
4. Proceder con migración física

---

**Última actualización:** 2025-12-21
**Enfoque confirmado:** Hardware stock Phase 3-5, hardware premium Phase 12
