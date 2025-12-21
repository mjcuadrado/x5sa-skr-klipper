# Phase 2, Step 2: Desconexión Electrónica Stock

**Estado:** ✅ Completado (2025-12-21)
**Tiempo estimado:** 1 hora
**Dificultad:** Baja

---

## 🎯 Objetivo

Desconectar completamente la electrónica stock de la impresora de forma ordenada y segura, preparando la instalación de la SKR 1.4 Turbo.

---

## ⚠️ Seguridad

- [x] **Impresora completamente apagada**
- [x] **Cable de alimentación desconectado de la pared**
- [x] **Esperar 5 minutos** para descarga de condensadores
- [x] **Verificar con multímetro** que no hay tensión (opcional pero recomendado)

---

## 📋 Material Necesario

- [ ] Destornilladores (Phillips, plano)
- [ ] Bolsas/cajas para organizar cables
- [ ] Etiquetas o cinta adhesiva para marcar cables (opcional)
- [ ] Cámara/móvil para fotos de respaldo (además de las del repositorio)

---

## 🔌 Procedimiento de Desconexión

### Paso 1: Cable Ribbon de Distribución

**Qué desconectar:**
- Cable ribbon (cinta plana) entre placa principal y subplaca de distribución

**Procedimiento:**
1. Localizar el conector del ribbon cable en la subplaca de distribución
2. Presionar suavemente los clips laterales del conector
3. Extraer el ribbon cable tirando recto (no en ángulo)
4. Guardar el cable en bolsa etiquetada: "STOCK - Ribbon cable"

**Estado:** ✅ Desconectado

---

### Paso 2: Cables de Subplaca de Distribución

**Qué desconectar:**
- Todos los cables conectados a la subplaca de distribución superior
- Típicamente: motores, endstops, toolhead (calentador, termistor, ventiladores)

**Procedimiento:**
1. Fotografiar cada conector ANTES de desconectar (por si acaso)
2. Desconectar uno a uno, memorizando o etiquetando origen
3. Dejar los cables colgando desde sus componentes (NO cortar)
4. Guardar la subplaca en bolsa etiquetada: "STOCK - Distribution board"

**Estado:** ✅ Todos los cables desconectados de subplaca

---

### Paso 3: Motores desde Placa Principal

**Qué desconectar:**
- Motor Y (CoreXY)
- Motor Z1 (leadscrew izquierdo)
- Motor Z2 (leadscrew derecho)

**Notas:**
- Motor X (CoreXY) probablemente ya desconectado con la subplaca
- Los conectores de motores suelen tener clips de seguridad - presionar antes de tirar

**Procedimiento:**
1. Localizar conectores de motores en placa principal
2. Presionar clips de seguridad si los tienen
3. Desconectar tirando recto
4. Dejar cables con los motores

**Estado:** ✅ Motores Y, Z1, Z2 desconectados

---

### Paso 4: Cama Caliente

**Qué desconectar:**
- Cables de alimentación cama (gruesos, potencia)
- Cable termistor cama (fino, etiquetado "B TEMP")

**Procedimiento:**
1. **Termistor primero** (es más delicado):
   - Localizar conector pequeño etiquetado "B TEMP" o "TH1"
   - Desconectar suavemente
2. **Alimentación después** (cables gruesos):
   - Aflojar tornillos del terminal de tornillo
   - Extraer cables
   - Verificar que no quedan hilos sueltos de cobre

**Estado:** ✅ Cama caliente desconectada (power + termistor)

---

### Paso 5: Alimentación Principal

**Qué desconectar:**
- Cables desde fuente de alimentación (P360W24V) a placa principal

**Procedimiento:**
1. Localizar terminal de alimentación en placa principal (24V, GND)
2. Aflojar tornillos del terminal de tornillo
3. Extraer cables ROJO (+24V) y NEGRO (GND)
4. **NO desconectar** los cables de la fuente de alimentación (quedan ahí)

**Estado:** ✅ Alimentación desconectada de placa

---

### Paso 6: Extracción Física de Placas Stock

**Qué extraer:**
- Placa principal (board inferior)
- Subplaca de distribución (board superior)

**Procedimiento:**
1. Aflojar tornillos/soportes que fijan la placa principal
2. Extraer placa principal con cuidado
3. Extraer subplaca de distribución (si aún montada)
4. Guardar ambas placas en bolsa antiestática etiquetada: "STOCK - Main + Distribution boards"

**Almacenamiento:**
- Guardar en lugar seco
- Proteger de golpes
- **NO tirar** - son backup de emergencia si algo falla

**Estado:** ✅ Placas stock extraídas y almacenadas

---

## ✅ Checklist de Verificación

Antes de continuar al siguiente paso, verificar:

- [x] Toda la electrónica stock desconectada
- [x] Cables de motores, cama, toolhead libres y accesibles
- [x] Fuente de alimentación (PSU) sigue montada en compartimento inferior
- [x] Cables 24V de PSU accesibles para conexión a SKR
- [x] Placas stock almacenadas de forma segura
- [x] Espacio libre para montaje de SKR en zona superior

---

## 📸 Fotos de Referencia

**Nota:** Esta fase se realizó de forma fluida y rápida. Las fotos del estado stock están documentadas en Step 1. El proceso de desconexión fue directo siguiendo el checklist anterior.

---

## 🔧 Troubleshooting

### Problema: Conector atascado, no sale

**Solución:**
- NO forzar
- Verificar que no hay clips de seguridad
- Presionar suavemente mientras se mueve ligeramente de lado a lado
- Si sigue atascado, fotografiar y revisar tipo de conector

### Problema: Cable cortado accidentalmente

**Solución:**
- Evaluar longitud restante
- Si es suficiente, pelar y reconectar con conector nuevo
- Si es muy corto, fabricar extensión (como hicimos con Motor Z2)

### Problema: No sé dónde iba conectado un cable

**Solución:**
- Revisar fotos de Step 1 (documentación stock)
- Normalmente los cables tienen etiquetas o marcas
- Código de colores suele ser estándar (rojo=+, negro=-, amarillo/blanco=señal)

---

## 📊 Estado de Componentes tras Desconexión

| Componente | Estado | Ubicación |
|------------|--------|-----------|
| Motores (X, Y, Z1, Z2) | Montados en impresora, cables libres | En sus posiciones |
| Cama caliente | Montada, cables libres | En posición |
| Toolhead | Montado, cables libres | En posición |
| PSU P360W24V | Montada y funcional | Compartimento inferior |
| Placa principal stock | Desmontada | Almacenada |
| Subplaca distribución | Desmontada | Almacenada |
| Endstops | Montados (pero NO se usarán) | En posiciones X/Y/Z |

---

## ➡️ Siguiente Paso

Una vez completada la desconexión, continuar con:

**[Phase 2, Step 3: Montaje SKR Posición Superior](step3_skr_mounting.md)**

---

**Completado:** 2025-12-21
**Tiempo real empleado:** ~45 minutos
**Incidencias:** Ninguna
