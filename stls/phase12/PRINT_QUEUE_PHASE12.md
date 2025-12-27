# Print Queue Phase 12 - Voron Stealthburner Toolhead
**Objetivo:** Imprimir todas las piezas necesarias para Phase 12 **DURANTE Phase 3**
**Estado actual:** Phase 3 (hardware stock funcionando)
**Instalación Voron:** Phase 12 (futuro)

---

## 🎯 Filosofía

**Durante Phase 3 (ahora):**
- ✅ Impresora funciona perfectamente con hardware stock
- ✅ Usamos la impresora para IMPRIMIR piezas del Voron
- ✅ Vamos acumulando piezas mientras imprimimos normalmente
- ✅ Sin prisa, sin urgencia

**En Phase 12 (futuro):**
- 🔮 Instalamos el Voron Stealthburner con todas las piezas ya listas
- 🔮 Orbiter 2.5, Dragonfly BMO, EBB42 integrada
- 🔮 Multicolor ready

---

## ⚠️ INFORMACIÓN NECESARIA DEL USUARIO

**Antes de descargar STLs, necesito saber:**

### **1. ¿Qué tipo de guías lineales tienes en X e Y?**
- [ ] MGN12 (12mm)
- [ ] MGN9 (9mm)
- [ ] Otras: __________

**Esto determina qué X carriage necesitas.**

### **2. ¿Hotend para Phase 12?**
Mencionaste **Dragonfly BMO**. Confirma:
- [ ] Dragonfly BMO (standard flow)
- [ ] Dragonfly BMO HF (high flow)
- [ ] Dragonfly BMO UHF (ultra high flow)

**Esto determina qué printhead frontal/trasero imprimir.**

### **3. ¿Tienes LED Neopixel para el Stealthburner?**
- [ ] Sí, quiero LEDs (imprimir logo con difusor)
- [ ] No, sin LEDs (imprimir logo sólido)

---

## 📦 Bill of Materials - Piezas a Imprimir

### **CATEGORÍA 1: Official Voron Stealthburner (Core)**

