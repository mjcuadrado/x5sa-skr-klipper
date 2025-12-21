# Phase 2, Step 5: Verificación Final Phase 2

**Estado:** ✅ Completado (2025-12-21)
**Tiempo estimado:** 30-45 minutos
**Dificultad:** Baja

---

## 🎯 Objetivo

Realizar una verificación exhaustiva de todo el trabajo de Phase 2 antes de energizar el sistema por primera vez, asegurando que todas las conexiones son correctas y seguras.

---

## 📋 Checklist de Verificación Completa

### Sección 1: Hardware SKR

**Placa SKR 1.4 Turbo:**
- [x] SKR montada firmemente en posición superior
- [x] Montaje temporal con bridas (2+ bridas)
- [x] Placa no se mueve al presionar suavemente
- [x] No hay tensión mecánica en la PCB
- [x] Todos los conectores son accesibles

**Drivers TMC2209:**
- [x] 5 drivers instalados (X, Y, Z, E0, E1)
- [x] Orientación correcta (marca EN coincide con esquina marcada en placa)
- [x] Jumpers UART configurados (MS3 bajo cada socket)
- [x] Disipadores térmicos instalados en todos los drivers
- [x] Drivers insertados completamente (sin pines doblados)

**Referencia:** Ver Phase 1 completa para detalles de drivers

---

### Sección 2: Alimentación

**Cable 24V PSU → SKR:**
- [x] Cable conectado a PSU (terminales +24V y GND)
- [x] Cable con termorretráctil de identificación:
  - ROJO → +24V (centro de barrel jack)
  - AZUL → GND (exterior de barrel jack)
- [x] Cable conectado a DCIN de SKR
- [x] Longitud suficiente (~50cm), sin tensión
- [x] Cable sin daños, aislante íntegro

**PSU (Fuente de alimentación):**
- [x] PSU montada en compartimento inferior
- [x] PSU apagada (interruptor en posición OFF)
- [x] Cable de red AC desconectado de la pared
- [x] Tensión de salida correcta (verificar 24V en placa PSU)

---

### Sección 3: Motores

**Motor X (CoreXY, superior izquierdo):**
- [x] Conectado a puerto X de SKR
- [x] Conector JST-XH insertado completamente (click)
- [x] Cable sin tensión mecánica
- [x] Motor físicamente montado en frame

**Motor Y (CoreXY, superior derecho):**
- [x] Conectado a puerto Y de SKR
- [x] Conector JST-XH insertado completamente (click)
- [x] Cable sin tensión mecánica
- [x] Motor físicamente montado en frame

**Motor Z1 (Leadscrew izquierdo):**
- [x] Conectado a puerto Z de SKR
- [x] Conector JST-XH insertado completamente (click)
- [x] Cable sin tensión mecánica
- [x] Motor acoplado a leadscrew izquierdo

**Motor Z2 (Leadscrew derecho) + Extensión:**
- [x] Cable original motor → extensión hembra JST-XH
- [x] Extensión macho JST-XH → puerto E1 de SKR (NO E0)
- [x] Extensión de 60cm con 4 conductores
- [x] Ambas conexiones insertadas completamente
- [x] Cable de extensión bien gestionado (no enredado)
- [x] Motor acoplado a leadscrew derecho

**Notas importantes motores:**
- ✅ Motor Z2 va a puerto **E1** (segundo Z en configuración dual Z)
- ✅ Puerto E0 queda **libre** (reservado para extrusor en EBB42)

---

### Sección 4: Cama Caliente

**Alimentación Cama (HB):**
- [x] Cables gruesos conectados a terminal HB
- [x] Polaridad correcta (verificar marcas +/-)
- [x] Tornillos del terminal apretados firmemente
- [x] Tirar suavemente: cables NO se salen
- [x] NO hay hilos de cobre sueltos fuera del terminal
- [x] Cable con holgura para permitir movimiento Z

**Termistor Cama (TB):**
- [x] Cable fino conectado a conector TB
- [x] Conector insertado completamente
- [x] Cable delicado protegido (no tenso, no en contacto con partes móviles)
- [x] Etiqueta "B TEMP" visible (opcional pero útil)

---

### Sección 5: Conexiones NO Realizadas (Por Diseño)

