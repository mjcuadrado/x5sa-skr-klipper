# Phase 2, Step 1: Documentación Fotográfica Completa

**Objetivo:** Documentar el estado actual del cableado stock ANTES de desconectar nada.

**Tiempo estimado:** 30-40 minutos

**⚠️ IMPORTANTE:** NO desconectes NADA todavía. Solo fotografiar.

---

## 📋 Material Necesario

- [ ] Cámara / smartphone (con buena iluminación)
- [ ] Linterna (para iluminar compartimento inferior)
- [ ] Acceso completo a la impresora
- [ ] Bloc de notas (para apuntar observaciones)

---

## 📸 Fotos Obligatorias

### Compartimento Inferior (Electrónica)

- [ ] **Foto 1:** Vista general del compartimento de electrónica (donde está la placa stock)
- [ ] **Foto 2:** Placa controladora stock (vista frontal completa)
- [ ] **Foto 3:** Placa controladora stock (vista posterior si tiene conexiones)
- [ ] **Foto 4:** Zona de conectores de la placa (acercamiento)
- [ ] **Foto 5:** Cable ribbon plano negro (punto de salida de la placa)
- [ ] **Foto 6:** Cable ribbon plano negro (recorrido hasta arriba)
- [ ] **Foto 7:** Cables al extrusor (punto de salida de la placa)
- [ ] **Foto 8:** Fuente de alimentación 24V (conexiones)

### Distribución Superior (Motores y Sensores)

- [ ] **Foto 9:** Cable ribbon llegando arriba (punto de distribución)
- [ ] **Foto 10:** Conexiones a motores X/Y (CoreXY)
- [ ] **Foto 11:** Conexión a motor Z
- [ ] **Foto 12:** Endstops X, Y, Z (cada uno)
- [ ] **Foto 13:** Cualquier otro sensor visible

### Extrusor/Hotend

- [ ] **Foto 14:** Cables llegando al extrusor (completo)
- [ ] **Foto 15:** Conexión del motor extrusor
- [ ] **Foto 16:** Conexión del hotend/termistor
- [ ] **Foto 17:** Conexión del ventilador de capa
- [ ] **Foto 18:** Conexión del ventilador de hotend

**Guardar en:** `photos/phase2/01_stock_*.jpg`

---

## 📝 Procedimiento

### Paso 1.1: Preparar la Impresora

1. **Apagar completamente** la impresora
2. **Desconectar de la corriente** (por seguridad durante fotos)
3. Abrir acceso al compartimento inferior si es necesario
4. Preparar buena iluminación

### Paso 1.2: Fotografiar Compartimento Inferior

1. Tomar foto general del compartimento
2. Fotografiar placa stock desde todos los ángulos necesarios
3. Acercamiento a zona de conectores
4. Fotografiar cable ribbon negro (salida y recorrido)
5. Fotografiar cables al extrusor
6. Fotografiar fuente de alimentación

**⚠️ IMPORTANTE:**
- Asegúrate que se vean TODOS los conectores claramente
- Que se lean las etiquetas de la placa si las hay
- Que se distingan los colores de los cables

### Paso 1.3: Fotografiar Distribución Superior

1. Seguir el cable ribbon hasta arriba
2. Fotografiar punto donde se distribuye
3. Fotografiar cada motor con sus conexiones
4. Fotografiar cada endstop
5. Fotografiar cualquier sensor adicional

### Paso 1.4: Fotografiar Extrusor

1. Vista general de cables llegando
2. Detalle de cada conexión (motor, hotend, ventiladores)

### Paso 1.5: Apuntar Observaciones

Mientras fotografías, anota:
- Colores de cables que ves
- Etiquetas existentes en cables o placa
- Cualquier cosa inusual o modificación
- Número de pines en cada conector

---

## ✅ Validación

- [ ] Todas las fotos requeridas tomadas
- [ ] Fotos con buena iluminación y enfoque
- [ ] Se ven claramente todos los conectores
- [ ] Se distinguen colores de cables
- [ ] Observaciones anotadas
- [ ] **NO has desconectado nada**

---

## 📤 Entrega

Cuando tengas las fotos:
1. Pégalas en el chat
2. Indicar si viste algo inusual
3. Yo las validaré
4. Las renombraré según convención
5. Las documentaré

---

## 📸 Fotos de Referencia - Sistema Stock Documentado

### Compartimento Inferior - Vista General

![Vista general del compartimento de electrónica](../../photos/phase2/01_compartment_general.jpg)
*Compartimento inferior con placa controladora stock y PSU P360W24V. Sistema dual-board con ribbon cable de distribución visible.*

![Placa controladora stock - Vista superior](../../photos/phase2/02_board_top_view.jpg)
*Placa principal stock vista desde arriba. Se observan conectores de motores, terminales de potencia y salida del ribbon cable.*

![Salida del ribbon cable](../../photos/phase2/03_ribbon_cable_exit.jpg)
*Punto de salida del ribbon cable plano negro desde la placa principal, conectando con la subplaca de distribución superior.*

![Detalle de conectores placa stock](../../photos/phase2/04_connectors_detail.jpg)
*Acercamiento a la zona de conectores de la placa stock: motores, alimentación y ribbon cable.*