**Fuente:** [GitHub - VoronDesign/Voron-Stealthburner](https://github.com/VoronDesign/Voron-Stealthburner)

| # | Pieza | Archivo STL | Cantidad | Material | Color | Prioridad |
|---|-------|-------------|----------|----------|-------|-----------|
| 1.1 | **Main Body** | `[a]_stealthburner_main_body.stl` | 1 | ABS/ASA | Accent | ⭐ Alta |
| 1.2 | **Cowl** (specific to hotend) | Depende de Dragonfly BMO variant | 1 | ABS/ASA | Main | ⭐ Alta |
| 1.3 | **Front** (specific to hotend) | Depende de Dragonfly BMO variant | 1 | ABS/ASA | Main | ⭐ Alta |
| 1.4 | **Rear** (Clockwork2) | `rear_[variant].stl` | 1 | ABS/ASA | Main | ⭐ Alta |
| 1.5 | **Logo / LED Diffuser** | `[o]_stealthburner_LED_diffuser_mask.stl` o sólido | 1 | Transparent/Clear o ABS | Clear/Accent | Media |
| 1.6 | **Cable Cover** | `[a]_cable_cover.stl` (si existe) | 1 | ABS/ASA | Accent | Baja |

**Notas:**
- Main color = Color principal del toolhead (ej. negro)
- Accent color = Color de acento (ej. rojo, azul)
- Transparent = Para LEDs, usar PETG/ABS transparent o white translucent

---

### **CATEGORÍA 2: Orbiter 2.5 Mount**

**Fuente:** [GitHub - sneakytreesnake/StealthOrbiter](https://github.com/sneakytreesnake/StealthOrbiter)

| # | Pieza | Archivo STL | Cantidad | Material | Prioridad |
|---|-------|-------------|----------|----------|-----------|
| 2.1 | **Orbiter Mount** | Depende de repo específico | 1 | ABS/ASA | ⭐ Alta |
| 2.2 | **Spacers / Adapters** | Si requiere (verificar repo) | Varios | ABS/ASA | Media |

**Repositorios recomendados:**
1. [StealthOrbiter by sneakytreesnake](https://github.com/sneakytreesnake/StealthOrbiter) - CAN bus toolhead support
2. [Voron StealthBurner + Orbiter v2.0 (Printables)](https://www.printables.com/model/345237-voron-stealthburner-orbiter-v20)
3. [Lightweight Orbiter2 mount (Printables)](https://www.printables.com/model/404882-lightweight-orbiter2-mount-for-voron-stealthburner)

**⚠️ IMPORTANTE:** Orbiter 2.5 usa el mismo cuerpo que 2.0, así que mounts de "Orbiter 2.0" son compatibles.

---

### **CATEGORÍA 3: BTT Eddy Coil Mount**

**Fuentes múltiples:**

| # | Pieza | Fuente | Cantidad | Material | Prioridad |
|---|-------|--------|----------|----------|-----------|
| 3.1 | **Eddy Mount** | Elegir una de las opciones abajo | 1 | ABS/ASA | ⭐ Alta |

**Opciones de mount (elegir UNA):**

#### **Opción A: BTT Eddy Voron CNC Stealthburner by Drak** (Recomendada)
- **Fuente:** [Printables - BTT Eddy Voron CNC Stealthburner](https://www.printables.com/model/961497-btt-eddy-voron-cnc-stealthburner)
- **Pro:** Ajustable, diseño robusto
- **Última actualización:** Agosto 2024
- **Archivos:** Verificar en Printables si incluye versión para Dragonfly BMO

#### **Opción B: Stealthburner Eddy Adjustable Side Mount**
- **Fuente:** [Printables - Stealthburner Eddy Adjustable](https://www.printables.com/model/1466060-stealthburner-eddy-adjustable-side-mount-works-wit)
- **Pro:** Compatible con UHF hotends, ajustable
- **Altura Eddy sobre nozzle:** Ajustable

#### **Opción C: BTT Eddy Probe Stealthburner Side Mount (TZ/Revo)**
- **Fuente:** [Printables - BTT Eddy Side Mount](https://www.printables.com/model/1254785-btt-eddy-probe-stealthburner-side-mount-for-tz-ver)
- **Pro:** Posiciona Eddy 1.8-2mm sobre nozzle (optimal)
- **Última actualización:** Abril 2025
- **Con:** Específico para TZ/Revo (verificar compatibilidad Dragonfly)

#### **Opción D: Voron 2.4 X Carriage Eddy for StealthBurner HF/UHF**
- **Fuente:** [Printables - X Carriage Eddy HF/UHF](https://www.printables.com/model/1355412-voron-24-x-carriage-eddy-for-stealthburner-hfuhf)
- **Pro:** Diseñado para high flow hotends
- **Eddy altura:** 2.5mm sobre nozzle
- **Última actualización:** Diciembre 2025

**⚠️ DECISIÓN REQUERIDA:** ¿Qué mount prefieres? (Recomiendo Opción A o D según tu Dragonfly variant)

---

### **CATEGORÍA 4: EBB42 Mount en Stealthburner**

| # | Pieza | Fuente | Cantidad | Material | Prioridad |
|---|-------|--------|----------|----------|-----------|
| 4.1 | **EBB42 Bracket** | Elegir una opción abajo | 1 | ABS/ASA | ⭐ Alta |

**Opciones de EBB42 mount:**

#### **Opción A: EBB42 Stealthburner mount & cover**
- **Fuente:** [Printables - EBB42 mount & cover](https://www.printables.com/model/1059359-ebb42-stealthburner-mount-cover)
- **Descripción:** Mount simple + cover para EBB42 en Stealthburner
- **Archivos:** Mount + Cover separados

#### **Opción B: Voron Stealthburner EBB42 holder**
- **Fuente:** [Printables - Voron Stealthburner EBB42](https://www.printables.com/model/1328311-voron-stealthburner-ebb42-holder)
- **Descripción:** Holder específico para EBB42

#### **Opción C: StealthOrbiter (integrado)**
- **Fuente:** [GitHub - StealthOrbiter](https://github.com/sneakytreesnake/StealthOrbiter)
- **Descripción:** Si usas este mount de Orbiter, puede incluir soporte para CAN boards (verificar)

**⚠️ NOTA:** Algunos mounts Orbiter ya incluyen espacio para EBB42. Verificar antes de imprimir separado.

---

### **CATEGORÍA 5: X Carriage (Depende de guías lineales)**

**⚠️ CRÍTICO: Necesito saber qué guías lineales tienes en X/Y antes de especificar STLs.**

#### **Si tienes MGN12:**
- **Fuente oficial:** [Voron Trident X Axis](https://github.com/VoronDesign/Voron-Trident/tree/main/STLs/Gantry/X_Axis)
- **Archivos principales:** X carriage parts para MGN12

#### **Si tienes MGN9 (dual):**
- **Fuente oficial:** [Voron 2.4 X Axis](https://github.com/VoronDesign/Voron-2/tree/Voron2.4/STLs/Gantry/X_Axis)
- **Archivos principales:** X carriage parts para dual MGN9

#### **Si tienes custom/Ender-style:**
- Puede requerir adapters específicos
- **Ejemplo:** [Ender 3 Stealthburner MGN12](https://www.printables.com/model/219196-ender-3-stealthburner-mount-mgn12-linear-rail)

| # | Pieza | Depende de | Cantidad | Material | Prioridad |
|---|-------|------------|----------|----------|-----------|
| 5.1 | **X Carriage Front** | Tipo guía | 1 | ABS/ASA | ⭐ Alta |
| 5.2 | **X Carriage Rear** | Tipo guía | 1 | ABS/ASA | ⭐ Alta |
| 5.3 | **Belt Clamps** | Tipo guía | 2 | ABS/ASA | Alta |
| 5.4 | **Spacers** (si req.) | Tipo guía | Varios | ABS/ASA | Media |

---

### **CATEGORÍA 6: Misc / Opcional**

| # | Pieza | Descripción | Cantidad | Material | Prioridad |
|---|-------|-------------|----------|----------|-----------|
| 6.1 | **Cable Chain Mount** | Si cambias cable management | 1 | ABS/ASA | Baja |
| 6.2 | **Adxl Mount** | Si quieres ADXL en toolhead | 1 | ABS/ASA | Baja |
| 6.3 | **Neopixel Mounts** | Para LEDs adicionales | Variable | ABS/ASA | Opcional |
| 6.4 | **Bowden Clip** (NO!) | NO necesario (direct drive) | 0 | N/A | ❌ No |

---

## 🎨 Configuración de Impresión Recomendada

### **Material: ABS o ASA** (NO PLA - demasiado calor cerca del hotend)

| Parámetro | Valor | Notas |
|-----------|-------|-------|
| **Material** | ABS o ASA | ASA mejor para UV resistance |
| **Layer Height** | 0.2mm | Standard profile de OrcaSlicer |
| **Infill** | 40% | Voron recomienda 40% para structural parts |
| **Perimeters** | 4 | Robustez |
| **Top/Bottom Layers** | 5 | Solidez |
| **Supports** | Según pieza | Muchas piezas Voron NO requieren supports |
| **Bed Temp** | 100-105°C | ABS/ASA |
| **Hotend Temp** | 240-250°C | ABS/ASA |
| **Cooling** | 0-30% | Minimal para ABS/ASA |
| **Enclosure** | ⚠️ REQUERIDO | ABS/ASA necesitan enclosure para evitar warping |

### **Colores Recomendados:**

**Esquema de 2 colores (típico Voron):**
- **Main (negro/gris):** Cowl, front, rear, x carriage
- **Accent (rojo/azul/naranja):** Main body, logo, cable cover

**Esquema 1 color (más simple):**
- Todo negro o todo gris

---

## 📋 Orden de Impresión Sugerido

**Prioridad ALTA (imprimir primero):**
1. ✅ **Main Body** - Core del Stealthburner
2. ✅ **X Carriage (front/rear)** - Montaje en gantry
3. ✅ **Cowl + Front + Rear** (hotend specific) - Soporte hotend
4. ✅ **Orbiter Mount** - Soporte extrusor
5. ✅ **Eddy Mount** - Sensor crítico
6. ✅ **EBB42 Mount** - Electrónica

**Prioridad MEDIA:**
7. ⏳ **Belt Clamps** - Necesario para tensión
8. ⏳ **Cable Cover** - Estética y protección
9. ⏳ **Spacers/Adapters** - Si se requieren

**Prioridad BAJA / Opcional:**
10. 🔹 **Logo/Diffuser** - Estética
11. 🔹 **Neopixel mounts** - Si quieres LEDs
12. 🔹 **Misc decorativos** - Cuando tengas tiempo

---

## ⏱️ Estimación de Tiempo de Impresión

**Con perfil Standard (0.2mm, 40% infill):**

| Categoría | Tiempo Estimado | Notas |
|-----------|----------------|-------|
| Stealthburner Core (1.1-1.6) | ~8-12 horas | 6 piezas |
| Orbiter Mount | ~2-4 horas | 1-2 piezas |
| Eddy Mount | ~1-3 horas | 1 pieza |
| EBB42 Mount | ~1-2 horas | 1-2 piezas |
| X Carriage | ~4-6 horas | 2-4 piezas |
| Misc | ~2-4 horas | Variable |
| **TOTAL** | **~18-31 horas** | Distribuido en semanas/meses |

**Estrategia:**
- No imprimas todo de golpe
- Imprime 1-2 piezas entre tus impresiones normales
- Ve acumulando durante Phase 3-11 (tienes tiempo)

---

## ✅ Checklist Pre-Descarga STLs

**Antes de descargar y empezar a imprimir, responde:**

- [ ] **¿Qué tipo de guías lineales X/Y?** (MGN12 / MGN9 / Otro)
- [ ] **¿Dragonfly BMO variant?** (Standard / HF / UHF)
- [ ] **¿Quieres LEDs Neopixel?** (Sí / No)
- [ ] **¿Qué mount Eddy prefieres?** (Opción A/B/C/D)
- [ ] **¿Qué mount EBB42 prefieres?** (Opción A/B/C)
- [ ] **¿Qué mount Orbiter prefieres?** (StealthOrbiter / Printables / Otro)
- [ ] **¿Tienes ABS/ASA filament?** (Sí / No - si no, comprar primero)
- [ ] **¿Tienes enclosure?** (Sí / No - crítico para ABS/ASA)

**Cuando respondas estas preguntas, podré:**
1. Generar la lista exacta de archivos STL a descargar
2. Crear scripts de descarga automática
3. Organizar en carpetas por categoría
4. Darte la secuencia óptima de impresión

---

## 📁 Estructura de Carpetas STL

```
/Users/mjcuadrado/projects/x5sa-skr-klipper/stls/phase12/
├── official_voron/              # STLs oficiales Voron Stealthburner
│   ├── main_body.stl
│   ├── cowl_dragonfly_bmo.stl
│   ├── front_dragonfly_bmo.stl
│   ├── rear_clockwork2.stl
│   ├── logo_led_diffuser.stl
│   └── cable_cover.stl
├── orbiter_mounts/              # Mounts Orbiter 2.5
│   ├── stealthorbiter_mount.stl
│   └── (otros según repo elegido)
├── eddy_mounts/                 # Mounts Eddy Coil
│   ├── eddy_mount_adjustable.stl
│   └── (según opción elegida)
├── ebb42_mounts/                # Mounts EBB42
│   ├── ebb42_mount.stl
│   ├── ebb42_cover.stl
│   └── (según opción elegida)
├── x_carriage/                  # X carriage según guías lineales
│   ├── x_carriage_front.stl
│   ├── x_carriage_rear.stl
│   ├── belt_clamp_x2.stl
│   └── (según MGN12/MGN9)
├── misc/                        # Piezas misceláneas
│   └── (opcional)
└── PRINT_QUEUE_PHASE12.md      # Este archivo
```

---

## 🔗 Enlaces de Referencia

**Official Voron:**
- [Voron Stealthburner GitHub](https://github.com/VoronDesign/Voron-Stealthburner)
- [Voron Trident GitHub](https://github.com/VoronDesign/Voron-Trident) (MGN12 X axis)
- [Voron 2.4 GitHub](https://github.com/VoronDesign/Voron-2) (MGN9 X axis)

**Orbiter Mounts:**
- [StealthOrbiter by sneakytreesnake](https://github.com/sneakytreesnake/StealthOrbiter)
- [Voron StealthBurner + Orbiter v2.0 (Printables)](https://www.printables.com/model/345237-voron-stealthburner-orbiter-v20)
- [Lightweight Orbiter2 mount (Printables)](https://www.printables.com/model/404882-lightweight-orbiter2-mount-for-voron-stealthburner)

**Eddy Mounts:**
- [BTT Eddy Voron CNC Stealthburner (Printables)](https://www.printables.com/model/961497-btt-eddy-voron-cnc-stealthburner)
- [Stealthburner Eddy Adjustable (Printables)](https://www.printables.com/model/1466060-stealthburner-eddy-adjustable-side-mount-works-wit)
- [BTT Eddy Side Mount TZ/Revo (Printables)](https://www.printables.com/model/1254785-btt-eddy-probe-stealthburner-side-mount-for-tz-ver)
- [X Carriage Eddy HF/UHF (Printables)](https://www.printables.com/model/1355412-voron-24-x-carriage-eddy-for-stealthburner-hfuhf)
- [BTT Eddy Official GitHub](https://github.com/bigtreetech/Eddy) (incluye mounts en /3D folder)

**EBB42 Mounts:**
- [EBB42 mount & cover (Printables)](https://www.printables.com/model/1059359-ebb42-stealthburner-mount-cover)
- [Voron Stealthburner EBB42 holder (Printables)](https://www.printables.com/model/1328311-voron-stealthburner-ebb42-holder)

**Hardware Info:**
- [Orbiter v2.5 Official](https://www.orbiterprojects.com/orbiter-v2-5/)
- [Dragonfly BMO Hotend](https://www.phaetus.com/dragonfly-bmo/) (Phaetus official)

---

## 🚀 Próximos Pasos

1. **Responde el checklist de arriba** con tus especificaciones
2. **Yo creo la lista exacta de STLs** a descargar
3. **Descargas los archivos** y los organizas en carpetas
4. **Empiezas a imprimir** en orden de prioridad (durante Phase 3-11)
5. **Vas acumulando piezas** hasta tener todo listo para Phase 12

**¿Listo para responder el checklist y empezar a buscar los STLs exactos?**

---

**Documentación relacionada:**
- [ARQUITECTURA_PHASE3.md](../../guides/phase3/ARQUITECTURA_PHASE3.md) - Por qué Phase 3 es estable
- [../../HARDWARE_EVOLUTION.md](../../HARDWARE_EVOLUTION.md) - Roadmap completo del proyecto
