# PowerPanel

![3D render, angled](images/pcb/3d_angled.png)

## What it is

A wall-panel power supply board: 230 VAC in, six regulated DC rails out on screw terminals, with an ESP32-S2
web interface that shows voltage, current and power of every rail and drives four PC fans.

| Rail | Source | Rated |
|---|---|---|
| 24 V | Mean Well LRS-150-24 brick, through a TPS16630 eFuse | 2.5 A |
| 48 V | LM5156H boost from the 24 V bus | 1 A |
| 12 V | LMR51450 buck | 3 A |
| 12 V fan | LMR51450 buck, feeds 4 × 4-pin PWM fan headers | 3 A |
| 5 V | LMR51450 buck, TPS2557 current-limited terminal switch | 3 A |
| 3V3 | LMR51450 buck | 2 A |
| 1V6 | buck from 5 V | 1 A |

- **Mains side:** the certified Mean Well LRS-150-24 (150 W) does the isolation; on the PCB there is only the fused,
  fenced mains zone feeding it. Everything after the brick is SELV (≤ 60 V DC).
- **Monitoring:** INA226 per rail (INA237 on 48 V), TMP75 board temperature, all on one I²C bus.
- **MCU:** ESP32-S2-MINI-2, USB-C for flashing (bench power from USB without mains), status LEDs via 74HC595.
- **Board:** 150 × 250 mm, 4 layers, all parts on top, the bottom is the outside face of the device and carries
  only silkscreen art. White mask, black silk. 342 components, 294 of them placed by JLCPCB assembly.

## Repository

| Folder | Content |
|---|---|
| `altium/` | Altium Designer project: `PowerPanel.PrjPcb`, 6 schematic sheets, `PowerPanel.PcbDoc` |
| `fab/` | JLCPCB package: Gerber + drill zip, BOM and CPL (pick and place) in JLC format, parts JLC does not place, order notes |
| `images/schematic/` | PNG of every schematic sheet |
| `images/pcb/` | 3D renders and Gerber renders |
| `images/layers/` | every layer on its own, rendered from the Gerbers |

Open `altium/PowerPanel.PrjPcb` in Altium Designer. The footprints and 3D models are embedded in the PcbDoc; the
schematic symbols are embedded in the SchDocs, so no external library is needed to view the design.

## Schematics

![All schematic sheets](images/schematic/all_schematics.png)

| | |
|---|---|
| ![Top](images/schematic/0_Top.png) | ![Mains, 24 V, 48 V](images/schematic/1_Mains_24V_48V.png) |
| ![Bucks](images/schematic/2_Bucks.png) | ![Low-voltage rails and monitors](images/schematic/3_LowV_Monitors.png) |
| ![MCU and USB](images/schematic/4_MCU_USB.png) | ![Fans, LEDs, IO](images/schematic/5_Fans_LEDs_IO.png) |

## PCB

| Top | Bottom (outside face) |
|---|---|
| ![Top](images/pcb/3d_top.png) | ![Bottom](images/pcb/3d_bottom.png) |
| ![Gerber top](images/pcb/gerber_top.png) | ![Gerber bottom](images/pcb/gerber_bottom.png) |

![Mean Well LRS-150-24 on the board](images/pcb/3d_psu_angled.png)

## Layers

Rendered from the Gerber files. Copper layers are seen from the top; the bottom silkscreen and mask are
mirrored so they read as seen from below.

![All layers](images/layers/all_layers.png)

| L1 top copper | L2 GND plane | L3 power | L4 bottom copper |
|---|---|---|---|
| ![L1](images/layers/L1_top_copper.png) | ![L2](images/layers/L2_inner1_copper_GND.png) | ![L3](images/layers/L3_inner2_copper_power.png) | ![L4](images/layers/L4_bottom_copper.png) |

| Top silkscreen | Bottom silkscreen | Top mask openings | Bottom mask openings |
|---|---|---|---|
| ![Top silk](images/layers/top_silkscreen.png) | ![Bottom silk](images/layers/bottom_silkscreen.png) | ![Top mask](images/layers/top_soldermask_openings.png) | ![Bottom mask](images/layers/bottom_soldermask_openings.png) |

| Top paste | Drill |
|---|---|
| ![Paste](images/layers/top_paste.png) | ![Drill](images/layers/drill.png) |

## Ordering

Upload `fab/PowerPanel_gerbers_2026-10-04.zip` as the PCB, `fab/PowerPanel_BOM_JLC.csv` and
`fab/PowerPanel_CPL_JLC.csv` for assembly (`fab/PowerPanel_BOM_manual.csv` lists the parts JLC does not place).
Board: 4 layers, 1.6 mm, 150 × 250 mm, white mask / black silk, tented vias, no impedance control.

## Safety

This board connects to mains voltage. Build and test it only if you are qualified to work with mains, inside a
closed, earthed enclosure. It is an untested AI-generated design: no warranty, use at your own risk.

## Licence

MIT see [LICENSE](LICENSE).
