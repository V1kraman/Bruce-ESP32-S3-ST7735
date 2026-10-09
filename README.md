# Hardware Guide

> Hardware reference for the custom **ESP32-S3 + 1.8-inch ST7735 Bruce handheld**.
>
> This document connects the physical build to the board-specific firmware configuration. Keep it in sync with the actual wiring whenever the hardware changes.

## Contents

- [Hardware overview](#hardware-overview)
- [Five-button controls](#five-button-controls)
- [Display and peripheral wiring](#display-and-peripheral-wiring)
- [Power and battery](#power-and-battery)
- [Pin verification workflow](#pin-verification-workflow)
- [Assembly photos and diagrams](#assembly-photos-and-diagrams)
- [Bring-up checklist](#bring-up-checklist)
- [Troubleshooting](#troubleshooting)

---

## Hardware overview

| Part | Role | Notes |
|---|---|---|
| ESP32-S3 N8R8 development board | Main controller | Confirm the exact board revision before wiring. |
| 1.8-inch ST7735 TFT | User interface | Display setup is defined by the board-specific firmware configuration. |
| Five tactile buttons | Navigation and selection | The current board configuration uses active-low button inputs. |
| Optional add-on modules | Depends on the assembled revision | Only treat a module as installed when it is shown in the current wiring diagram and physically present. |

**Board-specific firmware files:** [`boards/esp32-s3-st7735/`](../boards/esp32-s3-st7735/)  
**Build configuration:** [`platformio.ini`](../platformio.ini)

## Five-button controls

The button GPIO assignments below are taken from the current project configuration. The inputs are active-low, so each button should connect its assigned GPIO to **GND when pressed**, with the input configured appropriately by the firmware.

| Button | GPIO | Intended control |
|---|---:|---|
| UP | GPIO 9 | Move up |
| SELECT | GPIO 11 | Short press: Select / OK |
| LEFT | GPIO 12 | Move left / previous |
| RIGHT | GPIO 13 | Move right / next |
| DOWN | GPIO 14 | Move down |

### SELECT long-press

The intended control scheme is:

- **Short SELECT press:** Select / OK.
- **Hold SELECT for approximately 700 ms:** Back / Escape.
- A long hold should generate one Back event, not repeated events or an additional Select event.

The precise behavior can vary by screen if a screen handles navigation events differently. Test the main menus and on-screen keyboard separately after changing input code.

## Display and peripheral wiring

The display and optional peripherals must be wired according to the configuration for the exact board revision. Do not infer connections from generic ESP32-S3 pin aliases or from a diagram for a different breakout board.

| Device / interface | What to document |
|---|---|
| ST7735 TFT | Controller pins, SPI bus, chip select, data/command, reset, backlight, supply voltage |
| SPI peripherals | SCK, MOSI, MISO if used, individual chip-select pins, interrupt/control pins |
| I²C peripherals | SDA, SCL, device address, supply voltage |
| UART peripherals | TX/RX from the perspective of each device, baud rate, supply voltage |
| IR receiver / transmitter | Signal GPIO, driver circuit if required, supply and current limits |
| Battery measurement | Measurement GPIO and the actual divider or sensing circuit, if present |

### Wiring reference

Add your real diagram to `docs/wiring/wiring-overview.png` and link it here:

![Wiring overview](wiring/wiring-overview.png)

If the diagram uses another filename, update the path above to match the committed file.

For a detailed pin-by-pin table, keep a separate file at [`docs/wiring/pinout.md`](wiring/pinout.md). Record the **GPIO number, module pin label, voltage, bus, and the firmware file that configures it**.

## Power and battery

Power arrangements depend on the exact battery, charging board, regulator, and connected peripherals used in your build. Document the actual circuit rather than assuming all similarly named modules have identical protection or pinouts.

Before powering the device:

- Verify battery polarity and the charger board's `B+` / `B-` and output connections.
- Confirm the battery chemistry and the charger configuration are compatible.
- Check the regulator's input range, output voltage, and current rating.
- Confirm every peripheral's permitted supply and logic voltage.
- Avoid short circuits and exposed battery terminals.
- Do not charge a swollen, damaged, leaking, or unusually hot lithium battery.

**Do not connect a raw battery directly to a 3.3 V rail unless the complete circuit is explicitly designed for it.** A nominal battery voltage is not its full charge voltage, and electronics tend to be unimpressed by optimistic assumptions.

## Pin verification workflow

When adding or changing a module, use this process:

1. Identify the exact module and revision.
2. Read its pin labels and electrical requirements.
3. Check the board configuration and source code for existing GPIO assignments.
4. Check for shared buses, reserved pins, and pin conflicts.
5. Update the wiring diagram and pinout table.
6. Build the firmware and test the module independently.
7. Record the tested firmware revision and any known limitations.

### Pinout record template

Copy this table into `docs/wiring/pinout.md` and fill it with verified connections.

| Module | Module pin / signal | ESP32-S3 GPIO | Voltage / notes | Firmware location | Status |
|---|---|---:|---|---|---|
| TFT display | _Fill in from actual wiring_ | _Verify_ | _Verify_ | `boards/esp32-s3-st7735/` | Not documented |
| UP button | Signal | GPIO 9 | Active-low | Board input configuration | Configured |
| SELECT button | Signal | GPIO 11 | Active-low | Board input configuration | Configured |
| LEFT button | Signal | GPIO 12 | Active-low | Board input configuration | Configured |
| RIGHT button | Signal | GPIO 13 | Active-low | Board input configuration | Configured |
| DOWN button | Signal | GPIO 14 | Active-low | Board input configuration | Configured |

Only change a status to **Tested** after checking the real hardware.

## Assembly photos and diagrams

Keep images in the repository so the documentation works directly on GitHub.

Suggested layout:

```text
docs/
└── wiring/
    ├── wiring-overview.png
    ├── pinout.md
    ├── display-wiring.png
    └── power-wiring.png

images/
├── front.jpg
├── back.jpg
├── internals.jpg
└── firmware-menu.jpg
```

Suggested photo set:

- **Front:** display, buttons, and enclosure.
- **Back:** board and enclosure details.
- **Internals:** wiring, connectors, and power circuit.
- **Wiring overview:** readable full-system diagram.
- **Firmware menu:** actual screen output from the current build.

Use photos of the real device. Label diagrams clearly and avoid publishing placeholder images as if they were test evidence.

## Bring-up checklist

- [ ] GPIO assignments match the firmware configuration.
- [ ] Power polarity and voltage have been checked before connecting the board.
- [ ] The TFT starts with the correct orientation and usable colours.
- [ ] UP, DOWN, LEFT, and RIGHT work as intended.
- [ ] A short SELECT press selects an item once.
- [ ] A SELECT hold triggers Back / Escape once.
- [ ] SELECT behavior has been checked on menus and the on-screen keyboard.
- [ ] Each connected peripheral has been tested individually.
- [ ] The committed wiring diagram matches the physical device.
- [ ] The firmware revision used for testing is recorded.

## Troubleshooting

| Symptom | First checks |
|---|---|
| Display stays blank | Verify supply, ground, backlight, reset, SPI wiring, and the display configuration. |
| Buttons act by themselves | Check active-low wiring, pull-ups, shorts, and button GPIO definitions. |
| SELECT triggers the wrong action | Check short/long-press handling and how the current screen consumes Select and Escape events. |
| Peripheral does not respond | Verify supply voltage, shared bus wiring, chip-select, address, and pin configuration. |
| Board resets during use | Check the power source, regulator current capacity, wiring, and serial logs. |

## Documentation maintenance

When hardware changes, update these together in the same commit:

1. Board pin configuration and relevant firmware code.
2. Wiring overview and detailed pinout.
3. Assembly photos if the physical layout changes.
4. This guide's hardware table and test checklist.

A diagram that no longer matches the device is worse than no diagram: it confidently teaches the next person to wire it incorrectly.

---

**Related files:** [Project README](../README.md) · [Board configuration](../boards/esp32-s3-st7735/) · [PlatformIO configuration](../platformio.ini)
