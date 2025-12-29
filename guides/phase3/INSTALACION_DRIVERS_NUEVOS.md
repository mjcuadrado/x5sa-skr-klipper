# Instalación y Testing de Drivers TMC2209 Nuevos

**Proyecto:** Tronxy X5SA Klipper Migration
**Fase:** Phase 3 - Troubleshooting
**Fecha:** 2025-12-29

---

## 🎯 Objetivo

Instalar y probar drivers TMC2209 nuevos de forma SEGURA, verificando cada paso para evitar daños.

---

## 📦 Drivers a instalar

**Cantidad:** 4 drivers TMC2209
**Ubicación:** Sockets X, Y, E0, E1 en SKR 1.4 Turbo
**Versión recomendada:** TMC2209 v1.2 (evitar v1.3 si es posible)

---

## ⚠️ IMPORTANTE - Antes de empezar

**NO instales drivers sin leer este documento completo.**

Errores comunes que DAÑAN drivers:
- ❌ Instalar con alimentación conectada
- ❌ Invertir orientación del driver
- ❌ No doblar pin UART
- ❌ Cortocircuito por jumpers mal colocados
- ❌ Vref en 0 (aunque no daña, no funcionará)

---

## 🔧 Herramientas necesarias

- [ ] Destornillador pequeño de precisión (plano o Phillips)
- [ ] Multímetro digital
- [ ] Pinzas de punta fina (para doblar pin UART)
- [ ] Luz buena (para ver claramente los pines)
- [ ] Paciencia y sin prisas

---

## 📋 PROCEDIMIENTO DE INSTALACIÓN

### Paso 0: Preparación (CRÍTICO)

```
1. APAGA impresora (interruptor 24V OFF)
2. DESCONECTA cable USB de la SKR
3. ESPERA 60 segundos (capacitores se descargan)
4. Verifica con multímetro que NO hay voltaje
```

**⚠️ NUNCA instales/quites drivers con alimentación conectada.**

---

### Paso 1: Quitar drivers viejos

```
1. Identifica los 4 drivers actuales en sockets X, Y, E0, E1
2. Agarra el driver por los bordes (NO toques componentes)
3. Tira suavemente hacia ARRIBA con movimiento recto
4. Guarda los drivers viejos en bolsa antiestática
```

**Nota:** Socket Z debe quedar VACÍO (no lo usamos).

---

### Paso 2: Configurar jumpers MS (UART mode)

**Para cada socket (X, Y, E0, E1):**

```
Debajo de cada socket hay 3 pares de pines: [MS0] [MS1] [MS2]

Configuración para UART:
- MS0: SIN jumper (vacío)
- MS1: SIN jumper (vacío)
- MS2: CON jumper (instalado)

Total: 1 jumper por socket (solo en MS2)
```

**Verificación visual:**
```
[MS0] [MS1] [MS2]
  ○     ○     ●    ← Correcto para UART

○ = vacío
● = jumper instalado
```

---

### Paso 3: Preparar drivers nuevos

**Para CADA driver nuevo:**

#### 3A. Verificar orientación

```
El driver tiene una marca de orientación:
- Punto pequeño en una esquina
- O texto indicando "1" o similar

Vista correcta:
┌─────────────────┐
│ ●  TMC2209      │  ← Punto en esta esquina
│                 │
│   [Chip]        │
│                 │
└─────────────────┘
  │││││││││││││││
  Pines hacia abajo
```

**Orientación en SKR 1.4 Turbo:**
- Potenciómetro Vref hacia el CENTRO de la placa
- Pin 1 (marcado) hacia esquina exterior

#### 3B. Doblar pin UART

```
1. Identifica el pin UART (marcado en el driver)
   - Generalmente segundo pin desde la izquierda
   - Consulta schematic del driver si no estás seguro

2. Con pinzas de punta fina:
   - Agarra el pin cerca de la base
   - Dóblalo SUAVEMENTE 90° hacia ARRIBA
   - NO lo rompas (usar fuerza mínima)

3. Pin doblado NO debe tocar nada cuando insertas el driver
```

