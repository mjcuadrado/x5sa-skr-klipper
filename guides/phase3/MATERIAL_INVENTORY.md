# Inventario de Material - Phase 3: Toolhead EBB42 (Modo USB)

**Fecha:** 2025-12-21
**Modo seleccionado:** USB (no CAN bus)
**Estado:** 🔄 Verificación en curso

---

## 🎯 Objetivo del Inventario

Verificar que disponemos de **TODO** el material necesario antes de iniciar Phase 3. Esta fase integrará la placa EBB42 en el toolhead usando comunicación USB.

---

## ✅ Material Confirmado Disponible

### Hardware Principal

- [x] **BTT EBB42 CAN V1.2** (la usaremos en modo USB, no CAN)
- [x] **Omron TL-Q5MC1-Z** - Sensor inductivo de alta precisión para Z probing
- [x] **PT100 sensor** (1.5m, Y-terminal) - Sensor de temperatura de alta precisión
- [x] **MiniPC con Debian + Klipper** - Host ya configurado y funcional

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

**Ventiladores del toolhead (decisión pendiente: stock vs upgrade):**

- [ ] **Part cooling fan** (ventilador de capa)
  - Stock: Verificar estado y specs
  - Upgrade: ¿5015 blower? ¿Noctua?
  - **ACCIÓN:** Decidir si usamos stock o upgrade

- [ ] **Hotend fan** (ventilador de disipación)
  - Stock: Verificar estado y specs
  - Upgrade: ¿Noctua 40mm silencioso?
  - **ACCIÓN:** Decidir si usamos stock o upgrade

- [ ] **EBB42 cooling** (ventilador opcional para la placa)
  - ¿Necesario? (probablemente no)
  - Si se añade: 30mm o 40mm

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

- [ ] **Motor extrusor:**
  - Tipo: NEMA17? ¿Reductor?
  - Conector: JST-XH 4-pin
  - Cable: ¿Longitud actual?
  - **Destino:** Se conectará a EBB42 puerto E0

- [ ] **Hotend:**
  - Tipo: ¿E3D clone? ¿Stock Tronxy?
  - Cartucho calefactor: ¿24V? ¿Potencia?
  - Dimensiones: ¿6x20mm estándar?
  - **Destino:** Se conectará a EBB42 puerto HE0

- [ ] **Termistor hotend:**
  - Tipo actual: ¿100K NTC?
  - Conector: ¿2-pin?
  - **Decisión pendiente:** Usar stock o cambiar a PT100

- [ ] **Ventilador part cooling:**
  - Tensión: ¿24V?
  - Tamaño: ¿5015 blower?
  - Conector: ¿2-pin?
  - **Destino:** Se conectará a EBB42 puerto FAN

- [ ] **Ventilador hotend:**
  - Tensión: ¿24V?
  - Tamaño: ¿40mm?
  - Conector: ¿2-pin?
  - **Destino:** Se conectará a EBB42 puerto FAN (siempre ON)

- [ ] **Sensor Z actual:**
  - ¿Tiene sensor inductivo stock?
  - ¿Endstop mecánico?
  - **Acción:** Será reemplazado por Omron TL-Q5MC1-Z

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
| Ventiladores | ⚠️ Decidir | **Stock vs upgrade** |
| Tornillería | ⚠️ Verificar | **Medir puntos montaje** |
| Toolhead stock | 📋 Pendiente | **DOCUMENTAR CON FOTOS** |

---

## ⚠️ Decisiones Pendientes

Estas decisiones bloquean el inicio de Phase 3:

### 1. Termistor Stock vs PT100 Directo

**Opciones:**
- **A)** Usar termistor stock inicialmente, PT100 en Phase 8
- **B)** Instalar PT100 directamente en Phase 3

**Ventajas A:**
- Menos cambios simultáneos
- Validar EBB42 primero con hardware conocido
- PT100 requiere configuración específica

**Ventajas B:**
- Una sola intervención en hotend
- PT100 más preciso desde inicio
- Evitar reconfiguración posterior

**Recomendación pendiente:** ¿Qué prefiere el usuario?

### 2. Ventiladores Stock vs Upgrade

**Opciones:**
- **A)** Usar ventiladores stock en Phase 3, upgrade en Phase 12
- **B)** Instalar ventiladores silenciosos (Noctua) directamente

**Ventajas A:**
- Menos variables en Phase 3
- Validar sistema básico primero
- Upgrade ventiladores es fácil después

**Ventajas B:**
- Impresora silenciosa desde inicio
- Una sola intervención

**Recomendación pendiente:** ¿Qué prefiere el usuario?

### 3. Estrategia de Trabajo Toolhead

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

---

## 🎯 Próximos Pasos

**Antes de iniciar Phase 3:**

1. **Documentar toolhead stock** (fotos completas)
2. **Medir longitudes** de cables USB y 24V necesarias
3. **Identificar cable USB** específico del inventario
4. **Verificar cable 24V** (sección 1.5mm²)
5. **Decidir:** Termistor vs PT100
6. **Decidir:** Ventiladores stock vs upgrade
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

---

**Documento creado:** 2025-12-21
**Estado:** En verificación
**Acción requerida:** Completar checklist de material y decisiones pendientes
