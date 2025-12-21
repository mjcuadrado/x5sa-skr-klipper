# Phase 3 - Materials Checklist

**Versión:** 1.0
**Fecha:** 2025-12-21

---

## 🎯 Propósito

Esta lista te permite verificar rápidamente qué materiales necesitas para completar la integración de la EBB42.

**Antes de empezar, imprime esta página y marca cada item conforme lo tengas listo.**

---

## ✅ Hardware Principal

### Electrónica
- [ ] **BTT EBB42 CAN V1.2** (controladora toolhead)
- [ ] **Sensor Omron TL-Q5MC1-Z** (ya instalado en impresora)

### Componentes Stock (ya instalados)
- [x] Hotend + bloque calefactor
- [x] Thermistor NTC 100K (en hotend)
- [x] Ventilador hotend (típico 4010 24V)
- [x] Ventilador part cooling (típico 4010 o 5015 24V)
- [x] Motor extrusor (se queda en SKR E0 hasta Phase 12)

---

## 🔌 Cables y Conectores

### Cables Principales (para fabricar)

#### Cable 1: USB (SKR ↔ EBB42)
- [ ] **Cable USB-C a USB-C** (o USB-A según puerto SKR)
  - Longitud: 1.5-2 metros
  - Calidad: datos (no solo carga)
- [ ] **2x Anillos ferrita** (clip-on, diámetro ~5-8mm)

#### Cable 2: 24V Power (SKR ↔ EBB42)
- [ ] **Cable 2 conductores 1.5mm²** (~2 metros)
  - Rojo/negro o similar
- [ ] **Termorretráctil rojo** (4-5mm diámetro, ~10cm)
- [ ] **Termorretráctil azul** (4-5mm diámetro, ~10cm)
- [ ] **JST-XH 2-pin conector macho** (1x, para extremo EBB42)
- [ ] **JST-XH 2-pin conector hembra** (1x, opcional)
- [ ] **2x Pines JST-XH hembra** (para crimpar)

---

### Conectores para Componentes Stock

Necesitas adaptar/agregar conectores a los cables existentes:

#### Para Hotend Heater
- [ ] **JST-XH 2-pin macho** (1x)
- [ ] **JST-XH 2-pin hembra** (1x)
- [ ] **4x Pines JST-XH** (2 macho + 2 hembra)

#### Para Thermistor
- [ ] **JST-XH 2-pin macho** (1x)
- [ ] **JST-XH 2-pin hembra** (1x)
- [ ] **4x Pines JST-XH** (2 macho + 2 hembra)

#### Para Probe Omron
- [ ] **JST-XH 3-pin macho** (1x)
- [ ] **JST-XH 3-pin hembra** (1x)
- [ ] **6x Pines JST-XH** (3 macho + 3 hembra)

#### Para Ventiladores (x2)
- [ ] **Dupont 2-pin macho** (2x sets, uno por ventilador)
- [ ] **Dupont 2-pin hembra** (2x sets)
- [ ] **8x Pines Dupont** (4 macho + 4 hembra)

---

### Resumen Conectores (compra rápida)

| Tipo | Pines | Macho | Hembra | Pines sueltos |
|------|-------|-------|--------|---------------|
| JST-XH | 2-pin | 4 | 4 | 16 |
| JST-XH | 3-pin | 1 | 1 | 6 |
| Dupont | 2-pin | 2 | 2 | 8 |

**Recomendación:** Compra kits surtidos de JST-XH y Dupont (más económico que piezas sueltas).

---

## 🛠️ Herramientas

### Esenciales
- [ ] **Multímetro digital**
  - Medir voltajes, continuidad, resistencias
  - CRÍTICO para seguridad

- [ ] **Crimpadora JST-XH/Dupont**
  - Tipo Engineer PA-09 o similar
  - Alternativa: soldador + estaño

- [ ] **Pelacables / Wire Strippers**
  - Ajustable 0.5-2.5mm²

- [ ] **Destornilladores:**
  - [ ] Phillips #2 (tornillos impresora)
  - [ ] Plano pequeño (terminales tornillo)
  - [ ] Allen 1.5mm, 2mm, 2.5mm (tornillos M3)

- [ ] **Tijeras / Cutter**
  - Cortar cables, termorretráctil

- [ ] **Pistola de calor** o **Encendedor**
  - Contraer termorretráctil

### Recomendadas
- [ ] **Soldador + estaño** (backup si falla crimpado)
- [ ] **Lupa / Gafas magnificadoras** (crimpado preciso)
- [ ] **Pinzas de punta fina** (manipular pines pequeños)
- [ ] **Alicates de corte** (cables gruesos)

---

## 🧰 Accesorios y Consumibles

### Para Montaje y Cableado
- [ ] **Cinta doble cara resistente** (~10cm)
  - Montaje temporal EBB42 en toolhead
  - Tipo 3M VHB o similar

- [ ] **Bridas / Zip ties** (surtido)
  - [ ] 10x pequeñas (~100mm)
  - [ ] 5x medianas (~150mm)
  - Colores: negro o transparente

- [ ] **Velcro adhesivo** (opcional)
  - Gestión de cables en cable chain
  - ~20-30cm tira

### Para Seguridad de Conexiones
- [ ] **Hot glue / Silicona caliente**
  - Asegurar conectores Dupont
  - Pistola silicona + 2-3 barras

- [ ] **Cinta Kapton** (opcional, alternativa a hot glue)
  - 10-15mm ancho, ~1 metro

