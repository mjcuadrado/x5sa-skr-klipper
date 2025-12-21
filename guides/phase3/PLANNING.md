# Phase 3: Toolhead EBB42 CAN - Planificación

**Estado:** 📋 En planificación
**Fecha:** 2025-12-21

---

## 🎯 Objetivo de Phase 3

Instalar la placa EBB42 CAN V1.2 en el toolhead, conectar todos los componentes del extrusor/hotend a ella, y establecer comunicación CAN bus con la SKR 1.4 Turbo.

**Resultado esperado:**
- EBB42 montada físicamente en toolhead
- Todos los componentes toolhead conectados a EBB42
- Cable CAN tendido y conectado
- Sistema listo para firmware (Phase 4)

---

## 📦 Inventario Disponible

### Hardware confirmado:
- ✅ BTT EBB42 CAN V1.2
- ✅ Sensor Omron TL-Q5MC1-Z (probe Z)
- ✅ PT100 sensor + cartucho
- ✅ Cable Cat6 (para CAN bus)
- ✅ Cable alimentación azul (con termorretráctil)
- ✅ Toolhead stock actual (motor extrusor, hotend, ventiladores)

### Hardware toolhead stock:
- Motor extrusor (NEMA17)
- Hotend con calentador
- Termistor stock (¿tipo?)
- Ventilador hotend (cooling)
- Ventilador part cooling (capa)
- Cables actuales

---

## ❓ Decisiones Críticas a Tomar

### 1️⃣ Montaje Físico de EBB42

**Problema:** ¿Cómo montar la EBB42 en el toolhead?

**Opciones:**

**A) Montaje temporal con bridas/velcro**
- ✅ Rápido, no requiere impresora funcional
- ✅ Permite probar antes de solución definitiva
- ⚠️ Menos profesional
- ⚠️ Puede moverse durante impresión

**B) Imprimir soporte dedicado**
- ✅ Solución definitiva y profesional
- ✅ Bien integrado en toolhead
- ❌ Requiere impresora funcional (Catch-22)
- ❌ Puede requerir múltiples iteraciones

**C) Diseño/adaptación existente sin imprimir**
- ✅ Usar soporte metálico/plástico existente
- ⚠️ Puede requerir modificaciones

**¿Qué prefieres?**
- [ ] Opción A - Temporal (bridas/velcro)
- [ ] Opción B - Imprimir soporte (requiere otra impresora)
- [ ] Opción C - Adaptar existente
- [ ] Otra solución: _______________

---

### 2️⃣ Sensor de Temperatura Hotend

**Problema:** ¿Usar termistor stock temporal o instalar PT100 directamente?

**Opciones:**

**A) Termistor stock temporal**
- ✅ Más rápido (ya está instalado)
- ✅ Menos pasos en Phase 3
- ✅ Seguro (ya sabemos que funciona)
- ⚠️ Requiere cambiar a PT100 después (Phase posterior)
- ⚠️ Trabajo duplicado

**B) PT100 directo**
- ✅ Solución definitiva (no repetir trabajo)
- ✅ Alta precisión desde inicio
- ⚠️ Más complejo (verificar cableado MAX31865)
- ⚠️ Puede añadir tiempo a Phase 3
- ⚠️ Si falla, debugging más complejo

**¿Qué prefieres?**
- [ ] Opción A - Termistor stock temporal
- [ ] Opción B - PT100 directo
- [ ] Decidir después de ver complejidad

**Información necesaria:**
- ¿Qué tipo de termistor tiene el stock? (100K NTC típicamente)
- ¿El cartucho PT100 es compatible mecánicamente con el hotend?

---

### 3️⃣ Cable CAN - Fabricación

**Problema:** Fabricar cable CAN de 4 hilos desde SKR hasta toolhead

**Especificaciones decididas:**
- Cat6 (par trenzado) para CAN_H/CAN_L
- Cable alimentación separado (1.5mm²) para 24V/GND
- Termorretráctil para identificación

**Decisiones pendientes:**

**Longitud del cable:**
- Distancia SKR (arriba frame) → Toolhead (posición más alejada)
- Estimación: ¿1.5m? ¿2m?
- Necesitas medir o estimar

**Colores par CAN (Cat6):**
- Convención estándar:
  - Par naranja: Naranja sólido = CAN_H, Naranja/blanco = CAN_L
  - O prefieres: Par azul, par verde, etc.?

**Conectores en extremos:**
- ¿Crimpar Dupont/JST?
- ¿Soldadura directa en placas?
- ¿Conectores XH?

**Ruta del cable:**
- ¿Usa cable chain existente?
- ¿Cable suelto por fuera?
- ¿Necesita guías/clips impresos?

