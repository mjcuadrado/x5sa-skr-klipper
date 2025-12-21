# Phase 2, Step 3: Montaje SKR en Posición Superior

**Estado:** ✅ Completado (2025-12-21)
**Tiempo estimado:** 1-2 horas
**Dificultad:** Media

---

## 🎯 Objetivo

Montar la BTT SKR 1.4 Turbo en la posición superior del frame (donde estaba la subplaca de distribución stock), optimizando el alcance de cables y preparando la arquitectura CAN.

---

## 🤔 Decisión Arquitectónica Crítica

### Posiciones Evaluadas

Durante la planificación surgió una decisión crítica sobre dónde montar la SKR:

**Opción A: Compartimento inferior (con PSU)**
- ❌ Cables de motores superiores (X, Y) NO llegan
- ❌ Necesitaría extensiones para múltiples cables
- ❌ Gestión de cables compleja

**Opción B: Posición superior (donde estaba distribution board) ✅**
- ✅ Motores X, Y, Z1 alcanzan directamente
- ✅ Solo necesita 1 extensión: Motor Z2
- ✅ Alimentación 24V: extensión desde PSU
- ✅ Cable CAN al toolhead: distancia mínima
- ✅ Posición óptima para arquitectura CAN

**Decisión:** Montar SKR en posición superior

**Frase clave del usuario:**
> "Ahora entiendo el por qué montaban la placa arriba. Creo que es lo más sensato montarla donde estaba el distribuidor ya que en esa posición solo no llega uno de los cables z. El resto llega"

---

## 🧰 Material Necesario

### Hardware
- [x] SKR 1.4 Turbo (con TMC2209 ya instalados)
- [x] **Bridas (zip ties) - montaje temporal**
- [x] Tijeras para cortar bridas

### Cables a Fabricar
- [x] **Extensión Motor Z2:** JST-XH 4-pin macho + 60cm cable + JST-XH 4-pin hembra
- [x] **Extensión alimentación 24V:** 50cm cable azul (2 conductores, 1.5mm²)

### Futuro (cuando impresora funcione)
- [ ] Case impreso para SKR (ver `stls/upgrades/README.md`)
- [ ] Ventilador 40mm (12V o 24V)
- [ ] Tornillos M3 para case definitivo

---

## 🔧 Procedimiento

### Paso 1: Preparación Espacio Superior

**Ubicación:**
- Frame superior, perfil de aluminio 2020
- Zona donde estaba montada la subplaca de distribución stock

**Acciones:**
1. Verificar que la subplaca stock está desmontada (Step 2)
2. Limpiar la zona de restos de cinta/adhesivos
3. Verificar acceso a la zona para cables

**Estado:** ✅ Espacio preparado

---

### Paso 2: Fabricación Cable Extensión Motor Z2

**Necesidad:**
- Motor Z2 (leadscrew derecho) tiene cable de 6 pines stock
- SKR requiere 4 pines
- Cable original NO llega desde motor hasta posición superior SKR

**Solución:**
Fabricar cable extensión intermedio:
- **Conector hembra JST-XH 4-pin:** Conecta al cable original del motor
- **60cm cable 4 conductores:** Longitud suficiente
- **Conector macho JST-XH 4-pin:** Conecta a la SKR

**Esquema de pines (motor stepper estándar):**
```
Pin 1 (motor): Bobina A1  → Cable 1 → SKR
Pin 2 (motor): Bobina A2  → Cable 2 → SKR
Pin 3 (motor): Bobina B1  → Cable 3 → SKR
Pin 4 (motor): Bobina B2  → Cable 4 → SKR
(Pines 5-6 del conector 6-pin original no se usan)
```

**Colores utilizados:**
*(Documentar según lo fabricado - típicamente: Negro, Verde, Rojo, Azul)*

**Ventajas de esta solución:**
- ✅ NO cortar cable original del motor (reversible)
- ✅ Extensión reutilizable
- ✅ Conectores estándar JST-XH

**Estado:** ✅ Cable fabricado y probado

**Foto:** `photos/phase2/32_motor_z2_extension_cable.jpg`

---

### Paso 3: Preparación Cable Alimentación 24V

**Necesidad:**
- PSU (P360W24V) está en compartimento inferior
- SKR está en posición superior
- Distancia estimada: 40-50cm

**Solución:**
- **Cable:** 50cm, 2 conductores, 1.5mm² (azul)
- **Identificación polaridad:** Termorretráctil de colores en extremos
  - Termorretráctil ROJO → +24V
  - Termorretráctil AZUL → GND

**Conexiones:**
- Extremo inferior: Ya conectado a PSU (terminales +24V y GND)
- Extremo superior: Listo para conectar a SKR DCIN

**Estado:** ✅ Cable preparado y conectado a PSU

