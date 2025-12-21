# Phase 3: Toolhead EBB42 - Planificación

**Estado:** 📋 En planificación
**Fecha:** 2025-12-21
**Modo comunicación:** ✅ **USB** (no CAN bus)

---

## 🎯 Objetivo de Phase 3

Migrar todos los componentes **STOCK** del toolhead actual a la nueva placa EBB42 CAN V1.2, estableciendo comunicación **USB** con el MiniPC (host Klipper).

**Filosofía Phase 3-5:** Impresora funcional básica con hardware stock
- Usar TODO el hardware existente: motor extrusor stock, termistor stock, ventiladores stock, calentador stock
- ÚNICA excepción: Sensor Z Omron (mejora clara y definitiva)
- Objetivo: Sistema funcional que permita imprimir mejoras para Phase 12

**Phase 12 (futuro):** Toolhead completo nuevo
- Stealthburner + Orbiter 2.0/2.5 + Dragonfly BMO
- AHÍ sí: PT100, ventiladores premium, todo el hardware nuevo

**Resultado esperado Phase 3:**
- EBB42 montada físicamente en toolhead
- Todos los componentes stock migrados a EBB42
- Cable USB tendido y conectado al MiniPC
- Cable alimentación 24V desde SKR a EBB42
- Sistema listo para firmware (Phase 4)

---

## 📦 Inventario Disponible

### Hardware confirmado:
- ✅ BTT EBB42 CAN V1.2 (usaremos en **modo USB**, no CAN)
- ✅ Sensor Omron TL-Q5MC1-Z (probe Z - instalar en Phase 3)
- ✅ Material USB (switch, cable alargador, cable corto) - **verificar tipo y longitud**
- ✅ Cable alimentación 24V (para toolhead)
- ✅ MiniPC con Debian + Klipper (host)
- ✅ Toolhead stock actual (motor extrusor, hotend, ventiladores)

### Hardware toolhead stock (USAR en Phase 3):
- Motor extrusor (NEMA17) - migrar a EBB42
- Hotend con calentador stock - migrar a EBB42
- Termistor stock (¿tipo?) - migrar a EBB42
- Ventilador hotend (cooling) - migrar a EBB42
- Ventilador part cooling (capa) - migrar a EBB42
- Cables actuales

### Hardware premium (NO usar hasta Phase 12):
- PT100 sensor + cartucho - guardar para Phase 12 (Stealthburner)
- Orbiter 2.0/2.5 - guardar para Phase 12
- Dragonfly BMO - guardar para Phase 12
- Ventiladores nuevos - guardar para Phase 12

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

**Decisión:** ✅ **Usar termistor stock**

**Razón:** Filosofía Phase 3-5 = hardware stock funcional
- Termistor stock ya instalado y funcionando
- PT100 se instalará en Phase 12 con toolhead completo nuevo (Stealthburner)
- Evita complejidad innecesaria en migración inicial

**Información a documentar:**
- [ ] Tipo de termistor stock (probablemente 100K NTC)
- [ ] Verificar funcionamiento con multímetro

---

### 3️⃣ Cable USB - Selección

**Problema:** Seleccionar cable USB adecuado desde MiniPC hasta toolhead

**Arquitectura USB:**
- EBB42 USB → MiniPC (host Klipper)
- SKR USB → MiniPC (host Klipper)
- Ambas placas aparecen como MCUs independientes en Klipper

**Decisiones pendientes:**

**Tipo de conector USB:**
- EBB42 V1.2: ¿USB-C? ¿Micro-USB? (**verificar en placa**)
- MiniPC: Probablemente USB-A
- Cable necesario: USB-A (host) a ??? (EBB42)

**Longitud del cable:**
- Distancia MiniPC → Toolhead (posición más alejada)
- Estimación: ¿1.5m? ¿2m?
- **ACCIÓN:** Medir con cinta métrica

**Calidad del cable:**
- Preferible: USB 2.0 blindado
- Ferritas en extremos (protección EMI)
- Cable flexible (para movimiento toolhead)

**Material disponible:**
- Switch USB (¿para qué exactamente?)
- Cable alargador USB (¿longitud? ¿tipo conectores?)
- Cable corto USB (¿longitud? ¿tipo conectores?)
- **ACCIÓN:** Inventariar cada cable USB disponible

**Ruta del cable:**
- ¿Usa cable chain existente?
- ¿Cable suelto por fuera?
- ¿Necesita guías/clips impresos?

**¿Decisiones?**
- [ ] Conector EBB42: USB-C / Micro-USB (verificar)
- [ ] Longitud necesaria: ______ metros (medir)
- [ ] Cable del inventario a usar: ______
- [ ] Ferritas: Sí / No / Comprar
- [ ] Ruta: Cable chain / Suelto / Otro: ______

---

### 4️⃣ Cable Alimentación 24V - Preparación

**Problema:** Llevar alimentación 24V desde SKR hasta EBB42 en toolhead

