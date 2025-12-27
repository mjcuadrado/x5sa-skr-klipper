# Phase 3: Hardware Stock + EBB42 - Configuración Production-Ready

**Estado:** ✅ Completado
**Fecha:** 2025-12-27
**Modo comunicación:** USB (no CAN bus)

---

## 🎯 Filosofía del Proyecto

### Phase 3: Configuración Production-Ready con Hardware Stock

**Objetivo principal:** Crear una impresora completamente funcional y production-ready usando hardware stock, con toda la potencia de Klipper y Eddy Coil.

**¿Por qué esta configuración es production-ready y NO temporal?**
1. **Completamente funcional:** Todas las capacidades de Klipper disponibles
2. **Calidad profesional:** Eddy Coil rapid scan, adaptive meshing, macros Voron-level
3. **Estable:** Hardware stock probado y confiable
4. **Indefinido:** El usuario puede permanecer en Phase 3 sin limitaciones
5. **Phase 12 es opcional:** Solo necesario para multicolor o casos de uso específicos

**Componentes stock que SE USAN en Phase 3 (PRODUCTION-READY):**
- ✅ Motor extrusor Titan clone stock (frame-mounted)
- ✅ Termistor stock (100K NTC)
- ✅ Cartucho calentador stock
- ✅ Ventilador hotend stock
- ✅ Ventilador part cooling stock
- ✅ Hotend E3D V6 clone stock
- ✅ **Eddy Coil V1.0** para probing (±0.01mm precisión)

**Resultado:** Sistema completamente funcional con calidad profesional

---

### Phase 12: Upgrade OPCIONAL a Voron Toolhead

**Objetivo:** Toolhead Voron Stealthburner para casos de uso específicos

**Hardware premium opcional (Phase 12):**
- 🔮 Voron Stealthburner toolhead completo
- 🔮 Orbiter 2.0/2.5 extrusor (direct drive)
- 🔮 Dragonfly BMO hotend
- 🔮 PT100 sensor alta precisión (opcional)
- 🔮 Ventiladores premium silenciosos
- 🔮 Preparado para multicolor (MMU/AMS)

**¿Cuándo migrar a Phase 12?**
- ✅ Si necesitas imprimir multicolor (MMU/AMS)
- ✅ Si necesitas flexibles frecuentemente (TPU, direct drive)
- ✅ Si buscas estética Voron + LEDs Neopixel
- ❌ NO es necesario si solo imprimes PLA/PETG/ABS single-color

**Phase 3 es suficiente para la mayoría de usuarios**

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

## 🚀 Roadmap del Proyecto (12 Fases - Metodología 4-8-12)

```
Phase 0-2 ✅ Base electrónica (SKR + TMC2209)

GRUPO 1: STOCK (Phases 3-5) ← CONFIGURACIÓN PRODUCTION-READY
│
Phase 3 ✅ INSTALAR Eddy → G28 funcional
│         └─> Hardware STOCK + Eddy Coil V1.0
│         └─> EBB42 detrás del frame
│         └─> ⭐ PRODUCTION-READY - Completamente funcional
│
Phase 4 ⏳ CALIBRAR Stock → Perfiles funcionales
Phase 5 ⏳ VALIDAR 4-8-12 → Stock 100% validado
│
├─ Phase 5.5 (Opcional): Instalación Fleje PEI
│
GRUPO 2: VORON (Phases 6-8) - UPGRADE OPCIONAL
│
Phase 6 🔮 INSTALAR Voron → Toolhead nuevo (si se desea)
Phase 7 🔮 CALIBRAR Voron → Perfiles Orbiter
Phase 8 🔮 VALIDAR 4-8-12 → Voron 100%
│
GRUPO 3: MULTICOLOR (Phases 9-11) - UPGRADE OPCIONAL
│
Phase 9-11 🔮 MMU/AMS (solo si necesario)
Phase 12   🔮 Optimización final
```

**Nota:** Phase 3 es un milestone completo. Phases 6-12 son upgrades opcionales.

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