---

### Paso 4: Montaje Temporal SKR con Bridas

**Método de montaje:**
- **Temporal:** Bridas (zip ties) al perfil 2020
- **Futuro:** Case impreso con ventilador 40mm

**Procedimiento:**
1. Posicionar SKR en zona superior, orientación:
   - Conectores de motores accesibles
   - Puerto USB accesible (para futuras actualizaciones firmware)
   - DCIN accesible
   - Espacio para gestión de cables
2. Pasar 2 bridas por orificios de montaje de la SKR
3. Ajustar las bridas al perfil 2020
4. Apretar firmemente pero sin exceso (no quebrar la PCB)
5. Cortar sobrante de bridas

**Verificación:**
- [ ] SKR firme, no se mueve
- [ ] No hay presión excesiva en PCB
- [ ] Todos los conectores accesibles
- [ ] Espacio suficiente para cables

**Estado:** ✅ SKR montada temporalmente con bridas

**Notas:**
- Montaje temporal suficiente para toda la fase de configuración
- Case definitivo se imprimirá cuando la impresora esté funcional (Phase 5+)
- Ver `stls/upgrades/README.md` para STL del case

---

## 📊 Análisis de Alcance de Cables

**Desde posición superior SKR:**

| Componente | Alcance Cable Stock | Solución |
|------------|---------------------|----------|
| Motor X (CoreXY) | ✅ Llega | Directo |
| Motor Y (CoreXY) | ✅ Llega | Directo |
| Motor Z1 (leadscrew izq) | ✅ Llega | Directo |
| Motor Z2 (leadscrew der) | ❌ NO llega | ✅ Extensión 60cm fabricada |
| Alimentación 24V | ❌ NO llega desde PSU | ✅ Extensión 50cm fabricada |
| Cama caliente | ✅ Llega | Directo |
| Toolhead → EBB42 | ⏸️ Futuro | Cable CAN 4 hilos (Phase 3) |

**Resultado:**
- Solo 2 extensiones necesarias (Z2 + 24V)
- Arquitectura limpia y profesional

---

## 🎯 Ventajas de Esta Configuración

1. **Mínimas extensiones:** Solo Z2 y alimentación
2. **Preparado para CAN:** Toolhead cerca de SKR
3. **Accesibilidad:** Fácil acceso para mantenimiento
4. **Gestión térmica:** Zona bien ventilada (futuro fan 40mm)
5. **Reversibilidad:** Montaje temporal permite ajustes

---

## 📸 Fotos

**Relacionadas con este paso:**
- `photos/phase2/32_motor_z2_extension_cable.jpg` - Cable extensión Z2 fabricado

**Fotos de cableado (siguiente step):**
- `photos/phase2/33_skr_dcin_power_connector.jpg`
- `photos/phase2/34_motors_connected_to_skr.jpg`
- `photos/phase2/35_heated_bed_power_hb.jpg`
- `photos/phase2/36_heated_bed_thermistor_tb.jpg`

---

## 🔧 Troubleshooting

### Problema: Bridas no sujetan bien al perfil 2020

**Solución:**
- Usar bridas más largas
- Pasar las bridas por las ranuras del perfil 2020
- Alternativamente: usar tuercas T-slot + tornillos M3 si disponibles

### Problema: Cable extensión Z2 no hace contacto

**Solución:**
- Verificar crimpado de pines JST-XH
- Probar continuidad con multímetro
- Verificar que el orden de pines es correcto (1-4 en ambos lados)

### Problema: No tengo bridas suficientes

**Solución:**
- Mínimo: 2 bridas (suficiente para sujetar temporalmente)
- Alternativa: cinta americana/gaffer resistente (menos recomendado)

---

## ✅ Checklist de Verificación

Antes de continuar al siguiente paso:

- [x] SKR montada firmemente en posición superior
- [x] Cable extensión Motor Z2 fabricado y probado
- [x] Cable extensión 24V preparado (conectado a PSU)
- [x] Todos los conectores de SKR accesibles
- [x] No hay tensión mecánica en la PCB
- [x] Espacio preparado para gestión de cables

---

## 📚 Lección Aprendida

**Cita del usuario:**
> "Ahora entiendo el por qué montaban la placa arriba"

**Lección:**
Antes de cablear, **siempre evaluar alcance de cables** y posición óptima de controladora. Montar la placa donde llegan más cables nativamente ahorra trabajo y mejora la fiabilidad.

---

## ➡️ Siguiente Paso

SKR montada y lista para cablear. Continuar con:

**[Phase 2, Step 4: Cableado Básico SKR](step4_skr_basic_wiring.md)**

---

**Completado:** 2025-12-21
**Tiempo real empleado:** ~1.5 horas (incluye fabricación extensiones)
**Incidencias:** Ninguna
