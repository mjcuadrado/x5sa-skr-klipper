# Decisión Crítica Phase 3: CAN Bus vs USB para EBB42

**Fecha:** 2025-12-21
**Estado:** 🔄 En análisis
**Impacto:** CRÍTICO - Define toda la arquitectura de Phase 3

---

## 🎯 Contexto

La BTT EBB42 CAN V1.2 soporta **DOS modos de comunicación** con la placa principal (SKR 1.4 Turbo):

1. **CAN Bus** - Comunicación por bus CAN (4 hilos)
2. **USB** - Comunicación por USB (1 cable USB + alimentación separada)

Esta decisión afecta:
- Cableado del toolhead
- Configuración de Klipper
- Complejidad de instalación
- Fiabilidad del sistema
- Mantenimiento futuro

---

## 📊 Opción 1: CAN Bus

### Arquitectura

```
┌─────────────────────────────────┐
│ SKR 1.4 Turbo (Frame Superior)  │
│                                 │
│ CAN Transceiver (integrado)     │
│ ↕ Cable 4 hilos                 │
└─────────────────────────────────┘
         ↓
    CAN_H, CAN_L, 24V, GND
         ↓
┌─────────────────────────────────┐
│ EBB42 CAN (Toolhead)            │
│ - Comunicación: CAN bus         │
│ - Alimentación: 24V del cable   │
└─────────────────────────────────┘
```

### Cableado Necesario

**Cable al toolhead (4 hilos):**
- **CAN_H** - Señal CAN High (twisted pair)
- **CAN_L** - Señal CAN Low (twisted pair)
- **24V** - Alimentación positiva
- **GND** - Masa

**Implementación:**
- Cat6 (twisted pair) para CAN_H/CAN_L
- Cable alimentación separado (1.5mm²) para 24V/GND
- Total: ~4 conductores en cable multicore o 2 cables paralelos

### Ventajas ✅

1. **Robusto ante interferencias electromagnéticas**
   - Señal diferencial (CAN_H - CAN_L)
   - Twisted pair reduce ruido
   - Ideal para entornos con motores/calentadores

2. **Distancias largas**
   - CAN bus funciona hasta 40 metros (overkill para impresora)
   - No degradación de señal

3. **Múltiples dispositivos en el mismo bus**
   - Futuro: Podría añadir más placas CAN si necesario
   - Arquitectura escalable

4. **Menos cables móviles**
   - 4 hilos vs potencial cable USB + alimentación
   - Más limpio si se hace bien

5. **Estándar industrial**
   - Usado en automoción, maquinaria
   - Protocolo robusto y probado

### Desventajas ⚠️

1. **Configuración más compleja**
   - Requiere configurar CAN bus en Klipper
   - Obtener `canbus_uuid` con `canbus_query.py`
   - Configurar bitrate (250k, 500k, 1M)
   - Terminación 120Ω en ambos extremos del bus

2. **Flasheo firmware más complejo**
   - Firmware EBB42 en modo CAN
   - Requiere DFU mode o CanBoot
   - Más pasos vs USB directo

3. **Debugging más difícil**
   - No se ve directamente en `ls /dev/ttyUSB*`
   - Requiere usar herramientas CAN (`candump`, `cansend`)
   - Curva de aprendizaje mayor

4. **Requiere cable específico**
   - Necesita twisted pair para CAN
   - Cat6 + cable alimentación
   - Fabricación más cuidadosa

5. **Punto de fallo adicional**
   - CAN transceiver puede fallar
   - Terminación 120Ω crítica

### Configuración Klipper

**SKR 1.4 Turbo (printer.cfg):**
```ini
[mcu]
serial: /dev/serial/by-id/usb-Klipper_lpc1769_...
canbus_interface: can0

[mcu EBBCan]
canbus_uuid: 1234567890ab  # Obtener con canbus_query.py
canbus_interface: can0
```

**Pasos configuración:**
1. Compilar firmware Klipper para EBB42 (modo CAN, bitrate 1M)
2. Flashear EBB42 en modo DFU
3. Configurar interfaz CAN en host (can0)
4. Ejecutar `canbus_query.py` para obtener UUID
5. Añadir `[mcu EBBCan]` en printer.cfg
6. Configurar todos los pines como `EBBCan:PA0`, etc.

---

## 📊 Opción 2: USB

### Arquitectura

```
┌─────────────────────────────────┐
│ SKR 1.4 Turbo (Frame Superior)  │
│ - NO usa CAN transceiver        │
└─────────────────────────────────┘

┌─────────────────────────────────┐
│ Host (Raspberry Pi/PC)          │
│ ↕ USB (desde EBB42)             │
│ ↕ USB (desde SKR)               │
└─────────────────────────────────┘
         ↓                    ↓
    [SKR USB]          [EBB42 USB]
                            ↑
                    ┌───────┴────────┐
                    │ Cable al       │
                    │ toolhead:      │
                    │ - USB          │
                    │ - 24V          │
                    │ - GND          │
                    └────────────────┘
```

