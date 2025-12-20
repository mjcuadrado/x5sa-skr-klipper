# 1.3 — Orientación drivers TMC2209

**Fecha:** Pendiente
**Estado:** Pendiente

---

## Objetivo

Documentar la orientación correcta de los drivers TMC2209 ANTES de insertarlos físicamente.

**⚠️ CRÍTICO:** Insertar un driver con orientación incorrecta puede dañarlo permanentemente.

---

## ¿Cómo identificar la orientación?

### En el driver TMC2209

Los drivers TMC2209 tienen un **pequeño círculo o marca** en una de las esquinas del chip principal.
Esta marca indica el **PIN 1**.

### En el zócalo de la SKR 1.4 Turbo

Cada zócalo tiene una **marca de referencia** (generalmente un pequeño triángulo o esquina recortada) que indica dónde debe ir el PIN 1 del driver.

---

## Regla de oro

**La marca del driver debe alinearse con la marca del zócalo.**

---

## Material necesario

- 5× drivers TMC2209 BigTreeTech (sin sacar de bolsa antiestática todavía)
- Lupa o buena iluminación (la marca puede ser muy pequeña)
- Cámara para documentar

---

## Proceso paso a paso

### Paso 1: Identificar marcas en los zócalos

1. Localizar los 5 zócalos de drivers (X, Y, Z, E0, E1)
2. En cada zócalo, buscar la marca de referencia (triángulo, esquina recortada, o punto)
3. **Hacer foto de cada zócalo mostrando claramente la marca**

### Paso 2: Identificar marca en un driver

4. Sacar **UN SOLO** driver de su bolsa antiestática
5. Localizar el círculo/marca en una esquina del chip TMC2209
6. **Hacer foto del driver mostrando claramente la marca**

### Paso 3: Verificar orientación SIN insertar

7. Colocar el driver sobre el zócalo (sin insertarlo)
8. Alinear la marca del driver con la marca del zócalo
9. **Hacer foto mostrando la orientación correcta**
10. Retirar el driver y volver a guardarlo en bolsa antiestática

---

## Fotos requeridas

### Vista general de zócalos
![Zócalos SKR con marcas de orientación](../../photos/phase1/03a_socket_orientation_marks.jpg)
**⚠️ PENDIENTE**

### Driver TMC2209 con marca visible
![Driver TMC2209 mostrando marca PIN 1](../../photos/phase1/03b_tmc2209_pin1_mark.jpg)
**⚠️ PENDIENTE**

### Orientación correcta (driver sobre zócalo, sin insertar)
![Orientación correcta driver vs zócalo](../../photos/phase1/03c_correct_orientation.jpg)
**⚠️ PENDIENTE**

---

## Verificación antes de continuar

- [ ] Identificada la marca de orientación en los 5 zócalos
- [ ] Identificada la marca de orientación en al menos 1 driver
- [ ] Fotos realizadas y guardadas
- [ ] Drivers vueltos a sus bolsas antiestáticas
- [ ] Comprendido que la marca del driver va alineada con la marca del zócalo

---

## ⚠️ IMPORTANTE

**NO insertes los drivers todavía.**

Este paso es solo para documentar y verificar orientación.
La inserción física se hace en el siguiente paso con precauciones adicionales.

---

## Errores comunes

❌ **ERROR:** Insertar el driver rotado 180°
❌ **ERROR:** Forzar el driver sin verificar orientación
❌ **ERROR:** No identificar correctamente las marcas

---

## Siguiente paso

➡️ [Instalación física de drivers](04_driver_installation.md)