**Arquitectura:**
- En modo USB, la alimentación es **independiente** de la comunicación
- Cable USB: solo comunicación (no alimenta la EBB42)
- Cable 24V + GND: desde SKR hasta EBB42

**Especificaciones:**
- Sección mínima: **1.5mm²** (AWG 16)
- 2 conductores: 24V (positivo) + GND (negativo)
- Termorretráctil para identificación (rojo/azul)

**Decisiones pendientes:**

**Longitud del cable:**
- Distancia SKR (arriba frame) → Toolhead (posición más alejada)
- Estimación: ¿1.5m? ¿2m?
- **ACCIÓN:** Medir

**Origen alimentación en SKR:**
- ¿Terminal de tornillo auxiliar?
- ¿Derivar del DCIN?
- ¿Puerto específico para periféricos?
- **ACCIÓN:** Verificar esquema SKR 1.4 Turbo

**Conexión en EBB42:**
- ¿Terminal de tornillo VIN?
- ¿Conector específico?
- **ACCIÓN:** Verificar documentación EBB42

**Cable disponible:**
- ¿Tenemos cable 1.5mm² suficiente?
- ¿Qué longitud disponible?

**¿Decisiones?**
- [ ] Longitud: ______ metros (medir)
- [ ] Origen SKR: ______
- [ ] Destino EBB42: ______
- [ ] Cable disponible: Sí / No / Comprar
- [ ] Colores: Rojo (+24V) / Azul (GND) / Otros: ______

---

### 5️⃣ Ventiladores

**Decisión:** ✅ **Usar ventiladores stock**

**Razón:** Filosofía Phase 3-5 = hardware stock funcional
- Hotend cooling fan stock - migrar a EBB42
- Part cooling fan stock - migrar a EBB42
- Ventiladores premium se instalarán en Phase 12 con Stealthburner

**Información a documentar:**
- [ ] Voltaje ventiladores stock (12V o 24V)
- [ ] Verificar funcionamiento correcto
- [ ] Identificar tipo de conector

---

### 6️⃣ Calentador Hotend

**Decisión:** ✅ **Usar cartucho calentador stock**

**Razón:** Filosofía Phase 3-5 = hardware stock funcional
- Cartucho stock ya instalado y funcionando
- Cartucho nuevo se instalará en Phase 12 con Dragonfly BMO

**Información a documentar:**
- [ ] Potencia cartucho stock (típicamente 40W en 24V)
- [ ] Dimensiones (probablemente 6x20mm estándar)
- [ ] Verificar resistencia con multímetro

---

### 7️⃣ Sensor Z (Omron)

**Problema:** ¿Montar sensor Omron TL-Q5MC1-Z ahora o después?

**Opciones:**

**A) Montar Omron en Phase 3**
- ✅ Solución definitiva desde inicio
- ✅ No usar endstop Z temporal
- ✅ Mejora clara justificada (precisión, fiabilidad)
- ⚠️ Requiere soporte impreso/adaptado
- ⚠️ Más pasos en Phase 3

**B) Sensor Z temporal (sensorless TMC2209) → Omron en Phase 6-7**
- ✅ Más rápido Phase 3
- ✅ Omron en fase posterior con calma
- ⚠️ Sensorless Z menos fiable (pero funcional)
- ✅ Permite imprimir soporte Omron con la propia impresora

**Recomendación:** Decidir según disponibilidad de soporte montaje
- Si ya tienes soporte: Opción A
- Si no tienes soporte: Opción B (usar sensorless, imprimir soporte, instalar Omron después)

**Información necesaria:**
- [ ] ¿Tienes soporte para montar Omron en toolhead?
- [ ] ¿Necesita diseño custom o hay modelo Tronxy X5SA disponible?

---

### 8️⃣ Orden de Trabajo

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
4. Tender cables USB + 24V
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
- Seleccionar cable USB y planificar ruta
- Planificar cable 24V alimentación
- Decidir instalación sensor Omron (ahora vs después)

### Step 3: Preparación Hardware
- Seleccionar cable USB adecuado del inventario
- Preparar cable alimentación 24V + GND
- Preparar soporte EBB42 (temporal o definitivo)
- Verificar conectores y herramientas

### Step 4: Montaje EBB42
- Instalar EBB42 en toolhead (según decisión)
- Verificar espacio y acceso
- Asegurar firmemente

### Step 5: Migración Componentes Stock
- Motor extrusor stock: Stock → EBB42 E0
- Calentador hotend stock: Stock → EBB42 HE
- Termistor stock: Stock → EBB42 TH0
- Ventilador hotend stock: Stock → EBB42 FAN0
- Ventilador part cooling stock: Stock → EBB42 FAN1
- Sensor Omron (si disponible): Instalar y conectar a EBB42 PROBE