**¿Decisiones?**
- [ ] Longitud: ______ metros
- [ ] Par Cat6: Naranja / Azul / Verde / Otro: ______
- [ ] Conectores extremo SKR: ______
- [ ] Conectores extremo EBB42: ______
- [ ] Ruta: Cable chain / Suelto / Otro: ______

---

### 4️⃣ Ventiladores

**Problema:** ¿Reutilizar ventiladores stock o cambiar?

**Ventiladores stock:**
- Hotend cooling fan (siempre encendido)
- Part cooling fan (controlado, capa)

**Opciones:**

**A) Reutilizar stock**
- ✅ Rápido
- ✅ Sin coste
- ⚠️ Pueden ser ruidosos
- ⚠️ Voltaje? (12V o 24V?)

**B) Cambiar a Noctua u otros silenciosos**
- ✅ Más silencioso
- ✅ Mejor rendimiento potencial
- ❌ Coste adicional
- ❌ Tiempo adicional
- ⚠️ Puede requerir adaptadores voltaje

**¿Qué prefieres?**
- [ ] Opción A - Reutilizar stock
- [ ] Opción B - Cambiar a silenciosos (¿cuáles?)
- [ ] Decidir después

**Información necesaria:**
- ¿Voltaje ventiladores stock? (12V o 24V)
- ¿Funcionan correctamente?

---

### 5️⃣ Calentador Hotend

**Problema:** ¿Reutilizar cartucho calentador stock?

**Opciones:**

**A) Reutilizar cartucho stock**
- ✅ Ya instalado
- ✅ Sin coste
- ⚠️ Potencia? (típicamente 40W en 24V)

**B) Cambiar a cartucho nuevo 6x20mm**
- ✅ Potencia conocida (50W típico)
- ✅ Nuevo, fiable
- ⚠️ Requiere desmontar hotend
- ⚠️ Coste ~5-10€

**¿Qué prefieres?**
- [ ] Opción A - Reutilizar stock
- [ ] Opción B - Cartucho nuevo 6x20mm

---

### 6️⃣ Sensor Z (Omron)

**Problema:** ¿Montar sensor Omron TL-Q5MC1-Z ahora o después?

**Opciones:**

**A) Montar Omron en Phase 3**
- ✅ Solución definitiva desde inicio
- ✅ No usar endstop Z temporal
- ⚠️ Requiere soporte impreso/adaptado
- ⚠️ Más pasos en Phase 3

**B) Sensor Z temporal (endstop mecánico) → Omron después**
- ✅ Más rápido Phase 3
- ✅ Omron en fase posterior con calma
- ⚠️ Trabajo duplicado
- ⚠️ Endstop Z stock ya desconectado

**C) Sensorless Z temporal (TMC2209)**
- ✅ Sin hardware adicional
- ⚠️ Menos fiable en Z (cama pesada)
- ⚠️ No recomendado para Z

**¿Qué prefieres?**
- [ ] Opción A - Omron en Phase 3
- [ ] Opción B - Temporal → Omron después
- [ ] Opción C - Sensorless Z (no recomendado)

**Información necesaria:**
- ¿Tienes soporte para montar Omron en toolhead?
- ¿Necesita diseño custom?

---

### 7️⃣ Orden de Trabajo

**Problema:** ¿Estrategia de desmontaje/montaje?

**Opciones:**

**A) Desmontaje completo toolhead**
1. Documentar toolhead stock
2. Desconectar todos cables
3. Desmontar toolhead completo
4. Montar EBB42 en banco
5. Conectar todo en banco
6. Reinstalar toolhead completo

**Ventajas:** Más cómodo trabajar en banco
**Desventajas:** Más invasivo, riesgo perder posición/ajustes

**B) Trabajo in-situ paso a paso**
1. Documentar toolhead stock
2. Montar EBB42 en toolhead (sin desmontar)
3. Ir cambiando cables uno a uno (stock → EBB42)
4. Tender cable CAN
5. Verificar

**Ventajas:** Menos invasivo, más controlado
**Desventajas:** Menos espacio para trabajar

**¿Qué prefieres?**
- [ ] Opción A - Desmontaje completo
- [ ] Opción B - Trabajo in-situ
- [ ] Híbrido: _______________

---

## 📋 Pasos Tentatativos Phase 3

### Step 1: Documentación Toolhead Stock
- Fotos toolhead completo (múltiples ángulos)
- Identificar todos los cables
- Medir voltajes ventiladores (con multímetro)
- Documentar tipo termistor
- Identificar potencia calentador

### Step 2: Toma de Decisiones
- Revisar todas las decisiones de este documento
- Planificar solución montaje EBB42
- Decidir termistor vs PT100
- Planificar cable CAN

