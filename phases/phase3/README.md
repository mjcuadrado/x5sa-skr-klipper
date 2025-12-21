# Phase 3 — Toolhead EBB42 CAN

**Objetivo:** Instalar placa EBB42 CAN en el toolhead y conectar todos los componentes del extrusor/hotend. Establecer comunicación CAN bus con SKR 1.4 Turbo.

**Estado:** 📋 En planificación (2025-12-21)

---

## Planificación

📋 **Documento de planificación:** [`guides/phase3/PLANNING.md`](../../guides/phase3/PLANNING.md)

**Decisiones críticas pendientes:**
1. Montaje físico EBB42 (temporal/impreso/adaptado)
2. Sensor temperatura hotend (termistor stock vs PT100)
3. Cable CAN (longitud, colores, conectores, ruta)
4. Ventiladores (stock vs silenciosos)
5. Sensor Omron (ahora vs después)
6. Estrategia de trabajo (desmontaje completo vs in-situ)

---

## Índice de Guías (Pendiente)

📁 **Guías detalladas en:** [`guides/phase3/`](../../guides/phase3/)

**Pasos tentativos:**
1. Documentación toolhead stock
2. Toma de decisiones
3. Preparación hardware
4. Montaje EBB42
5. Migración componentes
6. Cable CAN
7. Verificación física

*(Se completarán tras tomar decisiones)*

---

## Requisitos Previos

- [x] Phase 2 completada (SKR cableada)
- [ ] Decisiones de planificación tomadas
- [ ] Hardware confirmado:
  - BTT EBB42 CAN V1.2
  - Sensor Omron TL-Q5MC1-Z
  - PT100 sensor (si se decide usar)
  - Cable Cat6 (~2m)
  - Cable alimentación (~2m)
- [ ] Herramientas: destornilladores, multímetro, crimpadora

---

## Objetivo al Finalizar Phase 3

### Hardware a Instalar

- [ ] EBB42 CAN montada en toolhead
- [ ] Motor extrusor → EBB42 E0
- [ ] Calentador hotend → EBB42 HE
- [ ] Termistor/PT100 → EBB42 TH0/PT100
- [ ] Ventilador hotend → EBB42 FAN0
- [ ] Ventilador part cooling → EBB42 FAN1
- [ ] Sensor Omron → EBB42 PROBE
- [ ] Cable CAN (4 hilos) tendido y conectado

### Arquitectura Final

```
┌─────────────────────────────────┐
│ SKR 1.4 TURBO (Frame Superior)  │
│                                 │
│ CAN Transceiver                 │
│ ↕ Cable CAN (4 hilos)           │
└─────────────────────────────────┘
         ↓
    CAN_H, CAN_L, 24V, GND
         ↓
┌─────────────────────────────────┐
│ EBB42 CAN (Toolhead)            │
│                                 │
│ E0    ← Motor extrusor          │
│ HE    ← Calentador hotend       │
│ TH0   ← Termistor/PT100         │
│ FAN0  ← Hotend cooling          │
│ FAN1  ← Part cooling            │
│ PROBE ← Sensor Omron Z          │
└─────────────────────────────────┘
```

---

## Estimación Temporal

| Opción | Tiempo |
|--------|--------|
| Rápida (temporal + termistor stock) | ~4 horas |
| Completa (definitivo + PT100 + Omron) | ~6 horas |

**Distribución:**
- Documentación: 30-45 min
- Fabricación cable CAN: 1-1.5h
- Montaje EBB42: 30-60 min
- Migración componentes: 1.5-2h
- Cable CAN instalación: 45-60 min
- Verificación: 30-45 min

---

## Material Necesario

### Hardware Principal
- [ ] BTT EBB42 CAN V1.2
- [ ] Sensor Omron TL-Q5MC1-Z
- [ ] PT100 sensor + cartucho (si se usa)
- [ ] Soporte EBB42 (temporal o impreso)

### Cables
- [ ] Cable Cat6 (~2m)
- [ ] Cable alimentación 1.5mm² (~2m)
- [ ] Termorretráctil (colores)
- [ ] Conectores (según decisión)

### Herramientas
- [ ] Destornilladores (Phillips, plano, Allen)
- [ ] Multímetro
- [ ] Crimpadora
- [ ] Pelacables
- [ ] Tijeras/cutter
- [ ] Bridas/velcro

---

## Reglas de Seguridad

1. **NUNCA** trabajar con impresora energizada
2. **SIEMPRE** verificar polaridad CAN y 24V
3. **NUNCA** forzar conectores en EBB42
4. **SIEMPRE** verificar continuidad cable CAN
5. **SIEMPRE** dejar holgura para movimientos toolhead

---

## Punto de Rollback

Si algo falla:
- Toolhead stock documentado con fotos
- Cables stock identificados
- Sistema puede volver a configuración Phase 2

---

## Enlaces Útiles

- **Planificación:** [`guides/phase3/PLANNING.md`](../../guides/phase3/PLANNING.md)
- **Guías detalladas:** [`guides/phase3/`](../../guides/phase3/) *(pendiente)*
- **Phase anterior:** [Phase 2](../phase2/README.md)
- **Phase siguiente:** Phase 4 - Firmware Klipper

---

## Próximo Paso

1. Revisar [`guides/phase3/PLANNING.md`](../../guides/phase3/PLANNING.md)
2. Tomar decisiones críticas
3. Documentar toolhead stock actual
4. Iniciar instalación EBB42

---

**Inicio planificación:** 2025-12-21
**Estado:** Pendiente decisiones del usuario