**Endstops:**
- [x] Confirmado: NO conectar endstops X, Y a SKR
- [x] Razón: Sensorless homing con TMC2209
- [x] Endstop Z: NO conectar (Omron en EBB42, Phase 3)

**Toolhead (Extrusor):**
- [x] Confirmado: Toolhead completo se conectará en Phase 3
- [x] Componentes pendientes (irán a EBB42 CAN):
  - Motor extrusor (E0)
  - Calentador hotend
  - Termistor hotend
  - Ventiladores (part cooling, hotend)
  - Sensor Z (Omron TL-Q5MC1-Z)

**Cable CAN:**
- [x] Confirmado: Cable CAN se preparará en Phase 3
- [x] Tipo: 4 hilos (CAN_H, CAN_L, 24V, GND)
- [x] Plan: Cat6 para señal + cable alimentación separado

---

### Sección 6: Seguridad Eléctrica

**Inspección visual de cables:**
- [x] NO hay cables pelados/expuestos
- [x] NO hay cables cortados o dañados
- [x] NO hay conexiones sueltas
- [x] NO hay cables tocando partes móviles (correas, leadscrews)
- [x] NO hay cables en zonas de alta temperatura (cerca de cama futura)

**Inspección de cortocircuitos:**
- [x] NO hay contacto entre terminales +24V y GND
- [x] NO hay objetos metálicos sueltos sobre la PCB
- [x] NO hay herramientas olvidadas en la impresora

**Protección:**
- [x] Cables organizados con bridas/velcro
- [x] Holgura suficiente en cables móviles (cama Z)
- [x] Separación señal/potencia (cables HB separados de motores)

---

### Sección 7: Mecánica de la Impresora

**Frame y componentes:**
- [x] Frame estable (verificar tornillos apretados)
- [x] Correas CoreXY tensadas correctamente
- [x] Leadscrews Z1 y Z2 giran libremente (sin atasco)
- [x] Cama se mueve en Z sin obstrucciones
- [x] Ejes X e Y se mueven libremente a mano

**Acoplamientos motores:**
- [x] Motores Z acoplados correctamente a leadscrews
- [x] Poleas CoreXY fijadas a motores X, Y
- [x] NO hay tornillos sueltos en acoplamientos

---

## 🧪 Preparación para Primera Energización

**IMPORTANTE:** NO energizar aún. Esta verificación es previa.

**Cuando llegue el momento (Phase 3), antes de energizar:**

1. **Triple verificación:**
   - Repasar este checklist completo
   - Verificar polaridad 24V (termorretráctil rojo/azul)
   - Verificar que NO hay cortocircuitos

2. **Primera energización controlada:**
   - Conectar cable AC a PSU
   - Encender PSU (interruptor ON)
   - **Observar inmediatamente:**
     - LED de la SKR debe encender (indica 24V correcto)
     - NO debe haber humo
     - NO debe haber olor a quemado
     - NO debe haber chispas
   - Si algo falla: **APAGAR INMEDIATAMENTE**

3. **Verificación con multímetro (recomendado):**
   - Medir tensión en terminales 24V de SKR: debe ser ~24V DC
   - Medir continuidad GND: debe ser continuo
   - Medir que NO hay tensión en carcasa metálica (debe ser 0V)

**Estado:** ⏸️ Verificación completa, energización pendiente Phase 3

---

## 📊 Inventario de Material Utilizado

**Hardware principal:**
- SKR 1.4 Turbo con 5x TMC2209
- Cable extensión Motor Z2 (JST-XH 4-pin, 60cm)
- Cable extensión alimentación 24V (2x1.5mm², 50cm)
- Bridas para montaje temporal SKR
- Termorretráctil rojo/azul para identificación polaridad

**Material consumible:**
- Bridas (zip ties) para gestión de cables
- (Opcional) Velcro reutilizable

**Herramientas utilizadas:**
- Destornilladores (Phillips, plano)
- Tijeras para bridas
- Multímetro (para verificaciones opcionales)
- Crimpadora JST (para fabricar extensión Z2)
- Pelacables

---

## 📸 Galería Fotográfica Phase 2

**Documentación stock (Step 1):**
- 01-31: Sistema completo stock documentado

