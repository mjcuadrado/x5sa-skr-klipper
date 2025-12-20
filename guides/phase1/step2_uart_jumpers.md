# Phase 1, Step 2: Configuración Jumpers UART

**Objetivo:** Configurar jumpers para habilitar comunicación UART con los drivers TMC2209.

**Tiempo estimado:** 15-20 minutos

---

## 📋 Material Necesario

- [ ] SKR 1.4 Turbo (del Step 1)
- [ ] 5 jumpers (1 por cada eje)
- [ ] Pinzas de punta fina (recomendado)
- [ ] Buena iluminación
- [ ] Cámara / smartphone

---

## 🤔 ¿Qué es UART y por qué lo necesitamos?

**UART** (Universal Asynchronous Receiver-Transmitter) es un protocolo de comunicación que permite controlar los drivers TMC2209 completamente por software.

**Sin UART (modo standalone):**
- ❌ Drivers trabajan con configuración básica por hardware
- ❌ No se puede ajustar corriente por software
- ❌ Sin funciones avanzadas (StealthChop, StallGuard)
- ❌ Sin monitorización de temperatura

**Con UART:**
- ✅ Klipper controla todo el driver por software
- ✅ Ajuste fino de corriente
- ✅ Modo silencioso (StealthChop)
- ✅ Monitorización en tiempo real
- ✅ Detección de problemas

---

## 📸 Fotos Obligatorias

Debes hacer las siguientes fotos:

### ANTES de insertar jumpers:
- [ ] **Foto 1:** Vista general placa sin jumpers
- [ ] **Foto 2:** Acercamiento zona jumpers socket X (mostrando MS0, MS1, MS2)
- [ ] **Foto 3:** Acercamiento zona jumpers socket Y

### DESPUÉS de insertar jumpers:
- [ ] **Foto 4:** Vista general placa con los 10 jumpers instalados
- [ ] **Foto 5:** Acercamiento socket X con jumpers (MS0✅ MS1✅ MS2❌)
- [ ] **Foto 6:** Acercamiento socket Y con jumpers
- [ ] **Foto 7:** Acercamiento socket Z con jumpers
- [ ] **Foto 8:** Acercamiento socket E0 con jumpers
- [ ] **Foto 9:** Acercamiento socket E1 con jumpers

**Guardar en:** `photos/phase1/`

**Nombres sugeridos:**
- `02a_uart_jumpers_before.jpg` (antes, vista general)
- `02b_uart_jumpers_after.jpg` (después, vista general)
- `02c_uart_jumpers_detail_x.jpg` (detalle X con jumpers)
- etc.

---

## 📝 Procedimiento

### Paso 2.1: Identificar Posiciones de Jumpers

Debajo de **cada** zócalo de driver (X, Y, Z, E0, E1) hay **3 pares de pines**:

```
Vista desde arriba del zócalo:

     ┌─────────────────┐
     │   DRIVER SOCKET │
     └─────────────────┘
         ↓  ↓  ↓
      [MS1][MS2][MS3]
       ↑    ↑    ↑
    Izq  Centro Der
```

**Configuración UART para TMC2209:**
- **MS1 (izquierda):** ❌ VACÍO (sin jumper)
- **MS2 (centro):** ❌ VACÍO (sin jumper)
- **MS3 (derecha):** ✅ Jumper insertado

**IMPORTANTE:** SKR 1.4 usa MS3 como pin UART. Solo se necesita 1 jumper por eje.

### Paso 2.2: Hacer Fotos "ANTES"

Antes de tocar nada, hacer las fotos 1-3 de la lista arriba (placa sin jumpers).

### Paso 2.3: Insertar Jumpers - Eje X

1. Localizar el socket **X** (esquina superior izquierda generalmente)
2. Localizar los 3 pares de pines debajo del socket
3. Tomar un jumper
4. Insertar en **MS3** (par derecho)
5. Verificar que esté completamente insertado (no ladeado)
6. **Dejar MS1 y MS2 vacíos**

**Resultado eje X:**
- MS1: ❌ Vacío
- MS2: ❌ Vacío
- MS3: ✅ Jumper

### Paso 2.4: Repetir para Ejes Y, Z, E0, E1

Repetir exactamente el mismo proceso para los otros 4 sockets:

**Eje Y:**
- MS1: ❌ Vacío
- MS2: ❌ Vacío
- MS3: ✅ Jumper

**Eje Z:**
- MS1: ❌ Vacío
- MS2: ❌ Vacío
- MS3: ✅ Jumper

**Eje E0:**
- MS1: ❌ Vacío
- MS2: ❌ Vacío
- MS3: ✅ Jumper

