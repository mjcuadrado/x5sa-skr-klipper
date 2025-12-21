# Inventario de Material - Phase 3: Toolhead EBB42 (Modo USB)

**Fecha:** 2025-12-21
**Modo seleccionado:** USB (no CAN bus)
**Estado:** 🔄 Verificación en curso

---

## 🎯 Objetivo del Inventario

Verificar que disponemos de **TODO** el material necesario antes de iniciar Phase 3. Esta fase migrará todos los componentes **STOCK** del toolhead actual a la nueva placa EBB42 usando comunicación USB.

**Filosofía Phase 3-5:** Base funcional con hardware stock
- Migrar componentes stock existentes a EBB42
- Sistema funcional que permite imprimir mejoras
- Hardware premium (PT100, Orbiter, Dragonfly) se usará en Phase 12 con Stealthburner completo

---

## ✅ Material Confirmado Disponible

### Hardware Principal

- [x] **BTT EBB42 CAN V1.2** (la usaremos en modo USB, no CAN)
- [x] **Omron TL-Q5MC1-Z** - Sensor inductivo de alta precisión para Z probing (instalar si soporte disponible)
- [x] **MiniPC con Debian + Klipper** - Host ya configurado y funcional

### Hardware Premium (Phase 12, NO usar en Phase 3)

- [x] **PT100 sensor** (1.5m, Y-terminal) - Para Phase 12 con Stealthburner
- [x] **Orbiter 2.0/2.5** - Para Phase 12 con Stealthburner
- [x] **Dragonfly BMO** - Para Phase 12 con Stealthburner
- [x] **Ventiladores premium** - Para Phase 12 con Stealthburner

### Material USB

- [x] **Switch USB** (disponible)
- [x] **Cable alargador USB** (disponible)
- [x] **Cable corto USB** (disponible)

---

## 📋 Material a Verificar

### Cables USB (PENDIENTE MEDICIÓN)

**Cable USB para toolhead:**

- [ ] **Tipo de conector USB:**
  - EBB42 V1.2 usa: ¿USB-C? ¿Micro-USB? (verificar en placa)
  - Host (MiniPC): Probablemente USB-A
  - Cable necesario: USB-A (host) a ??? (EBB42)

- [ ] **Longitud necesaria:**
  - Estimar distancia desde toolhead (posición extrema) hasta MiniPC
  - Añadir holgura para movimientos (~50cm extra)
  - Estimación inicial: **1.5-2 metros**
  - **ACCIÓN:** Medir con cinta métrica ruta real del cable

- [ ] **Calidad del cable:**
  - Preferible: USB 2.0 blindado
  - Ferritas en extremos (recomendado para EMI)
  - Flexible (para movimiento del toolhead)

**Material USB del inventario:**
- [ ] Identificar qué cable del inventario es más adecuado
- [ ] Verificar longitud real de cada cable
- [ ] Verificar tipo de conectores de cada cable

### Cables Alimentación (PENDIENTE MEDICIÓN)

**Cable 24V + GND para toolhead:**

- [ ] **Sección del cable:**
  - Recomendado: 1.5mm² (AWG 16) mínimo
  - Verificar si tenemos cable de esta sección disponible
  - Longitud estimada: **1.5-2 metros**

- [ ] **Conectores:**
  - Desde SKR: ¿Terminal de tornillo? ¿Conector específico?
  - Hacia EBB42: ¿Terminal de tornillo? ¿Conector específico?
  - **ACCIÓN:** Verificar conectores de alimentación en EBB42

### Ventiladores

**Decisión:** ✅ **Usar ventiladores stock en Phase 3**

**Ventiladores del toolhead stock:**

- [ ] **Part cooling fan** (ventilador de capa)
  - Usar ventilador stock actual
  - Verificar voltaje (12V o 24V)
  - Verificar funcionamiento correcto
  - Upgrade se hará en Phase 12 con Stealthburner

- [ ] **Hotend fan** (ventilador de disipación)
  - Usar ventilador stock actual
  - Verificar voltaje (12V o 24V)
  - Verificar funcionamiento correcto
  - Upgrade se hará en Phase 12 con Stealthburner

### Tornillería y Fijación