### Step 3: Preparación Hardware
- Fabricar cable CAN (Cat6 + alimentación)
- Preparar soporte EBB42 (temporal o definitivo)
- Verificar conectores y herramientas

### Step 4: Montaje EBB42
- Instalar EBB42 en toolhead (según decisión)
- Verificar espacio y acceso
- Asegurar firmemente

### Step 5: Migración Componentes
- Motor extrusor: Stock → EBB42 E0
- Calentador hotend: Stock → EBB42 HE
- Termistor/PT100: Stock → EBB42 TH0 o PT100
- Ventilador hotend: Stock → EBB42 FAN0
- Ventilador part cooling: Stock → EBB42 FAN1
- Sensor Omron: Instalar y conectar a EBB42 PROBE

### Step 6: Cable CAN
- Tender cable desde SKR a toolhead
- Conectar a SKR (pines CAN)
- Conectar a EBB42 (pines CAN + 24V)
- Verificar continuidad

### Step 7: Verificación Física
- Checklist completo conexiones
- Verificar no hay cortocircuitos
- Verificar cables con holgura (movimientos)
- Fotos finales

---

## 🔧 Material Necesario

### Herramientas
- [ ] Destornilladores (Phillips, plano, Allen)
- [ ] Multímetro (mediciones voltaje/continuidad)
- [ ] Crimpadora (si cables con conectores)
- [ ] Pelacables
- [ ] Tijeras/cutter
- [ ] Bridas/velcro (gestión cables)

### Consumibles
- [ ] Cable Cat6 (~2m estimado)
- [ ] Cable alimentación 1.5mm² (~2m estimado)
- [ ] Termorretráctil (varios colores)
- [ ] Conectores (según decisión)
- [ ] Bridas
- [ ] (Opcional) Soldadura + soldador

### Hardware
- [ ] EBB42 CAN V1.2
- [ ] Sensor Omron TL-Q5MC1-Z
- [ ] PT100 (si se decide instalar ahora)
- [ ] Soporte EBB42 (temporal o impreso)

---

## ⏱️ Estimación Temporal

**Según decisiones:**
- Opción rápida (temporal, termistor stock): ~4 horas
- Opción completa (definitivo, PT100, Omron): ~6 horas

**Distribución:**
- Documentación: 30-45 min
- Fabricación cable CAN: 1-1.5h
- Montaje EBB42: 30-60 min
- Migración componentes: 1.5-2h
- Cable CAN instalación: 45-60 min
- Verificación: 30-45 min

---

## 📸 Fotos Obligatorias

**Antes:**
- [ ] Toolhead stock completo (4-6 ángulos)
- [ ] Cables actuales identificados
- [ ] Conexiones actuales detalle

**Durante:**
- [ ] EBB42 soporte/montaje
- [ ] Cable CAN fabricado
- [ ] Cada componente conectado

**Después:**
- [ ] Toolhead con EBB42 completo
- [ ] Cable CAN tendido
- [ ] Vista general sistema

**Estimación:** 15-20 fotos mínimo

---

## 🚨 Riesgos y Mitigaciones

### Riesgo 1: EBB42 no cabe en toolhead
- **Mitigación:** Medir espacio ANTES, diseño adaptado

### Riesgo 2: Cable CAN demasiado corto
- **Mitigación:** Medir con margen, mejor sobrar que faltar

### Riesgo 3: Termistor/PT100 no lee correctamente
- **Mitigación:** Verificar con multímetro, probar termistor stock primero

### Riesgo 4: Ventiladores voltaje incorrecto
- **Mitigación:** Medir con multímetro ANTES de conectar

### Riesgo 5: CAN bus no comunica
- **Mitigación:** Verificar polaridad, terminación 120Ω, continuidad

---

## ✅ Criterios de Éxito Phase 3

- [ ] EBB42 montada firmemente en toolhead
- [ ] Todos los componentes conectados a EBB42
- [ ] Cable CAN tendido correctamente (4 hilos)
- [ ] Verificación física completa (sin cortocircuitos)
- [ ] Cables con holgura para movimientos
- [ ] Documentación fotográfica completa
- [ ] Sistema listo para firmware (Phase 4)

---

## ➡️ Siguiente Paso

**Una vez tomadas las decisiones, crear:**
- `guides/phase3/step1_stock_toolhead_documentation.md`
- Y subsiguientes steps según decisiones

---

**Pendiente decisiones del usuario:**
1. Montaje EBB42 (temporal/impreso/adaptado)
2. Termistor stock vs PT100 directo
3. Cable CAN: longitud, colores, conectores, ruta
4. Ventiladores (stock/cambiar)
5. Calentador (stock/nuevo)
6. Sensor Omron (ahora/después)
7. Estrategia trabajo (completo/in-situ)

**Discutir antes de proceder.**