**Eje E1:**
- MS1: ❌ Vacío
- MS2: ❌ Vacío
- MS3: ✅ Jumper

### Paso 2.5: Verificación Visual

Una vez insertados todos los jumpers:

1. **Contar jumpers:** Debe haber exactamente **5 jumpers** (1 por eje × 5 ejes)
2. **Verificar cada eje:**
   - ❌ MS1 está vacío
   - ❌ MS2 está vacío
   - ✅ MS3 tiene jumper
3. **Verificar que no hay jumpers ladeados**
4. **Verificar que no hay jumpers en posiciones incorrectas**

### Paso 2.6: Hacer Fotos "DESPUÉS"

Hacer todas las fotos 4-9 de la lista arriba (placa con jumpers).

---

## ✅ Validación

Antes de continuar al siguiente paso, verifica:

- [ ] 5 jumpers insertados en total
- [ ] Cada eje (X, Y, Z, E0, E1) tiene exactamente 1 jumper
- [ ] Todos en MS3, MS1 y MS2 vacíos
- [ ] Ningún jumper ladeado o mal insertado
- [ ] Foto "después" realizada y documentada

---

## ⚠️ Troubleshooting

### Problema: No tengo suficientes jumpers

**Síntoma:** Me faltan jumpers para completar los 10

**Solución:**
1. Verificar que no vinieron con la placa (a veces incluyen bolsa de jumpers)
2. Los jumpers son estándar de 2.54mm
3. Se pueden comprar en cualquier tienda de electrónica
4. **NO continúes** sin los 10 jumpers

### Problema: Los jumpers no entran

**Síntoma:** Los jumpers no se insertan fácilmente

**Solución:**
1. Verificar que estás usando el tipo correcto (2.54mm pitch)
2. Verificar que los pines no están doblados
3. Insertar con suavidad pero firmeza
4. NO fuerces (riesgo de doblar pines)

### Problema: Inserté jumpers en MS1 o MS2 por error

**Síntoma:** Tengo jumpers en MS1 o MS2

**Solución:**
1. **REMOVER** todos los jumpers de MS1 y MS2
2. MS1 y MS2 deben estar **completamente vacíos** en todos los ejes
3. Verificar configuración final: MS1❌ MS2❌ MS3✅

### Problema: No sé dónde están MS1, MS2, MS3

**Síntoma:** No identifico los pares de pines

**Solución:**
1. Los pares están etiquetados en la serigrafía de la placa
2. Desde el zócalo hacia abajo: MS1 (primero), MS2 (segundo), MS3 (tercero)
3. **MS3 es el más a la derecha** - ahí va el jumper
4. Consultar [pinout oficial](https://github.com/bigtreetech/BIGTREETECH-SKR-V1.3/blob/master/BTT%20SKR%20V1.4/Hardware/BTT%20SKR%20V1.4PIN.pdf)

---

## 📚 Información Adicional

### ¿Qué pasa si me equivoco en la configuración?

**Si pones jumpers incorrectos:**
- Los drivers TMC2209 funcionarán en modo standalone
- Klipper mostrará error "Unable to read tmc uart"
- La impresora NO se dañará
- Simplemente corrige los jumpers y reinicia

### ¿Por qué MS0 + MS1 y no otras combinaciones?

Esta configuración específica (MS0+MS1, MS2 vacío) le indica al driver TMC2209:
- Activar interfaz UART
- Responder en dirección específica
- Deshabilitar configuración por hardware

Es la configuración estándar para Klipper con TMC2209.

### ¿Puedo usar modo SPI en vez de UART?

Sí, pero los TMC2209 solo soportan UART.

Para SPI necesitarías drivers diferentes:
- TMC2130 (SPI)
- TMC5160 (SPI)

En esta guía usamos TMC2209 + UART por ser la configuración más común y silenciosa.

---

## 🔗 Referencias

- [TMC2209 Datasheet](https://www.trinamic.com/products/integrated-circuits/details/tmc2209-la/)
- [Klipper TMC Drivers Guide](https://www.klipper3d.org/TMC_Drivers.html)
- [SKR 1.4 Jumper Configuration Manual](https://github.com/GadgetAngel/SKR-V1.4-Turbo-Stepper-Driver-Jumper-Configuration-Manual)
- [HARDWARE_REFERENCE.md - TMC2209](../../HARDWARE_REFERENCE.md#tmc2209-drivers)

---

## ➡️ Siguiente Paso

Una vez completada la configuración de jumpers y validación:

**[Step 3: Orientación Drivers TMC2209](step3_driver_orientation.md)**

---

**Estado:** ✅ Completado (2025-12-20)
**Configuración validada:** 5 jumpers en MS3, MS1 y MS2 vacíos
