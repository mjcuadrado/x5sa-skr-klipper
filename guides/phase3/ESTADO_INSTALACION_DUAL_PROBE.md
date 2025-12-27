# Estado Actual Phase 3 - Eddy Coil Instalado (Sensor Original Desconectado)

**Proyecto:** Tronxy X5SA Klipper Migration
**Fase:** Phase 3 - INSTALAR Eddy → G28 funcional
**Estado:** Instalación física completada - Iniciando pruebas
**Fecha:** 2025-12-27
**Versión:** 1.1

---

## 🎯 Configuración Actual

### Hardware Instalado

**Probe Principal (Futuro):**
- **Tipo:** BTT Eddy Coil V1.0 (Eddy Current Probe)
- **Ubicación:** Toolhead, montado 2-3mm sobre nozzle
- **Conexión:** I2C via EBB42 (PB3/SCL, PB4/SDA)
- **Cable routing:** Drag chain desde toolhead hasta EBB42 (detrás del frame)
- **Estado:** ✅ Instalado físicamente

**Probe Legacy (Desconectado):**
- **Tipo:** Sensor inductivo original (stock)
- **Ubicación:** Brazo original (montado pero NO desmontado)
- **Estado eléctrico:** ❌ DESCONECTADO (cables desconectados)
- **Razón:** Mantenido físicamente instalado para evitar desmontar brazo
- **Nota:** NO está disponible como fallback - solo Eddy operativo

---

## 📋 Estado de Instalación Phase 3

### Completado ✅
- [x] Eddy Coil montado físicamente en toolhead
- [x] Verificada distancia 2-3mm sobre nozzle
- [x] Cable I2C conectado a EBB42 (VCC, GND, SCL, SDA)
- [x] Cable routing por drag chain
- [x] Documentación fotográfica completada
- [x] Sensor original mantenido instalado

### Pendiente ⏳
- [ ] Calibración inicial Eddy Coil (LDC_CALIBRATE_DRIVE_CURRENT)
- [ ] Configuración Z offset Eddy
- [ ] Pruebas G28 con Eddy
- [ ] Deshabilitar sensor original (cuando Eddy validado)
- [ ] Bed mesh con Eddy (rapid_scan)

---

## 🔧 Configuración Klipper Actual

### Solo Eddy Coil Configurado

**En printer.cfg actual:**

```ini
# SENSOR ORIGINAL - DESCONECTADO
# El sensor inductivo original está físicamente instalado pero DESCONECTADO
# NO está configurado en printer.cfg

# EDDY COIL (ÚNICO PROBE ACTIVO)
[probe_eddy_current btt_eddy]
sensor_type: ldc1612
i2c_address: 42
i2c_bus: i2c1_PB3_PB4  # EBB42 I2C
x_offset: 0.0
y_offset: 25.0  # Ajustar según montaje real
#z_offset: 0.0  # Se calibrará con PROBE_EDDY_CURRENT_CALIBRATE
#reg_drive_current: 15  # Se calibrará con LDC_CALIBRATE_DRIVE_CURRENT
```

**Nota crítica:** El Eddy Coil es el ÚNICO probe conectado y operativo. No hay fallback disponible.

---

## 📖 Estrategia de Calibración

### Calibración Eddy (ÚNICO PROBE)

**El Eddy Coil es el único probe conectado. NO hay sensor de respaldo.**

**Pasos:**
1. ✅ Instalación física completada
2. ⏳ Calibrar drive current (LDC_CALIBRATE_DRIVE_CURRENT)
3. ⏳ Calibrar Z offset (PROBE_EDDY_CURRENT_CALIBRATE)
4. ⏳ Probar G28 con Eddy
5. ⏳ Verificar repetibilidad
6. ⏳ Bed mesh rapid_scan
7. ⏳ Validar para Phase 4

**Sensor original:**
- Permanece físicamente instalado (brazo no desmontado)
- Desconectado eléctricamente
- Puede ser desmontado en el futuro cuando sea conveniente

---

## ⚠️ Importante

**Sensor original DESCONECTADO:**
- ❌ NO hay fallback disponible
- ✅ Solo Eddy Coil operativo
- ✅ Calibración Eddy es crítica (único probe)
- ✅ Verificar repetibilidad cuidadosamente antes de imprimir

**Desmontaje sensor original:**
- Puede hacerse en el futuro cuando sea conveniente
- No es urgente (solo está desconectado, no interfiere)
- Mantenerlo montado evita necesidad de desmontar brazo ahora

---

## 🔗 Documentación Relacionada

- [ARQUITECTURA_PHASE3.md](ARQUITECTURA_PHASE3.md) - Arquitectura completa Phase 3
- [EDDY_COIL_INSTALLATION.md](EDDY_COIL_INSTALLATION.md) - Instalación física Eddy
- [EDDY_COIL_CALIBRATION.md](EDDY_COIL_CALIBRATION.md) - Procedimiento calibración
- [CONFIGURACION_KLIPPER.md](CONFIGURACION_KLIPPER.md) - Configuración completa Klipper

---

**Próximo paso:** Iniciar calibración y pruebas del Eddy Coil con agente de Klipper.
