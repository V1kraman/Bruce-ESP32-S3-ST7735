# Build Log

## 2026-09-07

Project started. The `viniciushnf/ESP32-S3-Bruce-ST7735-1.8` project was selected as the firmware/hardware starting point for a five-button ESP32-S3 handheld.

## Prototype integration period

The prototype was developed incrementally rather than wiring every peripheral at once. The display and buttons were brought up first, followed by radio/NFC/GPS integration and finally the portable power and IR subsystems.

The firmware was rebuilt repeatedly while GPIO assignments were reconciled with the actual ESP32-S3 N8R8 board.

## 2026-09-27

Breadboard prototype completed. The merged ESP32-S3 firmware binary was successfully produced. Core peripheral operation was validated and the project was considered ready for the next physical stage.

### Final build notes

- Merged binary: `Bruce-esp32-s3-st7735.bin`
- Approximate final firmware image size: 4.29 MB
- Build completed successfully.
- Remaining refinement: battery percentage sensing/calibration.

### Warnings observed during build

- Duplicate `HAS_5_BUTTONS` definition.
- Duplicate `BTN_ALIAS` definition.
- NimBLE deprecated API warnings.
- Several compiler warnings involving conversions/unused values.
- `mykeyboard.cpp` implicit `this` capture warning.

These warnings did not prevent generation of the final ESP32-S3 binary.