### Para Identificación
- [ ] **Etiquetas adhesivas** o **Cinta de papel**
  - Identificar cables durante desmontaje

- [ ] **Marcador permanente** (punta fina)
  - Escribir en etiquetas

---

## 📦 Material Adicional (Backup)

Recomendado tener a mano por si falla algo:

- [ ] **Cable extra 2x1.5mm²** (~1 metro adicional)
- [ ] **Conectores JST-XH extra** (2-3 de cada tipo)
- [ ] **Pines JST-XH sueltos** (10-20 extras)
- [ ] **Termorretráctil surtido** (varios diámetros y colores)
- [ ] **Bridas extra** (10-20 unidades)

---

## 🖥️ Software y Firmware

### Pre-descarga
- [ ] **Klipper firmware para STM32G0B1** (EBB42)
  - Compilado con configuración USB (no CAN)
  - Archivo `.bin` listo para flashear

- [ ] **Configuración Klipper ejemplo** para EBB42
  - Ver `EBB42_INTEGRATION.md` sección "Configuración Klipper"

### Herramientas PC/Raspberry Pi
- [ ] **SSH access** a Raspberry Pi (si usas Klipper remoto)
- [ ] **Editor de texto** (nano, vim, VS Code con SSH)
- [ ] **Browser** para interfaz Klipper (Mainsail/Fluidd)

---

## 📸 Documentación

- [ ] **Cámara / Smartphone** (fotos ANTES y DESPUÉS)
- [ ] **Laptop/tablet** (consultar guías durante trabajo)
- [ ] **Cuaderno / Papel** (notas rápidas)

---

## ⚡ Seguridad

- [ ] **Gafas de seguridad** (cortar cables, soldar)
- [ ] **Guantes** (manipular hotend, evitar cortes)
- [ ] **Extintor** accesible (trabajo con electrónica 24V)
- [ ] **Ventilación adecuada** (si se suelda)

---

## 📋 Checklist Pre-Inicio

Antes de empezar Phase 3, verifica:

### Documentación Lista
- [ ] Leído completamente `EBB42_INTEGRATION.md`
- [ ] Impreso `MATERIALS_CHECKLIST.md` (esta página)
- [ ] Firmware EBB42 compilado y descargado

### Hardware Verificado
- [ ] EBB42 flasheada con Klipper (test previo)
- [ ] EBB42 detectada por PC: `ls /dev/serial/by-id/`
- [ ] Sensor Omron accesible y funcional (test con multímetro)

### Materiales Completos
- [ ] Todos los items marcados en esta lista
- [ ] Área de trabajo limpia y organizada
- [ ] Buena iluminación
- [ ] Tiempo disponible: 4-6 horas sin interrupciones

### Backup y Rollback
- [ ] Fotos del toolhead stock (10+ fotos desde varios ángulos)
- [ ] Backup de `printer.cfg` actual
- [ ] Placa stock accesible (para rollback si falla)

---

## 🛒 Lista de Compra Rápida

Si necesitas comprar materiales, aquí tienes una lista consolidada:

### Electrónica
1. BTT EBB42 CAN V1.2
2. Cable USB-C a USB-C (2m, datos)
3. 2x Anillos ferrita

### Cables
4. Cable 2x1.5mm² rojo/negro (2-3m)
5. Kit termorretráctil surtido (incluir rojo/azul)

### Conectores
6. Kit JST-XH 2-pin (10x sets macho+hembra + pines)
7. Kit JST-XH 3-pin (5x sets macho+hembra + pines)
8. Kit Dupont 2-pin (10x sets macho+hembra + pines)

### Herramientas (si no tienes)
9. Crimpadora JST-XH/Dupont (Engineer PA-09 o similar)
10. Multímetro digital
11. Pelacables ajustable
12. Pistola silicona caliente + barras

### Accesorios
13. Cinta doble cara 3M VHB (1 tira)
14. Bridas surtido (50 unidades, varios tamaños)
15. Etiquetas adhesivas / cinta papel

**Costo estimado total:** €40-60 (asumiendo que ya tienes herramientas básicas)

---

## 📝 Notas Adicionales

### Dónde Comprar (España/Europa)
- **Electrónica:** Aliexpress, Amazon, Biqu3D oficial
- **Cables/Conectores:** Aliexpress, Amazon (buscar "JST-XH kit")
- **Herramientas:** Amazon, Leroy Merlin, ferreterías locales
- **Consumibles:** Todo a 100, Amazon, Leroy Merlin

### Tiempos de Entrega
- **Aliexpress:** 2-4 semanas
- **Amazon Prime:** 1-2 días
- **Tiendas locales:** Inmediato

**Recomendación:** Si no tienes todo, pide con antelación para no detener el proyecto.

---

## ✅ Confirmación Final

**ANTES de empezar Phase 3, confirma:**

- [ ] ✅ Tengo TODOS los materiales de esta lista
- [ ] ✅ Tengo TODAS las herramientas necesarias
- [ ] ✅ He leído `EBB42_INTEGRATION.md` completo
- [ ] ✅ Tengo 4-6 horas disponibles sin interrupciones
- [ ] ✅ He hecho backup de configuración actual
- [ ] ✅ He fotografiado el toolhead stock

**Si alguno de estos items NO está marcado, NO empieces todavía.**

---

**Preparado por:** mjcuadrado + Claude Code
**Versión:** 1.0
**Fecha:** 2025-12-21

**Siguiente paso:** Leer `EBB42_INTEGRATION.md` → Fase 1: Preparación
