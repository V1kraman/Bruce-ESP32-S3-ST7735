<div align="center">
BRUCE · POCKET LAB
A custom ESP32-S3 handheld for Bruce firmware
Firmware · Wiring · Hardware notes · One source of truth
<!-- Add a clear photo of the assembled device at images/hero.jpg -->
<img src="images/hero.jpg" alt="Custom ESP32-S3 Bruce handheld, assembled prototype" width="720">
<p>
  <em>A hardware-specific Bruce firmware build for an ESP32-S3 board with a 1.8-inch ST7735 TFT and five-button navigation.</em>
</p>
Hardware & wiring ·
Build & flash ·
Repository map ·
Safety & legal use
</div>
---
What is this?
This repository documents a custom Bruce firmware build and its matching hardware setup. The goal is to keep the firmware, pin assignments, wiring diagrams, and build photos connected, so someone rebuilding the device can move from what the device does to where each wire goes without hunting through scattered notes.
> **Project rule:** if a pin assignment changes in firmware, update the pinout and wiring documentation in the same commit.
At a glance
Item	Details
Main controller	ESP32-S3, N8R8 configuration
Display	1.8-inch ST7735 TFT
Navigation	Five tactile buttons
Firmware base	Bruce
Build system	PlatformIO
Hardware status	DIY prototype; refer to the photos and wiring documents in this repository
Other modules should be considered part of the build only when they appear in the current wiring documentation and are enabled/configured in the firmware.
✦ The build, in pictures
<!-- Put your own photos in images/ using these filenames, or update the paths below. -->
<table>
  <tr>
    <td align="center" width="50%">
      <img src="images/front.jpg" alt="Front view of the handheld" width="100%">
      <br><strong>01 / Front</strong><br>
      <sub>Display, controls and enclosure</sub>
    </td>
    <td align="center" width="50%">
      <img src="images/back.jpg" alt="Back view of the handheld" width="100%">
      <br><strong>02 / Back</strong><br>
      <sub>Board, wiring and power assembly</sub>
    </td>
  </tr>
  <tr>
    <td align="center" width="50%">
      <img src="docs/wiring/wiring-overview.png" alt="Complete wiring overview" width="100%">
      <br><strong>03 / Wiring map</strong><br>
      <sub>Module-to-controller connections</sub>
    </td>
    <td align="center" width="50%">
      <img src="images/firmware-menu.jpg" alt="Bruce firmware running on the display" width="100%">
      <br><strong>04 / Firmware</strong><br>
      <sub>The software running on the actual hardware</sub>
    </td>
  </tr>
</table>
Image paths are repository conventions. Add your own images at these paths, or edit the paths to match your filenames. Do not use placeholder images as proof that a feature has been tested.
🔌 Hardware & wiring
The wiring documents are the bridge between the physical build and the firmware configuration.
Wiring overview — complete connection diagram.
Pinout reference — GPIO assignments, signal names, voltage notes, and any shared buses.
Hardware photos — actual assembly, module close-ups, and display photos.
Board-specific firmware configuration — board definitions and display/input configuration.
Main firmware source — application and core source code.
Shared headers — project-wide declarations and configuration.
PlatformIO configuration — build environments and compile-time settings.
Partition table — custom flash partition layout.
Pinout is the source of truth
Use `docs/wiring/pinout.md` as the human-readable pin reference. For each connected module, document:
Field	What to record
Module / signal	Exact module and pin label
ESP32-S3 GPIO	GPIO number used by the build
Power	Supply voltage and current considerations
Bus	SPI, I²C, UART, GPIO, or other interface
Firmware location	Relevant board config, header, or source file
Verification	`Planned`, `Wired`, or `Tested`
If a GPIO is shared, reserved, or conditionally used, explain that explicitly. Never infer a physical connection from a generic pin alias alone.
🧰 Build & flash
Requirements
ESP32-S3 board matching the configuration in this repository
USB data cable
Visual Studio Code
PlatformIO IDE extension
Python 3, if required by the project's build scripts
Build
Open the repository root in VS Code, wait for PlatformIO to initialise, then run:
```bash
pio run
```
Upload
Connect the board and run:
```bash
pio run --target upload
```
Serial monitor
```bash
pio device monitor
```
The exact PlatformIO environment, USB port, flash size, and upload settings depend on the board and `platformio.ini`. Check those settings before flashing. A successful compile does not prove every peripheral or hardware feature works.
🎮 Five-button navigation
The current intended control scheme is:
Control	Intended action
UP	Move up
DOWN	Move down
LEFT	Move left / previous
RIGHT	Move right / next
SELECT tap	Select / OK
SELECT hold (~700 ms)	Back / Escape
This table describes the intended behavior. If a screen handles navigation differently, document that exception and verify it on the device before calling it confirmed.
🗂 Repository map
```text
.
├── boards/
│   └── esp32-s3-st7735/    # Board-specific configuration
├── docs/
│   └── wiring/
│       ├── wiring-overview.png
│       └── pinout.md
├── images/
│   ├── hero.jpg
│   ├── front.jpg
│   ├── back.jpg
│   └── firmware-menu.jpg
├── include/                # Shared headers
├── lib/                    # Project libraries
├── src/                    # Firmware source
├── custom_8Mb.csv           # Flash partition table
├── platformio.ini           # PlatformIO build configuration
├── LICENSE
└── README.md
```
`docs/wiring/` and `images/` are documentation folders to create if they do not exist yet. Keep images reasonably sized; a clear 1200–1600 px photo is usually plenty for a GitHub README.
🧪 Validation checklist
Use this list for each hardware or firmware revision:
[ ] Firmware builds successfully with the documented PlatformIO environment.
[ ] Firmware uploads and boots on the target board.
[ ] Display orientation, colours, and layout are correct.
[ ] All five physical buttons behave as documented.
[ ] SELECT tap and long-press do not trigger both actions.
[ ] Every connected module is tested individually.
[ ] GPIO assignments match `docs/wiring/pinout.md`.
[ ] Photos show the actual revision being documented.
[ ] Known limitations are recorded below.
Known limitations
Add confirmed limitations here. Keep reproducible bugs separate from unverified suspicions.
None documented yet. Update this section as testing produces confirmed results.
⚠️ Safety & legal use
Bruce includes functionality that can interact with wireless, radio, infrared, NFC/RFID, and other systems. Use it only with devices and networks you own or are explicitly authorised to test. Follow local laws and the operating limits of your modules.
Before powering the assembly, verify polarity, voltage levels, current requirements, battery protection, and the pinout of the exact module revision you own. Similar-looking breakout boards do not always share the same wiring or electrical requirements.
📜 License & upstream credits
This project is based on Bruce firmware, which is distributed under the GNU Affero General Public License v3.0 (AGPL-3.0). Because this repository is a modified Bruce build, preserve the upstream copyright notices and license obligations, and review the licenses of bundled libraries and other third-party code before publishing.
Include the full applicable `LICENSE` text in the repository. Do not replace upstream notices with a claim that all code was written from scratch. Add your own attribution for original changes, wiring diagrams, photographs, and documentation, and clearly label which work is yours versus inherited.
Upstream Bruce: https://github.com/BruceDevices/firmware
Bruce project site: https://bruce.computer/
This repository is a community hardware adaptation, not an official Bruce release unless the maintainers explicitly say otherwise.
---
<div align="center">
Built, wired, tested, documented.
<sub>If the hardware changes, update the diagram. If the firmware changes, update the pinout. Keep both honest.</sub>
</div>