### Cableado Necesario

**Cable al toolhead (3 componentes):**
- **Cable USB** (4 hilos: D+, D-, VCC, GND)
  - Típicamente USB 2.0 cable estándar
  - O cable custom con USB
- **24V** - Alimentación positiva (cable separado grueso)
- **GND** - Masa (cable separado grueso)

**Implementación:**
- Cable USB (puede ser USB-A a USB-C o micro-USB según EBB42)
- Cable alimentación 24V/GND (1.5mm²)
- Total: 2 cables físicos (USB + alimentación)

### Ventajas ✅

1. **Configuración MUCHO más simple**
   - Klipper ve EBB42 como MCU adicional directo
   - No requiere configurar CAN bus en host
   - No requiere obtener canbus_uuid
   - Aparece como `/dev/serial/by-id/usb-Klipper_stm32g0b1...`

2. **Flasheo firmware trivial**
   - Modo DFU por USB estándar
   - Mismo proceso que flashear SKR
   - `make flash` directo
   - NO requiere CanBoot

3. **Debugging sencillo**
   - Se ve directamente con `ls /dev/ttyUSB*` o `ls /dev/serial/by-id/`
   - Logs claros en Klipper
   - Fácil identificar problemas

4. **Menos puntos de fallo**
   - No depende de CAN transceiver
   - No requiere terminación 120Ω
   - USB es plug-and-play

5. **Método "moderno" y recomendado actualmente**
   - Comunidades Voron, RatRig recomiendan USB para toolhead boards
   - Klipper optimizado para múltiples MCUs USB
   - Menos problemas reportados vs CAN

6. **Facilita actualizaciones firmware**
   - `make flash` directo desde SSH
   - No requiere herramientas CAN adicionales

### Desventajas ⚠️

1. **Cable USB adicional al toolhead**
   - 2 cables físicos vs potencial 1 cable multicore CAN
   - Cable USB puede ser más rígido
   - Gestión de cables ligeramente más compleja

2. **USB puede sufrir interferencias EMI**
   - Menos robusto que CAN diferencial
   - Requiere cable USB de calidad (blindado)
   - Ferritas recomendadas si hay problemas

3. **Longitud cable USB limitada**
   - USB 2.0: Máximo 5 metros (suficiente para impresora)
   - Puede requerir repetidor si distancia extrema (no aplica aquí)

4. **Dos dispositivos USB en host**
   - SKR + EBB42 = 2 puertos USB ocupados
   - Raspberry Pi tiene 4 puertos USB (suficiente)
   - Puede requerir USB hub si host limitado

5. **Latencia ligeramente mayor que CAN**
   - Diferencia insignificante para impresión 3D
   - CAN puede ser ~1-2ms más rápido (irrelevante)

### Configuración Klipper

**SKR 1.4 Turbo (printer.cfg):**
```ini
[mcu]
serial: /dev/serial/by-id/usb-Klipper_lpc1769_...

[mcu EBB]
serial: /dev/serial/by-id/usb-Klipper_stm32g0b1_...
```

**Pasos configuración:**
1. Compilar firmware Klipper para EBB42 (modo USB)
2. Flashear EBB42 en modo DFU por USB
3. Conectar USB desde EBB42 a host
4. Identificar serial ID: `ls /dev/serial/by-id/`
5. Añadir `[mcu EBB]` en printer.cfg con serial ID
6. Configurar todos los pines como `EBB:PA0`, etc.

**Mucho más simple que CAN.**

---

## 🔬 Comparación Directa

| Criterio | CAN Bus | USB | Ganador |
|----------|---------|-----|---------|
| **Simplicidad configuración** | ⚠️ Compleja | ✅ Simple | **USB** |
| **Simplicidad cableado** | ✅ 4 hilos (1 cable) | ⚠️ USB + alimentación (2 cables) | **CAN** |
| **Robustez señal** | ✅ Muy robusto (diferencial) | ⚠️ Menos robusto | **CAN** |
| **Debugging** | ⚠️ Difícil | ✅ Fácil | **USB** |
| **Flasheo firmware** | ⚠️ Complejo | ✅ Trivial | **USB** |
| **Escalabilidad** | ✅ Múltiples dispositivos bus | ⚠️ 1 USB por device | **CAN** |
| **Comunidad/soporte** | ⚠️ Menos común | ✅ Más común actualmente | **USB** |
| **Mantenimiento** | ⚠️ Más complejo | ✅ Más simple | **USB** |
| **Velocidad** | ✅ Ligeramente más rápido | ⚠️ Ligeramente más lento | Empate (irrelevante) |
| **Coste** | ✅ Cable más barato (Cat6) | ⚠️ Cable USB | **CAN** |

