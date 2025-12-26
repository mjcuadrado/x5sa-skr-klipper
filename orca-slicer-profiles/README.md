# OrcaSlicer Profiles for Tronxy X5SA with Klipper

Comprehensive OrcaSlicer configuration profiles for Tronxy X5SA CoreXY printer running Klipper firmware with SKR 1.4 Turbo + EBB42 CAN V1.2 (USB mode).

## Hardware Configuration

- **Printer**: Tronxy X5SA CoreXY
- **Build Volume**: 330×330×400mm
- **Main MCU**: BTT SKR 1.4 Turbo (LPC1769)
- **Toolhead MCU**: BTT EBB42 CAN V1.2 (STM32G0B1) via USB
- **Drivers**: 5× TMC2209 UART
- **Probe**: BIGTREETECH Eddy Coil V1.0 (rapid_scan capable)
- **Extruder**: Direct drive Titan clone (Phase 3-11) → Orbiter v2 (Phase 12+)
- **Max Velocity**: 300 mm/s
- **Max Acceleration**: 3000 mm/s²

## Profile Files

### Printer Profile
- `printer_tronxy_x5sa_klipper.json` - Main printer configuration with Klipper integration

### Filament Profiles
- `filament_pla.json` - Generic PLA (210-215°C, 60°C bed)
- `filament_petg.json` - Generic PETG (240-245°C, 80-85°C bed)
- `filament_abs.json` - Generic ABS (250-255°C, 100-105°C bed, **requires enclosure**)

### Print Profiles (Process)
- `process_draft.json` - **0.30mm Draft** - Fast prints, lower quality
- `process_standard.json` - **0.20mm Standard** - Balanced quality/speed
- `process_fine.json` - **0.10mm Fine** - High detail, slower prints

## Installation Instructions

### Method 1: Import Individual Profiles (Recommended)

1. **Open OrcaSlicer**
2. **Import Printer Profile**:
   - Go to: `Printer` tab → Click the gear icon → `Import`
   - Select: `printer_tronxy_x5sa_klipper.json`
   - The printer will be added to your printer list

3. **Import Filament Profiles**:
   - Go to: `Filament` tab → Click the gear icon → `Import`
   - Select each filament profile (PLA, PETG, ABS)
   - Repeat for all three filament types

4. **Import Process Profiles**:
   - Go to: `Process` tab → Click the gear icon → `Import`
   - Select each process profile (Draft, Standard, Fine)
   - Repeat for all three quality levels

### Method 2: Manual Profile Directory (Advanced)

1. **Locate OrcaSlicer Config Directory**:
   - **macOS**: `~/Library/Application Support/OrcaSlicer/user/`
   - **Windows**: `%APPDATA%\OrcaSlicer\user\`
   - **Linux**: `~/.config/OrcaSlicer/user/`

2. **Copy Profiles**:
   ```bash
   # Navigate to OrcaSlicer user directory
   cd ~/Library/Application\ Support/OrcaSlicer/user/

   # Create directories if they don't exist
   mkdir -p Tronxy/machine Tronxy/filament Tronxy/process

   # Copy printer profile
   cp /path/to/printer_tronxy_x5sa_klipper.json Tronxy/machine/

   # Copy filament profiles
   cp /path/to/filament_*.json Tronxy/filament/

   # Copy process profiles
   cp /path/to/process_*.json Tronxy/process/
   ```

3. **Restart OrcaSlicer**

## Key Features

### Klipper Integration

#### Start G-code
The printer profile uses this start sequence:
```gcode
M104 S0 ; Stops OrcaSlicer from sending temp waits separately
M140 S0
PRINT_START BED_TEMP=[bed_temperature_initial_layer_single] EXTRUDER_TEMP=[nozzle_temperature_initial_layer]
```

Your Klipper `PRINT_START` macro will:
- Heat bed and extruder
- Home all axes
- Level dual Z steppers (Z_TILT_ADJUST)
- Generate adaptive bed mesh with rapid_scan
- Purge nozzle
- Begin printing

#### End G-code
```gcode
PRINT_END
```

Your Klipper `PRINT_END` macro will:
- Park toolhead
- Turn off heaters
- Disable steppers
- Clear bed mesh

### Firmware Retraction

All profiles are configured to use **Klipper firmware retraction** (G10/G11):
- **PLA**: 1.0mm @ 40mm/s
- **PETG**: 1.2mm @ 35mm/s
- **ABS**: 1.0mm @ 40mm/s

OrcaSlicer will send `G10` (retract) and `G11` (unretract) commands, but Klipper controls the actual retraction parameters.

**To tune retraction in Klipper**:
```bash
# SSH into your printer
ssh pi@printer.local

# Update retraction settings (example for PETG)
SET_RETRACTION RETRACT_LENGTH=1.2 RETRACT_SPEED=35 UNRETRACT_SPEED=35

