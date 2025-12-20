# 1.4 — Instalación física de drivers

**Fecha:** Pendiente
**Estado:** Pendiente

---

## Objetivo

Insertar físicamente los 5 drivers TMC2209 en sus zócalos con la orientación correcta.

---

## Requisitos previos

- [ ] Paso 1.2 completado (jumpers UART configurados)
- [ ] Paso 1.3 completado (orientación documentada)
- [ ] Superficie antiestática o alfombrilla ESD
- [ ] Drivers TMC2209 en sus bolsas antiestáticas

---

## Material necesario

- 5× drivers TMC2209 BigTreeTech
- Superficie antiestática
- Iluminación adecuada
- Paciencia (no forzar nunca)

---

## Proceso paso a paso

### Preparación

1. Verificar que la placa SKR **NO está alimentada**
2. Verificar que los 10 jumpers UART están correctamente insertados
3. Preparar superficie antiestática

### Instalación de cada driver

**Para cada eje (X, Y, Z, E0, E1):**

1. Sacar **UN** driver de su bolsa antiestática
2. Identificar la marca de orientación (PIN 1)
3. Identificar la marca en el zócalo correspondiente
4. Alinear las marcas
5. Colocar el driver sobre el zócalo **sin presionar todavía**
6. Verificar visualmente que todos los pines están alineados con sus orificios
7. Verificar que el driver está perfectamente horizontal (no ladeado)
8. Presionar suavemente hacia abajo con presión uniforme
9. El driver debe insertarse sin forzar
10. Verificar que el driver está completamente asentado (no queda elevado)
11. Verificar visualmente que ningún pin quedó doblado por fuera

---

## ⚠️ Si algo no encaja

**NUNCA fuerces.** Si el driver no entra suavemente:

1. Retíralo completamente
2. Verifica la orientación
3. Verifica que ningún pin esté doblado
4. Verifica que los jumpers no obstruyen el zócalo
5. Intenta de nuevo

---

## Orden de instalación recomendado

1. **E0** (extrusor, más fácil acceso)
2. **E1** (segundo extrusor)
3. **Z** (eje Z)
4. **Y** (eje Y)
5. **X** (eje X)

Este orden deja los ejes más importantes para el final, cuando ya tienes práctica.

---

## Fotos requeridas

### Durante la instalación (al menos 1 driver)
![Proceso de inserción de driver](../../photos/phase1/04a_driver_installation_process.jpg)
**⚠️ PENDIENTE**

### Después de cada driver instalado
![Driver instalado individualmente](../../photos/phase1/04b_driver_installed_closeup.jpg)
**⚠️ PENDIENTE**

### Todos los drivers instalados (vista general)
![SKR con los 5 drivers instalados](../../photos/phase1/04c_all_drivers_installed.jpg)
**⚠️ PENDIENTE**

---

## Verificación por driver

Después de instalar **cada** driver:

- [ ] Driver completamente asentado (no elevado)
- [ ] Ningún pin doblado visible
- [ ] Orientación correcta (marca alineada)
- [ ] Driver horizontal (no ladeado)

---

## Verificación final (5 drivers)

- [ ] 5 drivers instalados (X, Y, Z, E0, E1)
- [ ] Todos con orientación correcta
- [ ] Ningún pin doblado
- [ ] Todos completamente asentados
- [ ] Fotos realizadas

---

## Siguiente paso

➡️ [Verificación visual final](05_visual_verification.md)