**Vista lateral después de doblar:**
```
     ┌──  ← Pin UART doblado hacia arriba
     │
[Driver]
  │││││││  ← Resto de pines normales
```

#### 3C. Ajustar Vref (CRÍTICO)

```
1. ANTES de instalar el driver
2. Con destornillador de precisión
3. Gira potenciómetro 3-4 vueltas hacia la DERECHA (⟳)
4. NO al máximo (puede dañarse)
5. Empezamos con Vref medio-alto
```

**Por qué:** Drivers nuevos a veces vienen con Vref en 0.

---

### Paso 4: Instalar drivers

**Para CADA driver (X, Y, E0, E1):**

```
1. Verifica orientación (punto hacia esquina correcta)
2. Verifica pin UART doblado hacia arriba
3. Alinea pines con socket
4. Presiona SUAVEMENTE hacia abajo
5. Debe encajar completamente (no forzar)
6. Pin UART NO debe estar insertado (está doblado arriba)
```

**Verificación visual después de instalar:**
```
Vista superior:
┌─────────────────┐
│   [Potenciómetro]  ← Hacia centro placa
│      ┌──  ← Pin UART visible arriba
│      │
│   [Driver]
└─────────────────┘
```

---

### Paso 5: Verificación visual final

```
Antes de conectar alimentación, verifica:

✓ 4 drivers instalados (X, Y, E0, E1)
✓ Socket Z VACÍO
✓ Orientación correcta (potenciómetro hacia centro)
✓ Pin UART doblado hacia arriba en cada uno
✓ Drivers encajados completamente
✓ Jumpers: Solo MS2 en cada socket
✓ No hay cables sueltos
✓ No hay objetos metálicos sobre la placa
```

---

### Paso 6: Primera conexión (SIN motores)

```
1. NO conectes motores todavía
2. Conecta USB a la SKR
3. Enciende 24V
4. Observa si:
   - ¿Hay humo? → APAGA INMEDIATAMENTE
   - ¿Olor a quemado? → APAGA INMEDIATAMENTE
   - ¿Drivers muy calientes al tacto? → APAGA INMEDIATAMENTE
   - ¿Todo normal? → Continúa
```

**Si todo OK, deja encendido 30 segundos y observa.**

---

### Paso 7: Testing Klipper (SIN motores)

```
1. En Mainsail/Fluidd:
   FIRMWARE_RESTART

2. Verifica log de Klipper:
   - ¿Errores UART? → Anota cuál driver
   - ¿Todo OK? → Continúa

3. Test de comunicación UART:
   DUMP_TMC STEPPER=stepper_x
   DUMP_TMC STEPPER=stepper_y
   DUMP_TMC STEPPER=stepper_z
   DUMP_TMC STEPPER=stepper_z1
```

**Resultado esperado:**
```
✓ Todos los comandos muestran datos del driver
✓ Sin errores "Unable to read tmc uart"
✓ GCONF register se lee correctamente
```

**Si hay errores UART:** Verifica pin UART doblado, orientación driver.

---

### Paso 8: Conectar motores

```
Solo si Paso 7 fue exitoso:

1. APAGA impresora (24V OFF + USB desconectado)
2. Conecta cables de motores:
   - X motor → Socket X
   - Y motor → Socket Y
   - Z motor (izquierdo) → E1-CLS (conector verde)
   - Z1 motor (derecho) → E0-CLS (conector verde)

3. Verifica cables bien conectados (no sueltos)
```

**Nota:** Extruder va en EBB42, no en SKR.

---

### Paso 9: Testing con motores

```
1. Conecta USB
2. Enciende 24V
3. FIRMWARE_RESTART

4. Verifica motores tienen FUERZA:
   - Intenta mover motores a mano
   - ¿Resisten? → ✓ Drivers energizando correctamente
   - ¿Blandos? → Aumentar Vref más

5. Test básico de movimiento:
   STEPPER_BUZZ STEPPER=stepper_x
   STEPPER_BUZZ STEPPER=stepper_y
   STEPPER_BUZZ STEPPER=stepper_z
   STEPPER_BUZZ STEPPER=stepper_z1
```