![Ruteo del ribbon cable y placa](../../photos/phase2/05_board_ribbon_routing.jpg)
*Vista del recorrido del ribbon cable desde la placa principal hacia arriba del frame.*

![Vista inferior placa - drivers](../../photos/phase2/06_board_bottom_drivers.jpg)
*Parte inferior de la placa stock mostrando drivers de motores integrados.*

![Terminal de alimentación placa stock](../../photos/phase2/07_board_power_terminal.jpg)
*Terminal de alimentación de la placa stock con cables de la PSU conectados.*

### Sistema Dual-Board

![Sistema dual-board completo](../../photos/phase2/08_dual_board_system.jpg)
*Arquitectura de dos placas: placa principal inferior + subplaca de distribución superior conectadas por ribbon cable.*

![Conectores sistema dual-board](../../photos/phase2/09_dual_board_connectors.jpg)
*Detalle de conectores en ambas placas del sistema dual-board.*

![Etiquetado de endstops](../../photos/phase2/10_endstop_labeling_detail.jpg)
*Detalle de etiquetas de endstops en la placa de distribución (X, Y, Z).*

### Fuente de Alimentación (PSU)

![Especificaciones PSU](../../photos/phase2/11_psu_label_specs.jpg)
*Etiqueta de la fuente P360W24V: 24V 15A, 360W máximo.*

![PSU vista lateral](../../photos/phase2/12_psu_side_view.jpg)
*Vista lateral de la PSU montada en el compartimento inferior.*

![Conexiones DC de salida PSU](../../photos/phase2/13_psu_dc_output_connections.jpg)
*Terminales DC de salida de la PSU (+24V y GND) con cables conectados.*

![Etiqueta de precaución PSU](../../photos/phase2/14_psu_caution_label.jpg)
*Etiqueta de advertencia de la PSU con especificaciones de seguridad.*

![Detalle terminales DC PSU](../../photos/phase2/15_psu_dc_terminals_detail.jpg)
*Acercamiento a los terminales de salida DC de la PSU (+24V, GND).*

### Caja de Distribución Superior

![Llegada ribbon cable a distribución](../../photos/phase2/16_distribution_box_ribbon_arrival.jpg)
*Ribbon cable llegando a la caja de distribución en la parte superior del frame.*

![Detalle cables distribución](../../photos/phase2/17_distribution_box_cables_detail.jpg)
*Cables distribuidos desde la caja hacia motores y sensores.*

![Caja distribución exterior](../../photos/phase2/18_distribution_box_external.jpg)
*Vista exterior de la caja de distribución superior.*

![PCB interior distribución](../../photos/phase2/19_distribution_box_interior_pcb.jpg)
*Interior de la caja mostrando la subplaca PCB de distribución.*

![Conectores detalle distribución](../../photos/phase2/20_distribution_box_connectors_detail.jpg)
*Detalle de conectores en la subplaca de distribución.*

### Motores CoreXY (X/Y)

![Motores CoreXY superiores](../../photos/phase2/21_motors_corexy_xy.jpg)
*Motores X e Y del sistema CoreXY montados en la parte superior del frame.*

![Detalle motores CoreXY](../../photos/phase2/22_motors_corexy_detail.jpg)
*Acercamiento a los motores CoreXY con sus cables y poleas de correas.*

### Motores Z (Leadscrews)

![Motor Z1 con leadscrew](../../photos/phase2/23_motor_z1_leadscrew.jpg)
*Motor Z1 (leadscrew izquierdo) con acoplamiento al tornillo trapezoidal.*

![Motor Z2 con leadscrew](../../photos/phase2/24_motor_z2_leadscrew.jpg)
*Motor Z2 (leadscrew derecho) con acoplamiento al tornillo trapezoidal.*

![Motor Y etiquetado](../../photos/phase2/25_motor_y_labeled.jpg)
*Motor Y del sistema CoreXY con etiqueta de identificación.*

### Cama Caliente (Heated Bed)

![Vista general cama y eje Z](../../photos/phase2/26_general_view_bed_z.jpg)
*Vista general de la cama caliente y sistema de elevación Z con dual leadscrew.*

![Cama caliente y motor Z](../../photos/phase2/27_heated_bed_motor_z.jpg)
*Detalle de la cama caliente montada sobre el sistema Z.*

![Cables de potencia cama](../../photos/phase2/28_heated_bed_power_cables.jpg)
*Cables de alimentación de la cama caliente (cables gruesos de potencia).*

![Cables etiquetados cama](../../photos/phase2/29_heated_bed_labeled_cables.jpg)
*Cables de la cama caliente con etiquetas: alimentación y termistor.*

### Extrusor/Hotend

![Extrusor y hotend completo](../../photos/phase2/30_extruder_hotend_complete.jpg)
*Vista completa del extrusor y hotend stock con todos sus componentes.*

![Ventilador extrusor/hotend](../../photos/phase2/31_extruder_hotend_fan.jpg)
*Detalle del ventilador del hotend y sistema de enfriamiento.*

---

## ➡️ Siguiente Paso

Una vez validadas todas las fotos:

**[Step 2: Desconexión Electrónica Stock](step2_stock_disconnection.md)**

---

**Estado:** ✅ Completado (2025-12-20)
**Fotos documentadas:** 31 fotos completas del sistema stock
**Arquitectura identificada:** Dual-board system + subplaca distribución
