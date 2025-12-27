# STLs Phase 12 - Voron Stealthburner Toolhead

**Propósito:** Almacenar todos los STLs necesarios para migrar a Voron Stealthburner en Phase 12
**Estado:** Preparación durante Phase 3 (imprimiendo piezas gradualmente)

---

## 📁 Estructura de Carpetas

```
stls/phase12/
├── official_voron/      # STLs oficiales de VoronDesign
├── orbiter_mounts/      # Mounts para Orbiter 2.5
├── eddy_mounts/         # Mounts para BTT Eddy Coil
├── ebb42_mounts/        # Mounts para EBB42 CAN board
├── x_carriage/          # X carriage según tipo de guías lineales
├── misc/                # Piezas misceláneas y opcionales
├── PRINT_QUEUE_PHASE12.md  # ⭐ Lista completa de impresión
└── README.md            # Este archivo
```

---

## 📋 Guía de Uso

### **Paso 1: Lee PRINT_QUEUE_PHASE12.md**

Este archivo contiene:
- ✅ Lista completa de piezas a imprimir
- ✅ Fuentes de cada STL con enlaces directos
- ✅ Configuración de impresión recomendada (ABS/ASA, 40% infill)
- ✅ Orden de impresión sugerido por prioridad
- ✅ Estimación de tiempos de impresión
- ✅ Checklist pre-descarga de STLs

### **Paso 2: Responde el Checklist**

**Necesitamos saber:**
1. ¿Qué guías lineales tienes? (MGN12 / MGN9 / Otro)
2. ¿Qué variant de Dragonfly BMO? (Std / HF / UHF)
3. ¿Quieres LEDs Neopixel? (Sí / No)
4. ¿Qué mount prefieres para Eddy? (Ver opciones en PRINT_QUEUE)
5. ¿Qué mount prefieres para EBB42? (Ver opciones en PRINT_QUEUE)

### **Paso 3: Descarga STLs Específicos**

Una vez sepamos tu configuración, descargaremos los archivos exactos necesarios.

### **Paso 4: Organiza en Carpetas**

Coloca cada STL en su carpeta correspondiente según categoría.

### **Paso 5: Empieza a Imprimir**

Durante Phase 3-11, imprime 1-2 piezas entre tus impresiones normales.
No hay prisa - tienes meses para acumular todas las piezas.

---

## 🎯 Filosofía

**Phase 3 (Ahora):**
- Impresora funciona perfectamente con hardware stock
- Vas imprimiendo piezas Voron gradualmente
- Sin urgencia, sin presión

**Phase 12 (Futuro):**
- Instalas el Voron Stealthburner con todas las piezas listas
- Migración rápida porque ya tienes todo impreso

---

## 🔗 Enlaces Útiles

**Official Voron:**
- [Voron Stealthburner GitHub](https://github.com/VoronDesign/Voron-Stealthburner)
- [Voron Trident GitHub](https://github.com/VoronDesign/Voron-Trident)
- [Voron 2.4 GitHub](https://github.com/VoronDesign/Voron-2)

**Mounts Community:**
- [StealthOrbiter GitHub](https://github.com/sneakytreesnake/StealthOrbiter)
- [Printables - Voron Mods](https://www.printables.com/search/models?q=voron+stealthburner)
- [BTT Eddy GitHub](https://github.com/bigtreetech/Eddy)

---

## ⚠️ Importante

**Material:** Solo ABS o ASA (NO PLA)
**Enclosure:** Requerido para imprimir ABS/ASA sin warping
**Infill:** 40% recomendado para piezas estructurales

---

**Para más detalles, ver:**
- [PRINT_QUEUE_PHASE12.md](PRINT_QUEUE_PHASE12.md) - Lista completa de impresión
- [../guides/phase3/ARQUITECTURA_PHASE3.md](../guides/phase3/ARQUITECTURA_PHASE3.md) - Arquitectura del proyecto
