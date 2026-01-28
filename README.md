# OMOTE - Open Universal Remote - Hardware

![](images/OMOTE_assembled.jpg)

## Overview

OMOTE is an ESP32-based open-source universal remote. Its 2.8" capacitive touchscreen provides an intuitive and responsive user interface for switching devices and settings. No hub or docking station is required, as the remote features infrared, Wi‑Fi, and Bluetooth connectivity. With its well-optimized power consumption, OMOTE can run for months on a single charge. Because the design files are open source, you can fully customize them to your devices and needs.

<div align="center">
  <img src="images/GUI_sliding_demo.gif" width="50%">
</div>

### Features
- 2.8" 320×240 px capacitive touchscreen
- Ergonomic, fully 3D-printed case
- Built-in infrared, Wi‑Fi, and Bluetooth
- Press any button or simply lift the remote to wake it
- Up to six months of battery life using a 2000 mAh Li‑Po battery

### The state of this project

The hardware for OMOTE is designed to be easily replicated using 3D-printed parts, a two-layer PCB, and commonly available components. The mechanical and PCB design can be considered mostly complete, though there may be areas for improvement—for example, the IR range could be further optimized.

### This fork of OMOTE

This fork has a few differences:
* The vias on the ESP32 have been enlarged to help reduce manufacturing cost.
* A BOM (`omote.csv`) for Digi‑Key has been included to help source parts.

### Errata

⚠️
The most significant issue is that the battery terminal connector is reversed compared to shipped lithium‑ion battery packs. I'm not sure if this is a standard; it could have caused serious damage if I hadn't noticed. ⚠️⚡

The current board and BOM have the following issues:
1. The vias under the ESP32 are not connected to ground; this will be modified manually.
1. The USB TVS diodes listed in the [BOM](https://github.com/rochuck/OMOTE-Hardware/blob/main/PCB/BOM.csv) are the wrong size. For now, I've removed the part and used 0 Ω resistors.
1. The CH340C was unavailable from Digi‑Key; a CH340G module was used instead. The CH340G requires an external crystal, which was taken from its adapter board.
1. The Li‑ion charger is unavailable from Digi‑Key. An MCP73831T2ACI/OT (SOT‑23‑5) is being used temporarily until the correct part can be sourced from AliExpress.

These changes are shown here:

<div align="center">
  <img src="images/bodge.png" width="50%">
</div>

## Charger changes

As shown in the diagrams below:

### TP4056
* On the TP4056, the TEMP pin is not used and is grounded.
* The 4.7 kΩ resistor sets the charge current to approximately 300 mA.
* The STANDBY pin goes low when not charging; the LED indicates charging.
* The CHRG pin is pulled low when charging; this is read by the microcontroller.

### MCP73831
* The STAT pin serves as the charging indicator and could be connected to the microcontroller, but it will require a pull-up. We will leave this disconnected for now.
* The PROG resistor at 4.7 kΩ results in a slightly lower charge current — approximately 213 mA at 4.2 V.

### The upshot

| Function | TP4056 pin | MCP pin |
|---|---:|---:|
| VCC | 4 | 4 |
| GND | 3 | 2 |
| PROG | 2 | 5 |
| BATT | 5 | 3 |

<div align="center">
  <img src="images/existing-li.png" width="25%"><img src="images/new-li.png" width="75%">
</div>

<div align="center">
  <img src="images/charger.png" width="50%">
</div>

### Building the hardware

The central component of OMOTE is its PCB. If you want to build the PCB yourself, you will need SMT-reflow tools such as a hot plate or a hot-air station. The two-layer board and a solder paste stencil can be ordered from any PCB manufacturer using the [KiCad files](https://github.com/OMOTE-Community/OMOTE-Hardware/tree/main/PCB). Manufacturers like OSHPark or Aisler will accept these files directly. For JLCPCB or PCBWay, you can use their plugin to export optimized Gerber files. A [zip archive](https://github.com/OMOTE-Community/OMOTE-Hardware/blob/main/PCB/production/gerber.zip) with these Gerber files is also included in this repository. You can also choose to order assembled PCBs from JLCPCB using the [instructions](https://github.com/OMOTE-Community/OMOTE-Hardware/wiki/How-to-order-assembled-PCBs) in the Wiki.

The electrical components can be sourced from LCSC, but most should also be available from the usual suppliers such as Digi‑Key or Mouser. See the [BOM](https://github.com/OMOTE-Community/OMOTE-Hardware/blob/main/PCB/BOM.csv) for all necessary components.

The project uses a 2000 mAh Li‑Ion battery with a JST‑PHR‑2 connector. Any 3.7 V Li‑Ion battery that fits within 50×34×10 mm should work. From revision 4 onward, the board includes battery protection features against overcurrent and undervoltage. It's still a good idea to use a battery with integrated protection (usually visible as a small PCB under Kapton tape between the battery cables).

The 2.8" capacitive touchscreen can be sourced from Adafruit ([2770](https://www.adafruit.com/product/2770)). If you search for the part number CH280QV10‑CT, you can also buy this display directly from the manufacturer via [Alibaba](https://www.alibaba.com/product-detail/High-Quality-240-3-rgb-320_1600408828330.html). Shipping from China is expensive, so this only makes sense if you order multiple displays. In general, the cost for a single OMOTE is fairly high. Please join the [Discord server](https://discord.gg/5PnYFAsKsG) and then check the [buy-sell page](https://discord.com/channels/1138116475559882852/1153343867681243279) to see if you can share the cost of PCBs and components with others.

<div align="center">
  <img src="images/OMOTE_parts.jpg" width="80%">
</div>

The [housing and buttons](https://github.com/OMOTE-Community/OMOTE-Hardware/tree/main/CAD) can be printed using PLA or PETG. The parts shown in the project photos were sliced in PrusaSlicer with a layer height of 0.25 mm and printed using ColorFabb PETG. It is important that the case is printed with its flat side toward the print bed using adequate support structures. If your printer is well calibrated, the cover plate will snap onto the case.

### Firmware
The firmware is available at [OMOTE ESP32 Firmware](https://github.com/OMOTE-Community/OMOTE-Firmware/)

## Contributing

If you have a suggestion for an improvement, please fork the repo and create a pull request. You can also open an issue, or for more general feature requests, head over to the [discussions](https://github.com/OMOTE-Community/OMOTE-Hardware/discussions).

## License

Distributed under the GPL v3 License. See [LICENSE](https://github.com/OMOTE-Community/OMOTE-Hardware/blob/main/LICENSE) for more information.

## Contact

[![OMOTE Discord](https://discordapp.com/api/guilds/1138116475559882852/widget.png?style=banner2 "OMOTE Discord")][link1]

Join the OMOTE Discord: [https://discord.gg/5PnYFAsKsG](https://discord.gg/5PnYFAsKsG)

Maximilian Kern - [kernm.de](https://kernm.de)

Project page on Hackaday.io: [https://hackaday.io/project/191752-omote-diy-universal-remote](https://hackaday.io/project/191752-omote-diy-universal-remote)

[link1]: https://discord.gg/5PnYFAsKsG
