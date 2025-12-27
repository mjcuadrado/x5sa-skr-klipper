# Guía Fotográfica - Instalación Eddy Coil Phase 3
**Proyecto:** Tronxy X5SA Klipper Migration
**Fase:** Phase 3 - Cambio de Probe (XY-08N → Eddy Coil V1.0)
**Fecha:** 2025-12-27

---

## 📸 Resumen de Fotografías Necesarias

**Total fotos requeridas:** 27-32 fotos (incluye routing por cadena portacables)
**Tiempo estimado toma de fotos:** 15-20 minutos
**Almacenamiento:** `/photos/phase3/eddy_coil_installation/`

---

## 🎯 Objetivo de la Documentación Fotográfica

Crear un registro visual completo que permita:
1. **Replicar** la instalación en impresoras similares
2. **Troubleshooting** - Comparar con estado conocido bueno
3. **Reversión** - Volver a XY-08N si es necesario (aunque Phase 3 es estable)
4. **Documentación histórica** del proyecto
5. **Referencia futura** para Phase 12 (Voron toolhead + Orbiter 2.5 + Dragonfly BMO)

---

## ⚠️ IMPORTANTE: Filosofía de Phase 3

**Phase 3 NO es temporal - es un estado PRODUCTION-READY estable:**

```
Phase 3 (ACTUAL - Estado Estable):
├─ Hardware: SKR 1.4 Turbo + EBB42 (detrás) + Eddy Coil
├─ Toolhead: Stock Tronxy con E3D V6 clone
├─ Extrusor: Titan clone en SKR E0 port
├─ Cable Eddy: A través de cadena portacables hasta EBB42
├─ Estado: COMPLETAMENTE FUNCIONAL
└─ Duración: Indefinida (usuario puede quedarse aquí)

Phase 12 (FUTURO - Voron Upgrade):
├─ Hardware: SKR 1.4 Turbo + EBB42 (integrada EN cabezal)
├─ Toolhead: Voron Stealthburner
├─ Extrusor: Orbiter 2.5 en EBB42
├─ Hotend: Dragonfly BMO
├─ Cable Eddy: Corto, dentro del cabezal (no necesita cadena)
└─ Estado: Upgrade opcional para multicolor
```

**Implicaciones para documentación:**
- El "cable temporal" es parte de la **solución Phase 3** (no es un hack)
- El routing por cadena portacables debe ser **robusto y permanente**
- Las fotos deben mostrar una instalación **profesional y limpia**
- Phase 3 debe verse como un **producto terminado**, no work-in-progress

---

## 📋 Checklist de Fotografías

### **CATEGORÍA A: ESTADO INICIAL CON XY-08N (5 fotos)**

#### **A1. Vista General Toolhead con XY-08N**
```
Archivo: 01_toolhead_xy08n_general.jpg
Descripción: Vista frontal completa del toolhead mostrando XY-08N instalado
Ángulo: Frontal, 45° desde arriba
Distancia: 20-30cm
Iluminación: Buena, sin sombras en el sensor

QUÉ DEBE VERSE:
- XY-08N sensor completamente visible
- Nozzle
- Part cooling fan
- Hotend fan
- Cable del XY-08N bajando hacia EBB42

NOTA: Esta es la foto de referencia "antes"
```

#### **A2. XY-08N Conexión en EBB42**
```
Archivo: 02_xy08n_ebb42_connection.jpg
Descripción: Detalle del conector XY-08N en pin PA5 de EBB42
Ángulo: Cenital sobre EBB42, enfocado en PA5
Distancia: 10-15cm
Iluminación: Directa sobre el pin

QUÉ DEBE VERSE:
- Pin PA5 en EBB42 con conector XY-08N
- Etiqueta del pin visible (si existe)
- Cable color y routing
- Pins adyacentes para contexto

NOTA: Importante para reversión
```

#### **A3. XY-08N Montaje Lateral**
```
Archivo: 03_xy08n_mounting_side.jpg
Descripción: Vista lateral mostrando cómo está montado el XY-08N
Ángulo: Lateral izquierdo del toolhead
Distancia: 15-20cm
Iluminación: Lateral para ver profundidad

QUÉ DEBE VERSE:
- Bracket/soporte del XY-08N
- Tornillos de montaje
- Altura relativa al nozzle
- Sistema de montaje completo

NOTA: Para entender mecánica de montaje
```