**Resultado esperado:**
- ✓ Motor se mueve/vibra
- ✓ Sin errores UART
- ✓ Motor tiene fuerza antes y después

---

### Paso 10: Homing completo

```
Solo si Paso 9 fue exitoso:

1. Verifica toolhead en posición segura (centro, Z alto)

2. Home individual (más seguro):
   G28 Z    # Home Z primero
   G28 X    # Luego X
   G28 Y    # Luego Y

3. Si todo OK, home completo:
   G28      # Home XYZ
```

**Observa:**
- Sensorless homing X/Y debe funcionar sin crash
- Z debe detenerse en endstop físico
- Sin errores UART durante movimiento

---

## ✅ Checklist de verificación final

Antes de dar por completada la instalación:

- [ ] DUMP_TMC funciona en todos los drivers
- [ ] Motores tienen fuerza de retención
- [ ] STEPPER_BUZZ mueve todos los motores
- [ ] G28 funciona sin errores
- [ ] Sensorless homing X/Y funciona
- [ ] Z homing en endstop funciona
- [ ] Sin errores UART en klippy.log
- [ ] Drivers no se calientan excesivamente (<60°C al tacto)
- [ ] Todo funciona como esperado

---

## 🔧 Ajuste fino de Vref (opcional)

Si motores funcionan pero quieres optimizar:

**Síntomas de Vref muy bajo:**
- Motores pierden pasos
- Ruido/vibración excesiva
- Poco torque

**Síntomas de Vref muy alto:**
- Drivers muy calientes (>60°C)
- Motores muy calientes
- Consumo excesivo

**Ajuste:**
```
1. Con impresora encendida y motores en idle
2. Drivers deben estar tibios (~40-50°C)
3. Si muy fríos: Aumentar Vref (girar derecha)
4. Si muy calientes: Reducir Vref (girar izquierda)
5. Ajustes pequeños (1/4 vuelta a la vez)
```

---

## 🚨 Troubleshooting

### Problema: Error UART después de instalar

**Posibles causas:**
- Pin UART no doblado correctamente (está insertado en socket)
- Driver mal orientado
- Jumpers MS incorrectos

**Solución:**
1. Apaga todo
2. Verifica orientación driver
3. Verifica pin UART doblado hacia arriba
4. Verifica jumpers (solo MS2)

---

### Problema: Motores blandos (sin fuerza)

**Posibles causas:**
- Vref en 0 o muy bajo
- Driver no energizando

**Solución:**
1. Ajustar Vref: Girar potenciómetro 2-3 vueltas más a la derecha
2. FIRMWARE_RESTART
3. Verificar si ahora tienen fuerza

---

### Problema: Driver se calienta mucho

**Posibles causas:**
- Vref muy alto
- Motor en cortocircuito
- Driver defectuoso

**Solución:**
1. Apaga inmediatamente
2. Reduce Vref girando izquierda
3. Verifica motor no tiene cable en corto
4. Si sigue caliente, driver puede estar dañado

---

### Problema: Motor gira al revés

**NO es un problema, es configuración:**

**Solución:**
```
En printer.cfg, invierte dir_pin:

Si tiene !:   dir_pin: !P2.6  →  dir_pin: P2.6
Si no tiene:  dir_pin: P2.6   →  dir_pin: !P2.6
```

---

## 📞 Contacto para dudas

**Si algo no funciona:**
1. NO fuerces nada
2. Apaga todo
3. Anota exactamente qué error ves
4. Toma fotos si es necesario
5. Consulta antes de continuar

---

## ✅ Éxito confirmado cuando:

🎉 **Todos los motores funcionan con fuerza**
🎉 **G28 completa sin errores**
🎉 **DUMP_TMC lee correctamente todos los drivers**
🎉 **Sin errores UART en klippy.log**

**→ Impresora lista para continuar con Phase 4 (Calibración)**

---

**Creado:** 2025-12-29
**Versión:** 1.0
**Estado:** Listo para usar cuando lleguen drivers nuevos
