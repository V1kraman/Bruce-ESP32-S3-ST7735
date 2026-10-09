# Bruce ESP32-S3 ST7735

A custom build of [Bruce firmware](https://github.com/BruceDevices/firmware) adapted for an ESP32-S3 board with a 1.8-inch ST7735 TFT display and five-button navigation.

This repository brings the firmware source, board configuration, wiring references, and hardware photos together so the software and physical build can be understood in one place.

## Contents

- [Project overview](#project-overview)
- [Hardware](#hardware)
- [Repository guide](#repository-guide)
- [Wiring and pinout](#wiring-and-pinout)
- [Build from source](#build-from-source)
- [Upload to the ESP32-S3](#upload-to-the-esp32-s3)
- [Button controls](#button-controls)
- [Troubleshooting](#troubleshooting)
- [Safety and responsible use](#safety-and-responsible-use)
- [Credits and license](#credits-and-license)

## Project overview

Bruce is an open-source firmware project for supported ESP32-based devices. This repository contains a hardware-specific build intended for an ESP32-S3 and a 1.8-inch ST7735 display.

The purpose of this repository is to keep the custom board configuration and firmware source alongside the information needed to wire, build, and work on the device.

> This is a community hardware adaptation, not an official Bruce release.

## Hardware

The core configuration is intended for:

- **Controller:** ESP32-S3 N8R8 development board
- **Display:** 1.8-inch ST7735 TFT
- **Navigation:** five tactile buttons
- **Build system:** PlatformIO

Optional modules should only be considered part of a particular build when they are present in the hardware and wiring documentation. Check the exact module revision and its electrical requirements before connecting it.

## Repository guide

Use this section to find the relevant part of the project.

| Path | Purpose |
|---|---|
| `boards/esp32-s3-st7735/` | Board-specific configuration for the ESP32-S3 and ST7735 build |
| `src/` | Main firmware source code |
| `include/` | Shared header files and declarations |
| `lib/` | Libraries included with the project |
| `platformio.ini` | PlatformIO environments, build settings, and dependencies |
| `custom_8Mb.csv` | Custom flash partition table |
| `wiring-diagram/` | Wiring diagrams and pinout references, if included |
| `images/` | Hardware photos and other project images, if included |
| `installation-guide.txt` | Additional installation notes, if included |

Folder names and files may change as the project develops. The files in the repository are the final reference for the current revision.

## Wiring and pinout

Use the wiring diagrams and pinout references in `wiring-diagram/` when assembling or modifying the device. Make sure the diagram matches the exact board and module revisions you own.

The current five-button GPIO assignments are:

| Button | ESP32-S3 GPIO | Intended function |
|---|---:|---|
| UP | GPIO 9 | Move up |
| SELECT | GPIO 11 | Select / OK |
| LEFT | GPIO 12 | Move left / previous |
| RIGHT | GPIO 13 | Move right / next |
| DOWN | GPIO 14 | Move down |

The buttons are configured as active-low inputs. Confirm the wiring and board configuration before changing any GPIO assignments. Also check for pin reuse, shared buses, and pins reserved by attached peripherals.

If you update a pin in firmware, update the wiring diagram and pinout documentation in the same change.

## Build from source

### Requirements

- ESP32-S3 board matching the project configuration
- USB data cable that supports data
- [Visual Studio Code](https://code.visualstudio.com/)
- [PlatformIO IDE extension](https://platformio.org/install/ide?install=vscode)
- Python 3, if required by the project's build scripts

### Steps

1. Download or clone this repository.
2. Open the project root folder in Visual Studio Code.
3. Install or enable the PlatformIO IDE extension.
4. Allow PlatformIO to install the configured dependencies.
5. Open the PlatformIO terminal and build the project:

   ```bash
   pio run
   ```

Check `platformio.ini` for the configured build environment and board settings. If the project defines multiple environments, use the environment intended for this hardware.

## Upload to the ESP32-S3

Connect the board with a USB data cable, then run:

```bash
pio run --target upload
```

If PlatformIO cannot find the board, check the USB cable, serial port, USB-to-serial driver, and upload settings. Some ESP32-S3 boards require you to enter bootloader mode manually.

To view serial output:

```bash
pio device monitor
```

A successful compile or upload does not, by itself, confirm that every display, button, or optional peripheral is working. Test the actual hardware after flashing.

## Button controls

The intended five-button navigation is:

| Input | Intended behavior |
|---|---|
| UP | Move up |
| DOWN | Move down |
| LEFT | Move left / previous |
| RIGHT | Move right / next |
| SELECT short press | Select / OK |
| SELECT hold (about 700 ms) | Back / Escape |

Screen-specific navigation can differ if a screen handles input events in a special way. Check the main menus and on-screen keyboard separately when testing input changes.

## Troubleshooting

| Problem | Checks |
|---|---|
| Display stays blank | Check display power, ground, SPI wiring, reset, backlight, and board-specific display configuration. |
| Buttons do not respond correctly | Check GPIO assignments, active-low wiring, pull-ups, and input handling. |
| SELECT performs the wrong action | Check short-press versus long-press logic and how the current screen handles Select and Escape. |
| Upload fails | Check the USB data cable, serial port, drivers, and bootloader mode. |
| A peripheral is not detected | Verify its power requirements, pinout, bus wiring, chip-select or address, and firmware configuration. |
| Device resets unexpectedly | Check the power supply, regulator capacity, wiring, and serial logs. |

## Safety and responsible use

Use wireless, NFC/RFID, infrared, and other security-related functions only on devices and systems you own or are explicitly authorised to test. Follow applicable laws and the operating limits of your modules.

Before powering the hardware, verify polarity, voltage levels, current requirements, and battery protection. Do not assume two similar-looking breakout boards have identical pinouts or electrical characteristics.

## Credits and license

This project is based on [Bruce firmware](https://github.com/BruceDevices/firmware).

Preserve the upstream copyright notices and follow the applicable license terms for Bruce and all included third-party libraries when modifying or redistributing this project. Review the upstream repository's license and the licenses of bundled dependencies before publishing a release. Do not imply that upstream code was written from scratch for this adaptation.

- **Bruce project:** https://bruce.computer/
- **Upstream firmware:** https://github.com/BruceDevices/firmware

---

Maintained as a DIY firmware and hardware project. If the firmware or wiring changes, keep the repository documentation in sync.