#### **A4. Routing Cable XY-08N**
```
Archivo: 04_xy08n_cable_routing.jpg
Descripción: Recorrido completo del cable desde sensor hasta EBB42
Ángulo: Vista que capture todo el recorrido
Distancia: 30-40cm
Iluminación: General

QUÉ DEBE VERSE:
- Cable saliendo del XY-08N
- Amarres/guías de cable
- Entrada a EBB42
- Puntos de fijación

NOTA: Para replicar routing similar con Eddy Coil
```

#### **A5. XY-08N Altura sobre Nozzle**
```
Archivo: 05_xy08n_nozzle_height.jpg
Descripción: Vista enfocada en distancia XY-08N ↔ Nozzle
Ángulo: Lateral muy cercano con regla/calibre si es posible
Distancia: 10cm
Iluminación: Lateral fuerte

QUÉ DEBE VERSE:
- Nozzle tip
- Sensor XY-08N tip
- Distancia entre ambos (idealmente con medida)
- Alineación horizontal

NOTA: Comparar con altura final Eddy Coil
```

---

### **CATEGORÍA B: REMOCIÓN XY-08N (3 fotos)**

#### **B1. Desconexión Cable en EBB42**
```
Archivo: 06_xy08n_disconnection_ebb42.jpg
Descripción: Pin PA5 VACÍO después de desconectar XY-08N
Ángulo: Cenital sobre EBB42
Distancia: 10cm
Iluminación: Directa

QUÉ DEBE VERSE:
- Pin PA5 libre (sin conector)
- Cable XY-08N a un lado (identificable)
- Contexto de pins vecinos
- Estado limpio del pin

NOTA: Confirmar desconexión correcta
```

#### **B2. Remoción Tornillos Montaje**
```
Archivo: 07_xy08n_mounting_screws_removed.jpg
Descripción: Tornillos del bracket XY-08N removidos, sensor suelto
Ángulo: Lateral/frontal
Distancia: 15cm
Iluminación: Buena general

QUÉ DEBE VERSE:
- XY-08N parcialmente desmontado
- Tornillos a un lado (contarlos visualmente)
- Bracket vacío o semi-vacío
- Huecos de tornillos visibles

NOTA: Documentar tipo y cantidad de tornillos
```

#### **B3. Toolhead sin Sensor**
```
Archivo: 08_toolhead_empty_no_probe.jpg
Descripción: Toolhead completamente limpio, sin XY-08N
Ángulo: Frontal similar a foto A1
Distancia: 20-30cm
Iluminación: Buena general

QUÉ DEBE VERSE:
- Toolhead vacío (solo nozzle, fans)
- Espacio donde estaba el sensor
- Bracket vacío o removido
- Área de montaje limpia

NOTA: Estado "intermedio" antes de Eddy Coil
```

---

### **CATEGORÍA C: EDDY COIL ANTES DE INSTALAR (4 fotos)**

#### **C1. Eddy Coil Unboxing**
```
Archivo: 09_eddy_coil_package.jpg
Descripción: Sensor Eddy Coil nuevo en su empaque
Ángulo: Cenital sobre mesa de trabajo
Distancia: 30cm
Iluminación: Buena uniforme

QUÉ DEBE VERSE:
- Empaque original BTT
- Sensor Eddy Coil V1.0 visible
- Accesorios incluidos (cables, tornillos)
- Documentación incluida (si existe)

NOTA: Verificar contenido del paquete
```

#### **C2. Eddy Coil Detalle Frontal**
```
Archivo: 10_eddy_coil_detail_front.jpg
Descripción: Vista frontal detallada del sensor
Ángulo: Frontal directo
Distancia: 5-10cm (macro)
Iluminación: Difusa, sin reflejos

QUÉ DEBE VERSE:
- Bobina sensora (coil) claramente
- Marca BTT/modelo visible
- Superficie de detección
- Tornillos de montaje (si tiene)

NOTA: Identificación del sensor
```

#### **C3. Eddy Coil Conexiones**
```
Archivo: 11_eddy_coil_connectors.jpg
Descripción: Vista de cables y conectores del Eddy Coil
Ángulo: Lateral/trasero del sensor
Distancia: 10cm
Iluminación: Buena

QUÉ DEBE VERSE:
- 4 cables: VCC (rojo), GND (negro), SCL, SDA
- Tipo de conector
- Longitud de cables
- Códigos de colores

NOTA: Verificar integridad de cables
```