**Montaje y cableado (Steps 3-4):**
- 32: Cable extensión Motor Z2
- 33: Alimentación DCIN
- 34: Motores conectados (X, Y, Z, E1)
- 35: Cama caliente alimentación (HB)
- 36: Cama caliente termistor (TB)

**Total:** 36 fotos documentadas

---

## ✅ Estado Final Phase 2

**Completado:**
- ✅ Electrónica stock desconectada y almacenada
- ✅ SKR 1.4 Turbo montada en posición superior (temporal con bridas)
- ✅ Alimentación 24V conectada (PSU → DCIN)
- ✅ 4 motores conectados (X, Y, Z1, Z2 con extensión)
- ✅ Cama caliente conectada (power + termistor)
- ✅ Estrategia endstops definida (sensorless)
- ✅ Arquitectura CAN planificada

**Pendiente (Phase 3 - Toolhead EBB42 CAN):**
- ⏸️ Desmontar toolhead stock
- ⏸️ Instalar EBB42 en toolhead
- ⏸️ Conectar componentes toolhead a EBB42
- ⏸️ Tender cable CAN (4 hilos)
- ⏸️ Configurar CAN bus
- ⏸️ Primera energización y pruebas

---

## 🎯 Logros de Phase 2

1. **Decisión arquitectónica crítica:** SKR en posición superior (óptima para alcance cables)
2. **Implementación limpia:** Solo 2 extensiones necesarias (Z2 + 24V)
3. **Preparación para CAN:** Arquitectura lista para EBB42
4. **Documentación exhaustiva:** 36 fotos + guías paso a paso
5. **Fabricación custom:** Cable extensión Z2 con conectores JST-XH
6. **Identificación profesional:** Termorretráctil para polaridad 24V

---

## 🧠 Lecciones Aprendidas

1. **Planificar antes de cablear:** Evaluar alcance de cables ahorra trabajo
2. **Extensiones cuando sea necesario:** NO cortar cables originales
3. **Identificación clara:** Termorretráctil/etiquetas evitan errores
4. **Documentar todo:** Fotos ANTES/DESPUÉS de cada paso
5. **Arquitectura desde inicio:** Implementar EBB42 CAN desde el principio evita recablear

---

## 🔧 Troubleshooting

### Verificación falla en algún punto

**Acción:**
1. NO continuar hasta resolver
2. Identificar qué punto del checklist falló
3. Corregir el problema
4. Re-verificar checklist completo desde inicio
5. Solo continuar cuando TODOS los puntos sean ✅

### Duda sobre alguna conexión

**Acción:**
- Revisar fotos de referencia (33-36)
- Revisar guía Step 4 (cableado detallado)
- Revisar esquema de pines SKR 1.4 Turbo
- Preguntar en comunidad si la duda persiste
- **NO asumir** - verificar siempre

### No tengo multímetro para verificaciones eléctricas

**Alternativa:**
- Verificación visual exhaustiva es suficiente
- Multímetro es recomendado pero no imprescindible
- En primera energización: observar cuidadosamente (LED, humo, olor)
- Si hay duda, mejor conseguir multímetro básico (~10-15€)

---

## 📈 Progreso del Proyecto

**Fases completadas:**
- ✅ Phase 0: Baseline (documentación inicial)
- ✅ Phase 1: SKR preparación (drivers + jumpers)
- ✅ Phase 2: SKR cableado básico

**Siguiente fase:**
- 📋 Phase 3: Toolhead EBB42 CAN

**Estimación tiempo restante hasta primera impresión:**
- Phase 3: ~4-6 horas
- Phase 4: Firmware Klipper básico ~2-3 horas
- Phase 5: Primera impresión funcional ~2-3 horas

**Total estimado:** 8-12 horas adicionales hasta primera impresión

---

## ➡️ Siguiente Fase

Phase 2 verificada y completada. Listo para continuar con:

**[Phase 3: Toolhead EBB42 CAN](../../phases/phase3/README.md)**

---

## 🎉 ¡Phase 2 Completada!

**Felicidades** - Has completado exitosamente la instalación y cableado básico de la SKR 1.4 Turbo.

El sistema está listo para la siguiente fase: integración del toolhead con EBB42 CAN.

---

**Completado:** 2025-12-21
**Tiempo total Phase 2:** ~6 horas
**Incidencias:** Ninguna
**Calidad:** ✅ Profesional
