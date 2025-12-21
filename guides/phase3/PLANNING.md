# Phase 3: Toolhead EBB42 - Planificación

**Estado:** 📋 En planificación
**Fecha:** 2025-12-21
**Modo comunicación:** ✅ **USB** (no CAN bus)

---

## 🎯 Objetivo de Phase 3

Instalar la placa EBB42 CAN V1.2 en el toolhead, conectar todos los componentes del extrusor/hotend a ella, y establecer comunicación **USB** con el MiniPC (host Klipper).

**Resultado esperado:**
- EBB42 montada físicamente en toolhead
- Todos los componentes toolhead conectados a EBB42
- Cable USB tendido y conectado al MiniPC
- Cable alimentación 24V desde SKR a EBB42
- Sistema listo para firmware (Phase 4)

---

## 📦 Inventario Disponible

### Hardware confirmado:
- ✅ BTT EBB42 CAN V1.2 (usaremos en **modo USB**, no CAN)
- ✅ Sensor Omron TL-Q5MC1-Z (probe Z)
- ✅ PT100 sensor + cartucho
- ✅ Material USB (switch, cable alargador, cable corto) - **verificar tipo y longitud**
- ✅ Cable alimentación 24V (para toolhead)
- ✅ MiniPC con Debian + Klipper (host)
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

### 6️⃣ Calentador Hotend

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

### 7️⃣ Sensor Z (Omron)

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
- Seleccionar cable USB y planificar ruta
- Planificar cable 24V alimentación

### Step 3: Preparación Hardware
- Seleccionar cable USB adecuado del inventario
- Preparar cable alimentación 24V + GND
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

### Hardware
- [ ] EBB42 CAN V1.2 (modo USB)
- [ ] Sensor Omron TL-Q5MC1-Z
- [ ] PT100 (si se decide instalar ahora)
- [ ] Soporte EBB42 (temporal o impreso)
- [ ] MiniPC con Klipper (ya disponible)

---

## ⏱️ Estimación Temporal

**Según decisiones:**
- Opción rápida (temporal, termistor stock, USB inventario): ~3-4 horas
- Opción completa (definitivo, PT100, Omron, cable custom): ~4-6 horas

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

### Riesgo 3: Termistor/PT100 no lee correctamente
- **Mitigación:** Verificar con multímetro, probar termistor stock primero

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

**Pendiente decisiones del usuario:**
1. Montaje EBB42 (temporal/impreso/adaptado)
2. Termistor stock vs PT100 directo
3. Cable USB: tipo conector, longitud, del inventario o nuevo, ruta
4. Cable 24V: longitud, origen en SKR, destino en EBB42
5. Ventiladores (stock/cambiar)
6. Calentador (stock/nuevo)
7. Sensor Omron (ahora/después)
8. Estrategia trabajo (completo/in-situ)

**Discutir antes de proceder.**