**Puntuación:**
- **CAN Bus:** 4 victorias
- **USB:** 6 victorias

---

## 🎯 Recomendación

### Para Este Proyecto: **USB** ✅

**Razones:**

1. **Proyecto didáctico "para novatos"**
   - USB es más simple de entender y configurar
   - Menos curva de aprendizaje
   - Debugging más fácil = menos frustración

2. **Primera impresora Klipper**
   - Mejor empezar con configuración simple
   - CAN se puede migrar después si se desea

3. **Soporte comunidad actual**
   - Voron, RatRig, comunidad Klipper recomiendan USB para toolheads
   - Más recursos y guías disponibles

4. **Mantenimiento futuro**
   - Actualizaciones firmware más fáciles
   - Troubleshooting más directo

5. **Escalabilidad no necesaria (ahora)**
   - Solo 1 toolhead board
   - Si en futuro necesitas múltiples, puedes migrar a CAN

### Casos donde CAN sería mejor:

- Múltiples toolhead boards (IDEX, tool changers)
- Entorno con mucha interferencia EMI
- Distancias muy largas (>5m)
- Experiencia previa con CAN bus
- Arquitectura de impresora compleja

**Ninguno de estos aplica a tu proyecto.**

---

## 📋 Decisión Final (Pendiente Confirmación Usuario)

### Propuesta: **Modo USB**

**Arquitectura:**
```
Host (PC Debian)
├─ USB → SKR 1.4 Turbo (MCU principal)
└─ USB → EBB42 (MCU toolhead)

Cable al toolhead:
├─ Cable USB (comunicación)
└─ Cable 24V + GND (alimentación, 1.5mm²)
```

**Ventajas para tu proyecto:**
- ✅ Configuración simple (novato-friendly)
- ✅ Debugging fácil
- ✅ Comunidad más grande
- ✅ Flasheo trivial
- ✅ Menos puntos de fallo

**Desventajas aceptables:**
- ⚠️ 2 cables físicos vs 1 (mínimo impacto)
- ⚠️ Necesita cable USB de calidad

---

## 🔧 Implementación Modo USB

Si se aprueba USB, Phase 3 cambiará a:

### Material Necesario:
- [ ] Cable USB (longitud: medir toolhead a host)
  - Tipo A (host) a Tipo C o Micro-USB (EBB42)
  - Calidad: Blindado preferible
  - Longitud estimada: 1.5-2m
- [ ] Cable alimentación 24V/GND (1.5mm², ~2m)
- [ ] Termorretráctil para identificación
- [ ] ~~Cat6~~ - NO necesario
- [ ] ~~Resistencias 120Ω~~ - NO necesario

### Pasos Phase 3 (USB):
1. Documentar toolhead stock
2. Montar EBB42 en toolhead
3. Conectar componentes a EBB42
4. Tender cable USB desde toolhead a host
5. Tender cable 24V/GND desde SKR a toolhead
6. Configurar Klipper (modo USB)
7. Flashear firmware EBB42 (modo USB)
8. Verificar comunicación

**Tiempo estimado:** 4-5 horas (más rápido que CAN)

---

## 📚 Referencias

**Documentación oficial:**
- [Klipper Multi-MCU](https://www.klipper3d.org/Multi_MCU_Homing.html)
- [BTT EBB42 GitHub](https://github.com/bigtreetech/EBB)

**Guías comunidad USB:**
- [Voron Design - EBB USB Setup](https://docs.vorondesign.com/build/electrical/v2_ebb_usb.html)
- [Klipper Discourse - USB vs CAN](https://klipper.discourse.group/)

**Guías comunidad CAN:**
- [Voron Design - CAN Bus Setup](https://docs.vorondesign.com/build/electrical/v2_can_bus.html)
- [Klipper CAN Bus Documentation](https://www.klipper3d.org/CANBUS.html)

---

## ✅ Acción Requerida

**Decisión del usuario:**

- [ ] ✅ **APROBAR USB** - Continuar Phase 3 con modo USB
- [ ] ❌ **PREFERIR CAN** - Continuar Phase 3 con modo CAN bus
- [ ] ❓ **MÁS INFO** - Necesito más detalles sobre [especificar]

**Justificación usuario (opcional):**
_____________________________________

---

**Documento creado:** 2025-12-21
**Próxima actualización:** Tras decisión del usuario
**Impacto:** Actualizar `PLANNING.md` según decisión