# Test with retraction tower
# Then update firmware_retraction section in printer.cfg
```

### Pressure Advance

Each filament profile includes pressure advance comments in the start G-code:
- **PLA**: Start with `SET_PRESSURE_ADVANCE ADVANCE=0.04`
- **PETG**: Start with `SET_PRESSURE_ADVANCE ADVANCE=0.055`
- **ABS**: Start with `SET_PRESSURE_ADVANCE ADVANCE=0.045`

**To calibrate pressure advance**:
1. Use Klipper's calibration guide: https://www.klipper3d.org/Pressure_Advance.html
2. Or use Ellis' calibration pattern: https://ellis3dp.com/Print-Tuning-Guide/articles/pressure_advance.html
3. Update the value in your filament's start G-code or add to `printer.cfg`

### Adaptive Bed Meshing

The `PRINT_START` macro uses adaptive bed meshing with rapid_scan:
```gcode
BED_MESH_CALIBRATE METHOD=rapid_scan ADAPTIVE=1
```

This feature:
- Only probes the print area + 5mm margin (not entire bed)
- Uses fast rapid_scan mode with Eddy Coil probe
- Completes in ~15 seconds for typical prints
- Automatically adjusts mesh to print size

## Profile Details

### Draft Profile (0.30mm)
**Best for**: Prototypes, test fits, large functional parts

| Setting | Value |
|---------|-------|
| Layer Height | 0.30mm |
| Wall Loops | 2 |
| Top/Bottom Layers | 4/3 |
| Infill Density | 15% |
| Outer Wall Speed | 80 mm/s |
| Inner Wall Speed | 100 mm/s |
| Infill Speed | 150 mm/s |
| **Print Time** | ~30-40% faster than Standard |

### Standard Profile (0.20mm)
**Best for**: General purpose prints, good balance of quality and speed

| Setting | Value |
|---------|-------|
| Layer Height | 0.20mm |
| Wall Loops | 3 |
| Top/Bottom Layers | 5/4 |
| Infill Density | 15% |
| Outer Wall Speed | 60 mm/s |
| Inner Wall Speed | 80 mm/s |
| Infill Speed | 120 mm/s |
| **Print Time** | Baseline |

### Fine Profile (0.10mm)
**Best for**: Detailed miniatures, smooth surfaces, showcase pieces

| Setting | Value |
|---------|-------|
| Layer Height | 0.10mm |
| Wall Loops | 4 |
| Top/Bottom Layers | 7/6 |
| Infill Density | 15% |
| Outer Wall Speed | 50 mm/s |
| Inner Wall Speed | 60 mm/s |
| Infill Speed | 100 mm/s |
| **Print Time** | ~2-3x longer than Standard |

## Filament-Specific Tips

### PLA
- **Cooling**: Aggressive (100% fan after layer 3)
- **Bed Adhesion**: Good on most surfaces
- **Issues**: Stringing? Increase retraction or lower temp 5°C
- **Storage**: Keep dry, but less sensitive than PETG/ABS

### PETG
- **Cooling**: Minimal (20-50% max to prevent warping)
- **Bed Adhesion**: Excellent (use glue stick or PEI)
- **Issues**: Stringing common - tune retraction and pressure advance
- **Storage**: MUST be dry - use desiccant storage
- **Tip**: Let bed cool before removing parts (PETG bonds strongly)

### ABS
- **Enclosure**: **REQUIRED** or draft-free environment
- **Cooling**: Very minimal (0-30% max)
- **Ventilation**: **REQUIRED** - ABS emits styrene fumes
- **Bed Adhesion**: Critical - use ABS slurry or glue stick
- **Warping Prevention**:
  - Keep enclosure closed during print
  - Let parts cool slowly in enclosure
  - Use brim for large parts
- **Post-processing**: Can be acetone vapor smoothed

## Calibration Checklist

Before your first print, ensure these calibrations are complete:

- [ ] **PID Tuning**: Run `PID_TUNE_HOTEND` and `PID_TUNE_BED`
- [ ] **E-steps**: Calibrate extruder rotation_distance
- [ ] **Probe Z-offset**: Run `PROBE_EDDY_CURRENT_CALIBRATE CHIP=btt_eddy`
- [ ] **Bed Mesh**: Generate with `GENERATE_BED_MESH`
- [ ] **First Layer**: Fine-tune Z-offset with paper test
- [ ] **Pressure Advance**: Calibrate per filament type
- [ ] **Retraction**: Test and tune if stringing occurs
- [ ] **Flow Rate**: Consider flow calibration cube per brand

## Multicolor Support (Phase 12)

These profiles are **prepared for future multicolor capability** when you upgrade to Orbiter v2 extruder in Phase 12.

Current configuration:
- Single extruder profiles
- Prime tower settings: **disabled** (enable for multicolor)

When upgrading to multicolor:
1. Update extruder configuration in Klipper (see printer.cfg Phase 12 notes)
2. In OrcaSlicer printer settings:
   - Add additional extruders
   - Enable prime tower
   - Configure tool change settings
3. Create per-extruder filament profiles
4. Update `PRINT_START` macro for tool preheating

## Troubleshooting

### First Layer Issues

**Problem**: Nozzle too close (squishing)
```bash
# Increase Z-offset by 0.05mm
SET_GCODE_OFFSET Z_ADJUST=0.05 MOVE=1
```

**Problem**: Nozzle too far (not adhering)
```bash
# Decrease Z-offset by 0.05mm
SET_GCODE_OFFSET Z_ADJUST=-0.05 MOVE=1
```

**Permanent fix**: Update `z_offset` in `[probe_eddy_current btt_eddy]` section of printer.cfg

### Stringing

1. **Lower temperature**: Try 5°C cooler
2. **Increase retraction**:
   ```bash
   SET_RETRACTION RETRACT_LENGTH=1.5 RETRACT_SPEED=40
   ```
3. **Tune pressure advance**: May need higher value
4. **Dry filament**: Moisture causes stringing (especially PETG/ABS)

### Poor Overhangs

1. **Increase cooling**: Especially for PLA
2. **Reduce overhang speeds**: Already configured in profiles
3. **Enable support**: For angles >60°
4. **Use support interface**: Better surface finish

### Layer Shifting

1. **Check belt tension**: Should be guitar-string tight
2. **Verify motor currents**: Check TMC2209 run_current values
3. **Reduce acceleration**: Lower max_accel in printer.cfg temporarily
4. **Check sensorless homing**: May need SGTHRS tuning

### Bed Adhesion Problems

**PLA**:
- Clean bed with IPA
- Ensure first layer squish is correct
- Try glue stick for large parts

**PETG**:
- Must use glue stick or PEI sheet
- Ensure bed is at 85°C for first layer
- Don't remove parts until bed cools

**ABS**:
- CRITICAL: Use ABS slurry or heavy glue stick
- Bed must be 105°C for first layer
- Use brim or raft for large parts
- Ensure enclosure is closed

## Advanced Features

### Arc Fitting (G2/G3)
Currently **disabled** in profiles. To enable:
- In process settings: `enable_arc_fitting: 1`
- Klipper supports G2/G3 via `[gcode_arcs]` (already configured)
- Reduces G-code file size and may improve print quality

### Exclude Objects
**Enabled** in all profiles. Allows canceling individual objects during multi-object prints:
- OrcaSlicer labels each object in G-code
- Use Mainsail/Fluidd to exclude failed objects mid-print
- Continue printing remaining objects

### Ironing
Currently **disabled**. To enable for ultra-smooth top surfaces:
- In process settings: Set `ironing_type: "top surfaces"`
- Adds extra pass over top layers
- Increases print time ~10-20%
- Best for flat decorative pieces

## Firmware References

Your Klipper configuration includes these macros (already integrated in profiles):
- `PRINT_START` - Called by start G-code
- `PRINT_END` - Called by end G-code
- `PAUSE` - Triggered by M600 or pause button
- `RESUME` - Resume after pause
- `CANCEL_PRINT` - Cancel current print

Calibration macros (run manually):
- `PID_TUNE_HOTEND TARGET=200`
- `PID_TUNE_BED TARGET=60`
- `GENERATE_BED_MESH`
- `CALIBRATE_EXTRUDER`
- `PRESSURE_ADVANCE_CALIBRATION`
- `RETRACTION_CALIBRATION`

## Resources

### Klipper Documentation
- Official Klipper Docs: https://www.klipper3d.org/
- Pressure Advance: https://www.klipper3d.org/Pressure_Advance.html
- Eddy Probe: https://www.klipper3d.org/Eddy_Probe.html

### Calibration Guides
- Ellis' Print Tuning Guide: https://ellis3dp.com/Print-Tuning-Guide/
- Teaching Tech Calibration: https://teachingtechyt.github.io/calibration.html

### OrcaSlicer
- Official Wiki: https://github.com/SoftFever/OrcaSlicer/wiki
- Profile Management: https://www.obico.io/blog/orcaslicer-comprehensive-profile-management-guide/

## Support

For issues specific to:
- **OrcaSlicer**: Check GitHub issues or OrcaSlicer Wiki
- **Klipper Configuration**: See your printer.cfg and Klipper docs
- **Hardware**: Consult BTT documentation for SKR/EBB42

## Changelog

### Version 1.0 (2025-12-26)
- Initial release
- Printer profile with Klipper integration
- 3 filament profiles (PLA, PETG, ABS)
- 3 process profiles (Draft, Standard, Fine)
- Firmware retraction enabled
- Adaptive bed mesh support
- Pressure advance configuration
- Phase 12 multicolor preparation

## License

These profiles are provided as-is for use with Tronxy X5SA printers running Klipper firmware. Modify as needed for your specific setup.

---

**Created**: 2025-12-26
**For**: Tronxy X5SA + SKR 1.4 Turbo + EBB42 CAN V1.2
**Firmware**: Klipper
**Slicer**: OrcaSlicer 1.9.0+