#### **C4. Bracket/Soporte para Eddy Coil**
```
Archivo: 12_eddy_coil_bracket.jpg
Descripción: Bracket o soporte que se usará para montar Eddy Coil
Ángulo: Múltiples ángulos si es complejo
Distancia: 15cm
Iluminación: Buena

QUÉ DEBE VERSE:
- Bracket completo
- Puntos de montaje para sensor
- Puntos de fijación a toolhead
- Tipo de tornillos necesarios

NOTA: Si es bracket impreso 3D, documentar modelo STL usado
```

---

### **CATEGORÍA D: INSTALACIÓN EDDY COIL (8 fotos)**

#### **D1. Montaje Bracket en Toolhead**
```
Archivo: 13_bracket_mounting_toolhead.jpg
Descripción: Bracket instalado en toolhead (sin sensor aún)
Ángulo: Frontal/lateral
Distancia: 20cm
Iluminación: Buena

QUÉ DEBE VERSE:
- Bracket fijado con tornillos
- Posición relativa al nozzle
- Tornillos de fijación apretados
- Alineación vertical/horizontal

NOTA: Verificar solidez del montaje
```

#### **D2. Eddy Coil en Bracket (sin apretar)**
```
Archivo: 14_eddy_coil_bracket_loose.jpg
Descripción: Sensor colocado en bracket pero sin apretar tornillos
Ángulo: Lateral
Distancia: 15cm
Iluminación: Lateral

QUÉ DEBE VERSE:
- Sensor en posición aproximada
- Tornillos insertados pero no apretados
- Posibilidad de ajuste de altura
- Cables colgando libremente

NOTA: Fase de ajuste de altura
```

#### **D3. Medición Altura Eddy ↔ Nozzle CON CALIBRE**
```
Archivo: 15_eddy_nozzle_height_measurement.jpg
Descripción: Calibre/regla midiendo distancia exacta
Ángulo: Lateral muy cercano
Distancia: 10cm
Iluminación: Lateral fuerte

QUÉ DEBE VERSE:
- Calibre/regla claramente visible
- Medida legible (2-3mm objetivo)
- Nozzle tip
- Eddy Coil coil bottom
- Lectura numérica nítida

NOTA CRÍTICA: Esta es la foto MÁS IMPORTANTE para altura
```

#### **D4. Eddy Coil Montado Final (frontal)**
```
Archivo: 16_eddy_coil_installed_front.jpg
Descripción: Sensor completamente instalado, vista frontal
Ángulo: Frontal similar a A1 y B3
Distancia: 20-30cm
Iluminación: Buena general

QUÉ DEBE VERSE:
- Eddy Coil firmemente montado
- Altura correcta sobre nozzle
- Cables organizados
- Aspecto profesional del montaje

NOTA: Foto "después" para comparar con A1
```

#### **D5. Eddy Coil Montado Final (lateral)**
```
Archivo: 17_eddy_coil_installed_side.jpg
Descripción: Vista lateral del montaje final
Ángulo: Lateral izquierdo
Distancia: 15-20cm
Iluminación: Lateral

QUÉ DEBE VERSE:
- Perfil del Eddy Coil
- Relación espacial con nozzle
- Bracket y puntos de montaje
- Cables saliendo hacia atrás

NOTA: Verificar no interfiere con movimientos
```

#### **D6. Cables Eddy Routing - Salida del Toolhead**
```
Archivo: 18_eddy_cable_routing_toolhead.jpg
Descripción: Cables saliendo del Eddy Coil hacia la cadena portacables
Ángulo: Vista lateral del toolhead
Distancia: 20-30cm
Iluminación: General

QUÉ DEBE VERSE:
- Cables saliendo del Eddy Coil
- Amarres de cables al toolhead
- Punto de entrada a cadena portacables
- Organización con otros cables (heater, thermistor, fans)

NOTA: Cables deben tener holgura para movimiento del toolhead
```