- [ ] **Tornillos para montar EBB42 en toolhead**
  - Verificar sistema de montaje actual del toolhead
  - ¿Necesitamos imprimir soporte custom?
  - ¿Tornillos M3? ¿M2.5?
  - **ACCIÓN:** Medir puntos de fijación disponibles

- [ ] **Bridas/velcro** para gestión de cables en toolhead
- [ ] **Termorretráctil** para identificación de cables

### Conectores

- [ ] **Conectores JST-XH** (si necesarios para adaptaciones)
- [ ] **Ferritas** para cable USB (recomendado)

---

## 🔍 Inventario Toolhead Stock (PENDIENTE DOCUMENTACIÓN)

Antes de desmontar nada, documentar **TODO** lo que hay actualmente en el toolhead:

### Componentes Actuales a Identificar

- [ ] **Motor extrusor stock:**
  - Tipo: NEMA17 (identificar si tiene reductor)
  - Conector: Identificar tipo
  - Cable: Medir longitud actual
  - **Destino:** Migrar a EBB42 puerto E0

- [ ] **Hotend stock:**
  - Tipo: Identificar (E3D clone / Stock Tronxy)
  - Cartucho calefactor: Verificar voltaje y potencia
  - Dimensiones: Medir (probablemente 6x20mm estándar)
  - **Destino:** Migrar a EBB42 puerto HE0

- [ ] **Termistor stock:**
  - Tipo actual: Identificar (probablemente 100K NTC)
  - Conector: Identificar tipo
  - Verificar resistencia con multímetro
  - **Destino:** ✅ Migrar a EBB42 puerto TH0 (PT100 en Phase 12)

- [ ] **Ventilador part cooling stock:**
  - Tensión: Verificar con multímetro (12V o 24V)
  - Tamaño: Medir
  - Conector: Identificar tipo
  - **Destino:** ✅ Migrar a EBB42 puerto FAN1

- [ ] **Ventilador hotend stock:**
  - Tensión: Verificar con multímetro (12V o 24V)
  - Tamaño: Medir
  - Conector: Identificar tipo
  - **Destino:** ✅ Migrar a EBB42 puerto FAN0 (siempre ON)

- [ ] **Sensor Z actual:**
  - Identificar si tiene sensor inductivo stock
  - Identificar si tiene endstop mecánico
  - **Acción:** Será reemplazado por Omron TL-Q5MC1-Z (o usar sensorless temporal)

- [ ] **Cableado actual:**
  - ¿Cuántos cables llegan al toolhead?
  - ¿Hay cable multicore o cables individuales?
  - ¿Cable chain existente?

**ACCIÓN CRÍTICA:** Tomar fotos detalladas de TODO el toolhead stock ANTES de tocar nada.

---

## 📸 Fotos a Tomar (Antes de Phase 3)

Documentación fotográfica del toolhead stock:

- [ ] **Vista general toolhead** (4 ángulos: frontal, posterior, izquierda, derecha)
- [ ] **Motor extrusor** (montaje y conector)
- [ ] **Hotend completo** (identificar tipo)
- [ ] **Cartucho calefactor** (dimensiones, conexión)
- [ ] **Termistor** (tipo, ubicación, conexión)
- [ ] **Ventilador part cooling** (montaje, conexión)
- [ ] **Ventilador hotend** (montaje, conexión)
- [ ] **Sensor Z actual** (si existe)
- [ ] **Cableado completo** (ruta desde toolhead hasta electrónica)
- [ ] **Cable chain** (si existe, cómo está montado)
- [ ] **Puntos de montaje** (tornillos, donde podríamos fijar EBB42)

---

## 🔧 Herramientas Necesarias

- [x] Destornilladores (Phillips, plano)
- [x] Llaves Allen (juego completo)
- [x] Pinzas de punta fina
- [x] Cortaalambres/pelacables
- [x] Multímetro digital
- [ ] **Cinta métrica** (para medir longitudes de cable necesarias)
- [ ] **Calibre** (para medir tornillos, si es necesario)
- [ ] Crimpadora JST (si vamos a fabricar cables custom)
- [ ] Soldador (solo si absolutamente necesario)

---

## 📊 Resumen Estado del Inventario

