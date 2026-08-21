# KNX Taster v1

Custom 6-button KNX wall controller — designed, built, and installed in my own apartment.

**Status:** deployed at home since Summer 2026 · **v2 in development**

---

## The project

Off-the-shelf KNX wall controllers are functional but expensive, and rarely fit the exact button layout or feedback style I wanted. So I built my own — from schematic and PCB layout in KiCAD, through firmware on an STM32, to a fully 3D-printed enclosure and switch frame. It has been the primary KNX switch in my hallway since Summer 2026.

<p align="center">
  <img src="docs/images/01-wall-installed.jpg" alt="KNX Taster v1 installed in the hallway" width="480">
</p>

---

## Design highlights

**Own KNX protocol stack.** Rather than pulling in an off-the-shelf KNX library, the firmware implements the KNX bus stack from scratch on the STM32 — a deliberate learning project to actually understand the protocol layer by layer. ETS integration is currently being developed; migration to an established stack is on the table for v2.

**Reconfigurable button layout.** The same PCB accepts either 6 individual buttons or 3 large rocker-style ("Wippen") caps — mix them as needed. Both configurations use identical Gateron Brown switches; only the 3D-printed caps change. Per-key colour via SK6812MINI addressable RGB LEDs shining through frosted white PLA, with black PLA sections directing the light to the intended cap without bleed between buttons.

**Modular KNX interface.** The KNX bus front-end (NCN5130-based) sits on a small daughterboard that plugs into the main PCB. This kept the bus-facing electronics isolated during development and let the coupler be revised independently — a pattern I now use whenever a design has a risky or reusable subsystem.

**Ambient-adaptive backlight.** An onboard photoresistor measures room brightness so the RGB LEDs adjust themselves in real time — dim enough at night to not light up the room, bright enough during the day to stay visible. Room temperature is measured in parallel via an NTC thermistor and made available on the KNX bus.

**Fully 3D-printed housing and wall frame.** No off-the-shelf switch frame — even the standard-format wall frame is printed, so the whole assembly is one coherent design. Standard European mounting depth, drops into any existing flush-mount box.

**Full firmware development flow.** Onboard SWD debug header, dedicated Reset and Boot0 buttons — no jumpers or bodge wires needed to flash or debug.

---

## Gallery

<p align="center">
  <img src="docs/images/02-front-lit.jpg" alt="Front view with RGB feedback" width="380"><br>
  <sub><i>Front side under RGB feedback — buttons lit through frosted 3D-printed caps</i></sub>
</p>

<p align="center">
  <img src="docs/images/03-side-profile.jpg" alt="Side profile" width="380"><br>
  <sub><i>Enclosure depth — fits a standard European flush-mount box</i></sub>
</p>

<p align="center">
  <img src="docs/images/04-main-pcb-render.png" alt="Main board — 3D render" width="380"><br>
  <sub><i>Main board, KiCAD 9 3D render — MCU, buttons, sensor cluster, coupler socket</i></sub>
</p>

<p align="center">
  <img src="docs/images/05-main-with-ankoppler.jpg" alt="Main board with KNX Ankoppler attached" width="380"><br>
  <sub><i>Assembled main board with the modular KNX Ankoppler plugged into its socket</i></sub>
</p>

<p align="center">
  <img src="docs/images/06-ankoppler-render.png" alt="KNX Ankoppler — 3D render" width="380"><br>
  <sub><i>KNX Ankoppler daughterboard — NCN5130 bus front-end with 8/16 MHz reference</i></sub>
</p>

---

## Hardware overview

### Main board

| | |
|---|---|
| **MCU** | STMicroelectronics STM32G070CBT6 (Cortex-M0+, 48-pin LQFP) |
| **Switches** | 6× Gateron Brown mechanical |
| **Indicators** | 6× SK6812MINI addressable RGB LEDs |
| **Sensors** | 10 kΩ NTC thermistor · GL-series photoresistor |
| **Debug** | SWD header (SWDIO, SWCLK, NRST, 3V3, GND) |
| **Expansion** | 2×10 GPIO breakout |
| **Coupler interface** | 1×7 pin socket for KNX Ankoppler |
| **User controls** | Reset · Boot0 |

### KNX Ankoppler (daughterboard)

| | |
|---|---|
| **Transceiver** | ON Semiconductor NCN5130 — full KNX bus front-end |
| **Reference** | 16 MHz crystal (jumper-selectable 8 / 16 MHz) |
| **Bus protection** | TVS diode (SMA40), series resistor + rectifier |
| **Power out** | Regulated 3.3 V and 5 V to the main board |
| **Bus connector** | 2-pin screw terminal (KNX+, GND) |

### Enclosure

- 3D-printed housing in white PLA
- 3D-printed button caps combining frosted white PLA (for LED diffusion) with black PLA sections (for light directing)
- 3D-printed wall frame in 60 mm format

---

## Firmware

Written in C++ on the STM32G070, developed in **STM32CubeIDE**. The KNX protocol stack is a custom implementation running directly on the MCU, communicating with the KNX bus through the NCN5130 coupler board. The firmware also drives the RGB LEDs, samples the two sensors, and handles button debouncing.

*The firmware source is not published in this repository — this repo documents the project, it is not an open-source release.*

---

## What v2 will change

- **Single-PCB design.** Merge the main board and the KNX Ankoppler into one PCB — smaller footprint, fewer connectors, cleaner build.
- **Improved 3D-printed enclosure and mechanics.** Better fit, cleaner light guides, more repeatable assembly.
- **Software refinements.** Cleaner architecture, potentially migrating to an established KNX stack.
- **Extended functionality.** Additional features on top of what v1 delivers today.
- **Improved user interaction.** Refined feedback and control patterns.

---

## Tooling

- **Schematic + PCB:** KiCAD 9
- **Mechanical / Enclosure:** Fusion 360
- **3D Printing:** Bambu Lab P1S (PLA)
- **Firmware IDE:** STM32CubeIDE

---

## License

This project's documentation is released under the MIT License — see [LICENSE](LICENSE).

Full schematics, PCB source files, firmware source, and CAD files are **not** published in this repository. This is a project showcase, not an open-hardware release.

---

## Author

**Samuel Hoppe** — ISE student @ OTH Regensburg  
[GitHub](https://github.com/samu-studio) · [LinkedIn](https://www.linkedin.com/in/samuel-hoppe-a228aa323/)