#### **D6b. Cable Eddy a través de Cadena Portacables**
```
Archivo: 18b_eddy_cable_chain.jpg
Descripción: Recorrido del cable dentro/junto a cadena portacables
Ángulo: Vista general mostrando recorrido completo
Distancia: 50-70cm (capturar toolhead + cadena + EBB42)
Iluminación: General uniforme

QUÉ DEBE VERSE:
- Toolhead con Eddy Coil instalado
- Cable recorriendo la cadena portacables
- Trayectoria completa hasta zona de EBB42
- Organización del cable con otros cables de la cadena

NOTA IMPORTANTE: Este es el "cable temporal" de Phase 3
En Phase 12 (Voron toolhead), EBB42 estará integrada en el cabezal
y este cable no será necesario.
```

#### **D6c. Cable Eddy Llegada a EBB42**
```
Archivo: 18c_eddy_cable_ebb42_entry.jpg
Descripción: Punto donde cable sale de cadena y llega a EBB42
Ángulo: Vista de la zona de EBB42
Distancia: 20-30cm
Iluminación: Buena

QUÉ DEBE VERSE:
- Salida del cable de la cadena portacables
- Organización del cable hacia EBB42
- Gestión de longitud sobrante (si existe)
- Fijación del cable para evitar tirones

NOTA: Asegurar que movimientos XY no tensan el cable
```

#### **D7. Conexión I2C en EBB42 - VCC/GND**
```
Archivo: 19_eddy_ebb42_power_connection.jpg
Descripción: Conexiones VCC (rojo) y GND (negro) en EBB42
Ángulo: Cenital sobre EBB42
Distancia: 10cm
Iluminación: Directa

QUÉ DEBE VERSE:
- Cable rojo (VCC) conectado a pin específico
- Cable negro (GND) conectado a pin específico
- Etiquetas de pins visibles
- Conexión firme

NOTA CRÍTICA: Verificar polaridad correcta
```

#### **D8. Conexión I2C en EBB42 - SCL/SDA (PB3/PB4)**
```
Archivo: 20_eddy_ebb42_i2c_connection.jpg
Descripción: Conexiones SCL (PB3) y SDA (PB4) en EBB42
Ángulo: Cenital sobre EBB42, enfoque en PB3/PB4
Distancia: 10cm
Iluminación: Directa

QUÉ DEBE VERSE:
- Cable conectado a PB3 (SCL)
- Cable conectado a PB4 (SDA)
- Etiquetas "PB3" y "PB4" visibles
- Contexto de pins vecinos

NOTA CRÍTICA: Configuración I2C según printer.cfg:308-309
```

---

### **CATEGORÍA E: VERIFICACIONES FINALES (5 fotos)**

#### **E1. Vista General Instalación Completa**
```
Archivo: 21_installation_complete_overview.jpg
Descripción: Vista general de toolhead con Eddy instalado y cableado
Ángulo: Frontal/superior a 45°
Distancia: 30-40cm
Iluminación: Excelente, sin sombras

QUÉ DEBE VERSE:
- Todo el toolhead
- Eddy Coil correctamente montado
- Cables organizados
- Aspecto limpio y profesional

NOTA: Foto de portada para documentación
```

#### **E2. EBB42 Completa con Todas las Conexiones**
```
Archivo: 22_ebb42_all_connections.jpg
Descripción: Vista completa de EBB42 mostrando todas las conexiones
Ángulo: Cenital sobre EBB42
Distancia: 15-20cm
Iluminación: Uniforme

QUÉ DEBE VERSE:
- Eddy Coil: VCC, GND, SCL (PB3), SDA (PB4)
- Heater en PB13
- Thermistor en PA3
- Part cooling fan en PA0
- Hotend fan en PA1
- USB cable
- Todas las conexiones claramente identificables

NOTA: Diagrama de conexiones real vs schematic
```

#### **E3. Toolhead en Posición Home**
```
Archivo: 23_toolhead_home_position.jpg
Descripción: Toolhead en posición home XY
Ángulo: Vista general de la impresora
Distancia: 50-70cm
Iluminación: General

QUÉ DEBEVERSE:
- Impresora completa
- Toolhead en X=0, Y=0 (o posición home)
- Eddy Coil visible
- Contexto de toda la máquina

NOTA: Estado "ready to calibrate"
```