| Categoría | Estado | Acción Necesaria |
|-----------|--------|------------------|
| EBB42 CAN V1.2 | ✅ Disponible | Ninguna |
| Omron TL-Q5MC1-Z | ✅ Disponible | Ninguna |
| PT100 sensor | ✅ Disponible | Ninguna |
| MiniPC + Klipper | ✅ Configurado | Ninguna |
| Material USB (genérico) | ✅ Disponible | **Identificar cable específico** |
| Cable USB toolhead | ⚠️ Verificar | **Medir longitud necesaria** |
| Cable 24V toolhead | ⚠️ Verificar | **Verificar sección y longitud** |
| Ventiladores stock | ✅ Usar stock | **Verificar voltaje y funcionamiento** |
| Tornillería | ⚠️ Verificar | **Medir puntos montaje** |
| Toolhead stock | 📋 Pendiente | **DOCUMENTAR CON FOTOS** |

---

## ✅ Decisiones Tomadas - Filosofía Hardware Stock

**Enfoque Phase 3-5:** Base funcional con componentes stock existentes
- ✅ **Termistor:** Usar stock (PT100 en Phase 12 con Stealthburner)
- ✅ **Ventiladores:** Usar stock (upgrade en Phase 12 con Stealthburner)
- ✅ **Calentador:** Usar stock (nuevo en Phase 12 con Dragonfly BMO)
- ✅ **Motor extrusor:** Usar stock (Orbiter en Phase 12 con Stealthburner)

**Beneficio:** Sistema funcional rápido que permite imprimir mejoras para Phase 12

**Phase 12:** Toolhead completo nuevo (Stealthburner + Orbiter + Dragonfly BMO + PT100 + ventiladores premium)

## ⚠️ Decisiones Pendientes

Estas decisiones bloquean el inicio de Phase 3:

### 1. Estrategia de Trabajo Toolhead

**Opciones:**
- **A)** Desmontar toolhead completamente, trabajar en mesa
- **B)** Trabajar in-situ con impresora

**Ventajas A:**
- Más cómodo y seguro
- Mejor iluminación y acceso
- Fotos más claras

**Ventajas B:**
- No perder ajustes mecánicos
- Verificar longitudes de cable en posición real

**Recomendación:** Probablemente **A** (desmontar) para documentación profesional

### 2. Sensor Omron - Instalación

**Opciones:**
- **A)** Instalar Omron en Phase 3 (si tienes soporte montaje)
- **B)** Usar sensorless Z temporal, instalar Omron en Phase 6-7 después de imprimir soporte

**Ventaja B:** Permite imprimir soporte custom con la propia impresora

**Recomendación:** Decidir según disponibilidad de soporte

---

## 🎯 Próximos Pasos

**Antes de iniciar Phase 3:**

1. **Documentar toolhead stock** (fotos completas, identificar componentes)
2. **Medir longitudes** de cables USB y 24V necesarias
3. **Identificar cable USB** específico del inventario
4. **Verificar cable 24V** (sección 1.5mm²)
5. **Decidir:** Estrategia trabajo (mesa vs in-situ)
6. **Decidir:** Sensor Omron (ahora vs después con sensorless)
7. **Verificar tornillería** para montaje EBB42
8. **Crear lista de compras** si falta algo

**Una vez inventario completo:**

→ Proceder a **Phase 3, Step 1: Documentación Toolhead Stock**

---

## 📝 Notas

- **Arquitectura USB confirmada:** No necesitamos Cat6 ni resistencias 120Ω
- **Material CAN descartado:** Cable twisted pair no necesario
- **Focus en USB de calidad:** Blindado, con ferritas si es posible
- **MiniPC ya funcional:** No requiere configuración adicional de host
- **Klipper multi-MCU:** Ya soporta SKR + EBB42 simultáneamente
- **Filosofía hardware stock:** PT100, Orbiter, Dragonfly BMO se guardan para Phase 12 (Stealthburner completo)
- **Objetivo Phase 3-5:** Sistema funcional que permite imprimir mejoras

---

**Documento creado:** 2025-12-21
**Estado:** En verificación
**Acción requerida:** Completar checklist de material y decisiones pendientes
