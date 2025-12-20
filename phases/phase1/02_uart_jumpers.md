# 1.2 — Configuración jumpers UART

**Fecha:** Pendiente
**Estado:** Pendiente

---

## Objetivo

Configurar los jumpers para habilitar comunicación UART con los drivers TMC2209 en todos los ejes.

---

## ¿Qué es UART?

UART (Universal Asynchronous Receiver-Transmitter) es un protocolo de comunicación que permite:
- Configurar drivers por software (corriente, microstepping, etc.)
- Monitorizar temperatura y estado del driver
- Habilitar funciones avanzadas como StealthChop y StallGuard

**Sin UART:** Los drivers funcionan en modo standalone (configuración por hardware, muy limitado)
**Con UART:** Klipper puede controlar y configurar los drivers completamente

---

## Material necesario

- 10 jumpers (2 por cada eje: X, Y, Z, E0, E1)
- Pinzas de punta fina (recomendado)
- Buena iluminación

---

## Configuración de jumpers por eje

Para **cada** uno de los 5 ejes (X, Y, Z, E0, E1), insertar **2 jumpers**:

### Posiciones exactas (debajo de cada zócalo de driver)

En la SKR 1.4 Turbo, debajo de cada zócalo hay 3 pares de pines:

```
[MS0] [MS1] [MS2]
```

**Configuración UART (TMC2209):**
- Insertar jumper en **MS0** (par izquierdo)
- Insertar jumper en **MS1** (par central)
- Dejar **MS2** vacío (par derecho)

---

## Proceso paso a paso

**ANTES DE EMPEZAR:** Hacer foto de la placa sin jumpers (estado stock)

### Para cada eje (X, Y, Z, E0, E1):

1. Localizar el zócalo del driver
2. Localizar los 3 pares de pines debajo del zócalo
3. Insertar jumper en MS0 (izquierda)
4. Insertar jumper en MS1 (centro)
5. Verificar que MS2 quede vacío (derecha)
6. Verificar que el jumper esté completamente insertado (no ladeado)

### Después de los 5 ejes:

7. Contar jumpers: debe haber **10 jumpers en total** (2 × 5 ejes)
8. Verificar visualmente cada eje
9. **Hacer foto de la placa con todos los jumpers insertados**

---

## Fotos de referencia

### Antes (sin jumpers)
![SKR stock sin jumpers](../../photos/phase1/01_skr14turbo_stock.jpg)

### Después (con jumpers UART)
![SKR con jumpers UART configurados](../../photos/phase1/02_skr14turbo_uart_jumpers.jpg)
**⚠️ PENDIENTE: Esta foto debe hacerse al completar este paso**

---

## Verificación final

Antes de continuar al siguiente paso, confirma:

- [ ] 10 jumpers insertados en total
- [ ] Cada eje tiene exactamente 2 jumpers (MS0 + MS1)
- [ ] MS2 vacío en todos los ejes
- [ ] Ningún jumper está ladeado o mal insertado
- [ ] Foto "después" realizada y guardada

---

## Errores comunes

❌ **ERROR:** Insertar jumpers en MS2 (modo standalone, no UART)
❌ **ERROR:** Insertar solo 1 jumper por eje
❌ **ERROR:** Mezclar configuraciones entre ejes

---

## Siguiente paso

➡️ [Orientación drivers TMC2209](03_driver_orientation.md)