#### **E4. Nozzle sobre Cama (altura test)**
```
Archivo: 24_nozzle_bed_clearance.jpg
Descripción: Vista lateral del nozzle cerca de la cama
Ángulo: Lateral muy bajo, a nivel de cama
Distancia: 20cm
Iluminación: Lateral

QUÉ DEBE VERSE:
- Nozzle a ~5-10mm de la cama
- Eddy Coil con clearance adecuado
- Verificar que Eddy no choca con cama

NOTA: Verificar espacio físico suficiente
```

#### **E5. Screenshot Mainsail/Fluidd - QUERY_PROBE "open"**
```
Archivo: 25_query_probe_open_screenshot.png
Descripción: Captura de pantalla mostrando QUERY_PROBE resultado
Formato: PNG screenshot (no foto física)
Resolución: 1920×1080 o nativa

QUÉ DEBE VERSE:
- Terminal/consola de Mainsail o Fluidd
- Comando: QUERY_PROBE
- Output: probe: open
- Timestamp visible

NOTA: Verificación software de que sensor responde
```

---

### **CATEGORÍA F: COMPARACIONES ANTES/DESPUÉS (2 fotos)**

#### **F1. Comparación XY-08N vs Eddy Coil**
```
Archivo: 26_comparison_xy08n_vs_eddy.jpg
Descripción: Sensores lado a lado para comparación
Ángulo: Cenital sobre mesa
Distancia: 20cm
Iluminación: Uniforme

QUÉ DEBE VERSE:
- XY-08N (removido) a la izquierda
- Eddy Coil V1.0 a la derecha
- Escala de referencia (regla)
- Diferencias de tamaño visibles

NOTA: Documentación histórica del cambio
```

#### **F2. Toolhead Antes (A1) vs Después (D4) - Collage**
```
Archivo: 27_before_after_collage.jpg
Descripción: Montaje lado a lado de fotos A1 y D4
Formato: Imagen editada/collage
Software: Cualquier editor de fotos

QUÉ DEBE VERSE:
Izquierda: Foto A1 (con XY-08N)
Derecha: Foto D4 (con Eddy Coil)
Mismo ángulo, misma iluminación

NOTA: Impacto visual del cambio
```

---

## 📂 Organización de Archivos

### **Estructura de Carpetas**

```
/Users/mjcuadrado/projects/x5sa-skr-klipper/
└── photos/
    └── phase3/
        └── eddy_coil_installation/
            ├── 01_before_xy08n/
            │   ├── 01_toolhead_xy08n_general.jpg
            │   ├── 02_xy08n_ebb42_connection.jpg
            │   ├── 03_xy08n_mounting_side.jpg
            │   ├── 04_xy08n_cable_routing.jpg
            │   └── 05_xy08n_nozzle_height.jpg
            ├── 02_removal/
            │   ├── 06_xy08n_disconnection_ebb42.jpg
            │   ├── 07_xy08n_mounting_screws_removed.jpg
            │   └── 08_toolhead_empty_no_probe.jpg
            ├── 03_eddy_unboxing/
            │   ├── 09_eddy_coil_package.jpg
            │   ├── 10_eddy_coil_detail_front.jpg
            │   ├── 11_eddy_coil_connectors.jpg
            │   └── 12_eddy_coil_bracket.jpg
            ├── 04_installation/
            │   ├── 13_bracket_mounting_toolhead.jpg
            │   ├── 14_eddy_coil_bracket_loose.jpg
            │   ├── 15_eddy_nozzle_height_measurement.jpg ★ CRÍTICA
            │   ├── 16_eddy_coil_installed_front.jpg
            │   ├── 17_eddy_coil_installed_side.jpg
            │   ├── 18_eddy_cable_routing.jpg
            │   ├── 19_eddy_ebb42_power_connection.jpg ★ CRÍTICA
            │   └── 20_eddy_ebb42_i2c_connection.jpg ★ CRÍTICA
            ├── 05_verification/
            │   ├── 21_installation_complete_overview.jpg
            │   ├── 22_ebb42_all_connections.jpg ★ CRÍTICA
            │   ├── 23_toolhead_home_position.jpg
            │   ├── 24_nozzle_bed_clearance.jpg
            │   └── 25_query_probe_open_screenshot.png
            └── 06_comparisons/
                ├── 26_comparison_xy08n_vs_eddy.jpg
                └── 27_before_after_collage.jpg
```

---

## 📝 Metadata de Fotos (para EXIF o notas)

