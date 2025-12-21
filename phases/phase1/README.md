# Phase 1 — SKR 1.4 Turbo: Preparación de electrónica

**Objetivo:** Preparar la placa SKR 1.4 Turbo con drivers TMC2209 en modo UART, sin cablear ni alimentar nada.

**Estado:** ✅ Completada (2025-12-20)

---

## Índice

1. [SKR 1.4 Turbo stock (sin tocar)](01_skr_stock.md)
2. [Configuración jumpers UART](02_uart_jumpers.md)
3. [Orientación drivers TMC2209](03_driver_orientation.md)
4. [Instalación física de drivers](04_driver_installation.md)
5. [Verificación visual final](05_visual_verification.md)

---

## Requisitos previos

- [ ] Phase 0 completada
- [ ] SKR 1.4 Turbo sin usar
- [ ] 5x drivers TMC2209 originales BigTreeTech
- [ ] Superficie antiestática o alfombrilla ESD
- [ ] Iluminación adecuada
- [ ] Cámara para documentación

---

## Resultado esperado al finalizar Phase 1

- SKR 1.4 Turbo con jumpers UART configurados en X, Y, Z, E0, E1
- 5x drivers TMC2209 correctamente orientados e insertados
- Verificación visual completa documentada con fotos
- **Sin cables conectados**
- **Sin alimentación**
- **Lista para Phase 2 (cableado)**

---

## Reglas de seguridad

1. **NUNCA** trabajar con la placa alimentada
2. **NUNCA** forzar drivers en los zócalos
3. **SIEMPRE** verificar orientación antes de insertar
4. **SIEMPRE** documentar con fotos ANTES y DESPUÉS de cada cambio

---

## Punto de rollback

Si algo sale mal, volver a SKR sin drivers, sin jumpers (estado documentado en `01_skr_stock.md`)
