# Phase 1, Step 1: SKR 1.4 Turbo Stock

**Objetivo:** Documentar el estado inicial de la placa SKR 1.4 Turbo antes de cualquier modificación.

**Tiempo estimado:** 10 minutos

---

## 📋 Material Necesario

- [ ] BTT SKR 1.4 Turbo (nueva, en caja)
- [ ] Superficie limpia y despejada
- [ ] Buena iluminación
- [ ] Cámara / smartphone

---

## 📸 Fotos Obligatorias

Debes hacer las siguientes fotos:

- [ ] **Foto 1:** Placa en caja (sin abrir)
- [ ] **Foto 2:** Placa sacada de la caja, vista general superior
- [ ] **Foto 3:** Vista de los 5 zócalos de drivers (vacíos)
- [ ] **Foto 4:** Zona de jumpers debajo de zócalos (sin jumpers)

**Guardar en:** `photos/phase1/`

**Nombre sugerido:** `01_skr14turbo_stock.jpg` (vista general principal)

---

## 📝 Procedimiento

### Paso 1.1: Inspección Visual Inicial

1. Abrir la caja de la SKR 1.4 Turbo
2. Sacar la placa **con cuidado** (evitar tocar componentes electrónicos)
3. Colocar sobre superficie limpia

### Paso 1.2: Verificación de Estado

Confirmar visualmente:

✅ **Zócalos de drivers:**
- 5 zócalos vacíos (X, Y, Z, E0, E1)
- Sin pines doblados
- Sin suciedad u obstrucciones

✅ **Jumpers:**
- No hay jumpers insertados en ningún pin
- Pines limpios y rectos

✅ **Componentes:**
- No hay daños visibles
- No hay soldaduras frías o defectuosas
- Conectores limpios

✅ **Etiquetado:**
- Serigrafía legible (X, Y, Z, E0, E1, HE0, HB, etc.)
- Identificación de modelo visible

### Paso 1.3: Documentación Fotográfica

Hacer las 4 fotos obligatorias listadas arriba.

**Consejo:** Usa buena iluminación natural o luz blanca. Evita sombras y reflejos.

---

## ✅ Validación

Antes de continuar al siguiente paso, verifica:

- [ ] Placa sin daños físicos
- [ ] Todos los zócalos vacíos y limpios
- [ ] No hay jumpers insertados
- [ ] Fotos realizadas y guardadas
- [ ] Placa guardada en superficie antiestática o en su caja

---

## ⚠️ Troubleshooting

### Problema: Zócalo con pines doblados

**Síntoma:** Uno o más pines de un zócalo están doblados

**Solución:**
1. **NO insertes drivers** en ese zócalo
2. Con pinzas de punta fina, endereza suavemente los pines
3. Verifica que queden perfectamente rectos
4. Si no puedes enderezarlos, contacta al vendedor (garantía)

### Problema: Componente suelto o dañado

**Síntoma:** Ves componentes desprendidos o quemados

**Solución:**
1. **NO uses la placa**
2. Documenta con fotos
3. Contacta al vendedor inmediatamente (DOA - Dead On Arrival)

### Problema: Serigrafía ilegible

**Síntoma:** No puedes leer las etiquetas de los conectores

**Solución:**
1. Descarga el [pinout oficial](https://github.com/bigtreetech/BIGTREETECH-SKR-V1.3/blob/master/BTT%20SKR%20V1.4/Hardware/BTT%20SKR%20V1.4PIN.pdf)
2. Imprime y ten a mano para referencia

---

## 📚 Información Adicional

### ¿Por qué documentamos el estado stock?

1. **Referencia base:** Si algo falla más adelante, puedes comparar con el estado inicial
2. **Garantía:** Prueba de que la placa llegó en buen estado
3. **Rollback:** Saber exactamente cómo volver al estado inicial

### Diferencia SKR 1.4 vs 1.4 Turbo

La única diferencia es el MCU:
- **SKR 1.4:** LPC1768 @ 100 MHz
- **SKR 1.4 Turbo:** LPC1769 @ 120 MHz (20% más rápido)

El pinout y layout son idénticos.

---

## 🔗 Referencias

- [SKR V1.4 GitHub](https://github.com/bigtreetech/BIGTREETECH-SKR-V1.3/tree/master/BTT%20SKR%20V1.4)
- [SKR V1.4 Pinout PDF](https://github.com/bigtreetech/BIGTREETECH-SKR-V1.3/blob/master/BTT%20SKR%20V1.4/Hardware/BTT%20SKR%20V1.4PIN.pdf)
- [HARDWARE_REFERENCE.md](../../HARDWARE_REFERENCE.md#btt-skr-14-turbo)

---

## ➡️ Siguiente Paso

Una vez completada la verificación y documentación:

**[Step 2: Configuración Jumpers UART](step2_uart_jumpers.md)**

---

**Estado:** ✅ Completado (2025-12-20)