**Para cada foto incluir en metadata/nombre:**
- Fecha: 2025-12-27
- Proyecto: X5SA-Phase3-Eddy
- Hardware: EBB42-V1.2, Eddy-Coil-V1.0
- Fase: Instalación física

---

## 🎯 Fotos CRÍTICAS (Prioridad Máxima)

Si el tiempo es limitado, estas **9 fotos son OBLIGATORIAS:**

| # | Archivo | Categoría | Por qué es crítica |
|---|---------|-----------|-------------------|
| 1 | `01_toolhead_xy08n_general.jpg` | A1 | Referencia "antes" |
| 2 | `15_eddy_nozzle_height_measurement.jpg` | D3 | **Altura exacta 2-3mm** |
| 3 | `16_eddy_coil_installed_front.jpg` | D4 | Referencia "después" |
| 4 | `18b_eddy_cable_chain.jpg` | D6b | **Routing por cadena portacables** |
| 5 | `18c_eddy_cable_ebb42_entry.jpg` | D6c | **Gestión cable llegada EBB42** |
| 6 | `19_eddy_ebb42_power_connection.jpg` | D7 | **Verificar VCC/GND correcto** |
| 7 | `20_eddy_ebb42_i2c_connection.jpg` | D8 | **Verificar PB3/PB4 correcto** |
| 8 | `22_ebb42_all_connections.jpg` | E2 | **Diagrama completo wiring** |
| 9 | `25_query_probe_open_screenshot.png` | E5 | **Prueba funcional** |

---

## 📷 Recomendaciones Técnicas de Fotografía

### **Equipo**
- Smartphone moderno (≥12 MP) es suficiente
- Linterna/luz adicional si el taller es oscuro
- Paño blanco como fondo (opcional, para fotos C)

### **Configuración Cámara**
- **Resolución:** Máxima disponible (4K si es posible)
- **Flash:** OFF (usar luz ambiente o linterna externa)
- **HDR:** ON (para mejor detalle en sombras)
- **Estabilización:** ON
- **Formato:** JPG (PNG para screenshots)

### **Técnica**
- **Enfoque:** Tap en pantalla sobre el objeto principal
- **Exposición:** Ajustar manualmente si es muy oscuro/brillante
- **Estabilidad:** Apoyar brazos, usar temporizador de 2 seg
- **Múltiples tomas:** 2-3 fotos por toma, elegir la mejor
- **Verificar:** Revisar foto antes de continuar (zoom para verificar nitidez)

### **Iluminación**
- **Evitar:** Sombras duras, reflejos en plásticos
- **Preferir:** Luz difusa, múltiples fuentes
- **Ángulo luz:** 45° lateral para profundidad

---

## ✅ Checklist Final Fotografías

Antes de continuar con calibraciones, verificar:

- [ ] **27 fotos** tomadas (mínimo 7 críticas)
- [ ] Todas las fotos están **nítidas** (no borrosas)
- [ ] **Altura Eddy ↔ Nozzle** medida visible (foto 15)
- [ ] **Conexiones I2C** PB3/PB4 claramente visibles (foto 20)
- [ ] **VCC/GND** polaridad correcta visible (foto 19)
- [ ] Fotos organizadas en carpetas correctas
- [ ] Screenshot `QUERY_PROBE` muestra "open" (foto 25)
- [ ] Comparación antes/después creada (foto 27)

---

## 📤 Integración con Documentación

Estas fotos se integrarán en:

1. **LOG_INSTALACION.md** - Referencias inline durante instalación
2. **EDDY_COIL_INSTALLATION.md** - Galería de instalación física
3. **phases/phase3/README.md** - Foto antes/después destacada
4. **HARDWARE_EVOLUTION.md** - Registro histórico del cambio

---

## 🚀 Próximo Paso

**Una vez tengas las fotos:**
1. Organizarlas en la estructura de carpetas indicada
2. Renombrar según nomenclatura (01_, 02_, etc.)
3. Actualizar LOG_INSTALACION.md con referencias `![alt](path/to/photo.jpg)`
4. Continuar con calibraciones

---

**¿Listo para tomar las fotos? Dime cuando tengas:**
- [ ] Cámara/smartphone lista
- [ ] Buena iluminación
- [ ] Calibre/regla disponible (para foto 15)
- [ ] Eddy Coil desempacado

**Entonces empezamos con Categoría A (Estado inicial con XY-08N).**
