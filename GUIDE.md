# GUÍA DE MIGRACIÓN: Tronxy X5SA → Klipper con SKR 1.4 Turbo

**Guía completa paso a paso con fotos reales para migrar tu Tronxy X5SA/Pro a Klipper**

---

## 🎯 Para quién es esta guía

Esta guía es para ti si:
- ✅ Tienes una **Tronxy X5SA o X5SA Pro**
- ✅ Quieres migrar a **Klipper** para mejor rendimiento
- ✅ Buscas una guía **paso a paso con fotos reales**
- ✅ Quieres entender **cada paso** sin asumir conocimientos previos
- ✅ Prefieres hacer el upgrade **de forma profesional y documentada**

---

## 🛒 Lista de la Compra

### Mínimo Imprescindible (Phase 1-5)

| Componente | Cantidad | Precio aprox | Enlace |
|------------|----------|--------------|--------|
| BTT SKR 1.4 Turbo | 1 | ~30-40€ | - |
| TMC2209 drivers | 5 | ~25-35€ | - |
| Cables Dupont | Set | ~10€ | - |
| Jumpers | 10+ | ~5€ | - |
| **Conectores JST-XH 4-pin** (macho+hembra) | 2-4 | ~5€ | Para extensiones motores |
| **Cable 4 conductores** | ~1-2m | ~5€ | Para extensiones motores |

**Total mínimo:** ~85-105€

### Upgrade Completo (Phase 1-11)

| Componente | Cantidad | Precio aprox |
|------------|----------|--------------|
| BTT SKR 1.4 Turbo | 1 | ~30-40€ |
| TMC2209 drivers | 5 | ~25-35€ |
| BTT EBB42 CAN V1.2 | 1 | ~25-35€ |
| PT100 sensor (1.5m, Y-terminal) | 1 | ~15-25€ |
| Omron TL-Q5MC1-Z | 1 | ~20-30€ |
| ADXL345 (GY-291) | 1 | ~5-10€ (opcional si usas EBB42) |
| DC-DC XL4015 | 1 | ~5-10€ |
| Cables, conectores, fundas | Set | ~30-50€ |
| Cartucho calefactor 6x20mm | 1 | ~5-10€ |

**Total completo:** ~160-245€

### Hardware Opcional (Phase 12)

- Fleje PEI magnético
- Extrusor Orbiter v2
- Hotend Dragon/Rapido
- Ventiladores Noctua

---

## 📖 Cómo Usar Esta Guía

### Filosofía

1. **Un paso a la vez** - No saltes pasos ni mezcles fases
2. **Fotos obligatorias** - Cada paso tiene fotos ANTES/DESPUÉS
3. **Verificación constante** - Validar antes de avanzar
4. **Reversible** - Siempre posible volver atrás
5. **Sin prisas** - Mejor invertir tiempo que romper hardware

### Formato de Cada Paso

Cada guía de paso incluye:
- ✅ **Objetivo claro** - Qué vamos a lograr
- ✅ **Material necesario** - Qué necesitas a mano
- ✅ **Fotos obligatorias** - Checklist de fotos a hacer
- ✅ **Procedimiento** - Paso a paso detallado
- ✅ **Validación** - Cómo verificar que funcionó
- ✅ **Troubleshooting** - Qué hacer si falla
- ✅ **Siguiente paso** - Enlace al siguiente

---

## 🗺️ Fases del Proyecto

### Phase 0: Baseline ✅
**Estado:** Completada
**Objetivo:** Documentar estado inicial

### Phase 1: SKR 1.4 Turbo - Preparación 🔄
**Estado:** En curso
**Objetivo:** Placa con drivers, sin cablear
**Tiempo estimado:** 2-3 horas

**Pasos:**
1. [SKR stock sin tocar](guides/phase1/step1_skr_stock.md)
2. [Configuración jumpers UART](guides/phase1/step2_uart_jumpers.md) ⬅️ **SIGUIENTE**
3. [Orientación drivers TMC2209](guides/phase1/step3_driver_orientation.md)
4. [Instalación física drivers](guides/phase1/step4_driver_installation.md)
5. [Verificación visual final](guides/phase1/step5_verification.md)

### Phase 2: Cableado Básico 📋
**Estado:** Pendiente
**Objetivo:** Motores + endstops + alimentación
**Tiempo estimado:** 4-6 horas

*(Se completará cuando se inicie)*

### Phase 3: Firmware Klipper 📋
**Estado:** Pendiente
**Objetivo:** Klipper corriendo, movimientos básicos
**Tiempo estimado:** 2-3 horas

### Phase 4: Cama + Sensor Inductivo 📋
**Estado:** Pendiente
**Objetivo:** Bed heating + Z probing
**Tiempo estimado:** 2-3 horas

### Phase 5: Toolhead Stock 📋
**Estado:** Pendiente
**Objetivo:** Primera impresión funcional
**Tiempo estimado:** 3-4 horas

### Phase 6-7: EBB42 CAN 📋
**Estado:** Pendiente
**Objetivo:** Toolhead con comunicación CAN
**Tiempo estimado:** 5-6 horas

### Phase 8-9: PT100 + ADXL345 📋
**Estado:** Pendiente
**Objetivo:** Sensores de alta precisión
**Tiempo estimado:** 4-6 horas

### Phase 10-11: Input Shaper + Calibraciones 📋
**Estado:** Pendiente
**Objetivo:** Sistema completamente optimizado
**Tiempo estimado:** 6-10 horas