### Step 6: Cableado USB + Alimentación
- Tender cable USB desde MiniPC a toolhead
- Conectar USB a EBB42
- Tender cable 24V desde SKR a toolhead
- Conectar alimentación a EBB42 (VIN + GND)
- Verificar continuidad y polaridad

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
- [ ] Cable USB (del inventario, verificar tipo/longitud)
- [ ] Cable alimentación 24V 1.5mm² (~2m estimado)
- [ ] Termorretráctil (rojo/azul para 24V)
- [ ] Conectores (según decisión)
- [ ] Bridas/velcro
- [ ] (Opcional) Ferritas para USB
- [ ] (Opcional) Soldadura + soldador

### Hardware Phase 3
- [ ] EBB42 CAN V1.2 (modo USB)
- [ ] Sensor Omron TL-Q5MC1-Z (si soporte disponible)
- [ ] Soporte EBB42 (temporal o impreso)
- [ ] MiniPC con Klipper (ya disponible)
- [ ] Componentes stock toolhead (motor, termistor, ventiladores, calentador)

### Hardware Phase 12 (NO usar en Phase 3)
- PT100 sensor + cartucho - guardar
- Orbiter 2.0/2.5 - guardar
- Dragonfly BMO - guardar
- Ventiladores premium - guardar
- Stealthburner toolhead - guardar

---

## ⏱️ Estimación Temporal

**Según decisiones:**
- Opción rápida (temporal, componentes stock, USB inventario, sensorless Z): ~3-4 horas
- Opción completa (definitivo, componentes stock, Omron, cable custom): ~4-6 horas

**Distribución:**
- Documentación toolhead stock: 30-45 min
- Selección/preparación cables: 30-45 min (USB del inventario + 24V)
- Montaje EBB42: 30-60 min
- Migración componentes: 1.5-2h
- Instalación cables USB + 24V: 45-60 min
- Verificación: 30-45 min

---

## 📸 Fotos Obligatorias

**Antes:**
- [ ] Toolhead stock completo (4-6 ángulos)
- [ ] Cables actuales identificados
- [ ] Conexiones actuales detalle

**Durante:**
- [ ] EBB42 soporte/montaje
- [ ] Cable USB seleccionado
- [ ] Cable 24V preparado
- [ ] Cada componente conectado a EBB42

**Después:**
- [ ] Toolhead con EBB42 completo
- [ ] Cable USB tendido y conectado
- [ ] Cable 24V tendido y conectado
- [ ] Vista general sistema

**Estimación:** 15-20 fotos mínimo

---

## 🚨 Riesgos y Mitigaciones

### Riesgo 1: EBB42 no cabe en toolhead
- **Mitigación:** Medir espacio ANTES, diseño adaptado

### Riesgo 2: Cable USB demasiado corto o tipo incorrecto
- **Mitigación:** Verificar tipo conector EBB42, medir con margen, confirmar longitud antes de tender

### Riesgo 3: Termistor stock no lee correctamente
- **Mitigación:** Verificar resistencia con multímetro antes de conectar, documentar tipo exacto

### Riesgo 4: Ventiladores voltaje incorrecto
- **Mitigación:** Medir con multímetro ANTES de conectar

### Riesgo 5: Cable 24V polaridad incorrecta
- **Mitigación:** Termorretráctil de colores (rojo +24V, azul GND), verificar con multímetro ANTES de conectar

### Riesgo 6: USB no comunica o interferencias EMI
- **Mitigación:** Cable USB de calidad blindado, ferritas si es necesario, verificar en `ls /dev/serial/by-id/`

---

## ✅ Criterios de Éxito Phase 3

- [ ] EBB42 montada firmemente en toolhead
- [ ] Todos los componentes conectados a EBB42
- [ ] Cable USB tendido y conectado (MiniPC → EBB42)
- [ ] Cable 24V tendido y conectado (SKR → EBB42)
- [ ] Verificación física completa (sin cortocircuitos)
- [ ] Polaridad 24V verificada con multímetro
- [ ] Cables con holgura para movimientos
- [ ] Documentación fotográfica completa
- [ ] Sistema listo para firmware USB (Phase 4)

---

## ➡️ Siguiente Paso

**Una vez tomadas las decisiones, crear:**
- `guides/phase3/step1_stock_toolhead_documentation.md`
- Y subsiguientes steps según decisiones

---

**Decisiones tomadas (filosofía hardware stock):**
1. ✅ Termistor: Usar stock (PT100 en Phase 12)
2. ✅ Ventiladores: Usar stock (premium en Phase 12)
3. ✅ Calentador: Usar stock (nuevo en Phase 12)

**Pendiente decisiones del usuario:**
1. Montaje EBB42 (temporal/impreso/adaptado)
2. Cable USB: tipo conector, longitud, del inventario o nuevo, ruta
3. Cable 24V: longitud, origen en SKR, destino en EBB42
4. Sensor Omron (ahora si tienes soporte / después con sensorless)
5. Estrategia trabajo (completo/in-situ)

**Discutir antes de proceder.**