### Phase 12: Upgrades PRO 📋
**Estado:** Futuro
**Objetivo:** Hardware premium opcional

---

## 📁 Estructura del Repositorio

```
x5sa-skr-klipper/
├── GUIDE.md                    # ⬅️ Estás aquí
├── ROADMAP.md                  # Plan técnico del proyecto
├── HARDWARE_REFERENCE.md       # Especificaciones técnicas
├── guides/                     # Guías paso a paso
│   ├── phase1/
│   ├── phase2/
│   └── ...
├── photos/                     # Fotos reales del proceso
│   ├── phase1/
│   ├── phase2/
│   └── ...
├── stls/                       # Archivos STL
│   ├── calibration/            # Piezas de calibración
│   └── upgrades/               # Mejoras impresas
├── klipper/                    # Configuración Klipper
│   └── printer.cfg
└── phases/                     # Documentación técnica por fase
    ├── phase0/
    ├── phase1/
    └── ...
```

---

## 🔧 Herramientas Necesarias

### Básicas
- [ ] Destornilladores (Phillips, plano)
- [ ] Llaves Allen/hexagonales
- [ ] Pinzas de punta fina
- [ ] Cortaalambres/pelacables
- [ ] Multímetro digital

### Seguridad
- [ ] Alfombrilla ESD
- [ ] Pulsera antiestática
- [ ] Gafas de seguridad

### Opcionales pero recomendadas
- [ ] Calibre (pie de rey)
- [ ] Crimpadora Dupont/JST
- [ ] Soldador (para modificaciones avanzadas)
- [ ] Impresora de etiquetas

---

## 📦 Piezas de Calibración

Archivos STL disponibles en `stls/calibration/`:

**Para Phase 5 (primera impresión):**
- `xyz_cube.stl` - Cubo de calibración XYZ (20mm)
- `first_layer_test.stl` - Test de primera capa

**Para Phase 10 (calibraciones finales):**
- `temperature_tower.stl` - Torre de temperatura
- `retraction_test.stl` - Test de retracción
- `flow_calibration.stl` - Calibración de flujo
- `pressure_advance_test.stl` - Test de pressure advance

**Para Phase 11 (input shaper):**
- `ringing_test.stl` - Test de ringing/ghosting

---

## 🏗️ Piezas Impresas para Upgrades

Archivos STL disponibles en `stls/upgrades/`:

*(Se irán añadiendo conforme se diseñen/prueben)*

**Planeadas:**
- Soporte EBB42 para toolhead
- Soportes cable chain
- Soporte sensor inductivo
- Soporte ADXL345 (si externo)
- Cable management clips

---

## ⚙️ Configuración Klipper

La configuración de Klipper está versionada en este repositorio:

**Ubicación:** `klipper/printer.cfg`

**Enlace simbólico en el sistema:**
```bash
~/printer_data/config/printer.cfg -> /ruta/al/repo/klipper/printer.cfg
```

**Versionado:**
- Cada fase importante = commit en Git
- Backups automáticos antes de cambios mayores
- Comentarios explicativos en español

---

## ❓ Preguntas Frecuentes

### ¿Puedo hacer solo algunas fases?

Sí. Las fases están diseñadas para ser modulares:
- **Mínimo funcional:** Phase 1-5 (SKR + Klipper básico)
- **Recomendado:** Phase 1-11 (sistema completo optimizado)
- **Opcional:** Phase 12 (upgrades premium)

### ¿Necesito saber programar?

No. Esta guía asume cero conocimientos previos. Todo se explica paso a paso.

### ¿Puedo volver atrás si algo falla?

Sí. Cada fase tiene un punto de rollback documentado. Puedes volver al estado anterior si es necesario.

### ¿Cuánto tiempo toma la migración completa?

**Estimación realista:**
- Phase 1-5: 15-20 horas (impresora funcional básica)
- Phase 1-11: 35-40 horas (sistema completo)
- Distribuido en 5-7 días con sesiones de 4-6 horas

### ¿Qué pasa con mi impresora actual mientras migro?

Durante Phase 1-4 tu impresora estará desmontada. Planifica tener la impresora inoperativa durante 2-3 días mínimo.

### ¿Esta guía sirve para otras impresoras?

Parcialmente. Los principios aplican a cualquier impresora, pero:
- Pines específicos son para SKR 1.4 Turbo
- Cinemática configurada para CoreXY (Tronxy X5SA)
- Dimensiones específicas de la X5SA

---

## 📞 Soporte y Contribuciones

**Repositorio GitHub:** https://github.com/mjcuadrado/x5sa-skr-klipper

**Si encuentras errores:**
- Abre un Issue en GitHub
- Incluye fotos y descripción detallada

**Si quieres contribuir:**
- Fork del repositorio
- Mejoras documentadas con fotos
- Pull Request con descripción clara

---

## 📜 Licencia

Este proyecto está bajo licencia MIT. Úsalo, compártelo, mejóralo libremente.

---

## 🚀 Empezar Ahora

**¿Listo para empezar?**

➡️ [Phase 1, Step 1: SKR Stock](guides/phase1/step1_skr_stock.md)

O si ya completaste el Step 1:

➡️ [Phase 1, Step 2: Jumpers UART](guides/phase1/step2_uart_jumpers.md)

---

**Última actualización:** 2025-12-20
**Versión:** 1.0
**Estado:** En construcción activa
