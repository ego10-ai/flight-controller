# Flight Controller
---
<img width="1024" height="1024" alt="Untitled (1)" src="https://github.com/user-attachments/assets/4ad1e12b-a57b-48cc-bc39-c8b35e61592a" />

This is a compact, custom-designed flight controller project based on the STM32F405RGT6 (168MHz) architecture. It is designed for high-performance motion tracking and stable flight processing. low-current LDOs for primary power, this board features a dedicated high-voltage power stage capable of stepping down battery voltage (up to 40V).
low-noise motion sensing, and professional-grade data logging.


## Hardware Specifications

core processing
- MCU: STM32F405RGT6 architecture
- Clock: 8MHz External Resonator
- Data Logging : W25Q128JVSIQ 128M-bit flash chips allow for simultaneous high-speed Blackbox recording

Sensors
- IMU: ICM-42688-PC (High-precision 6-axis motion tracking)
- Barometer: DPS310 (For altitude sensing and pressure monitoring)
- Video Overlay : AT7456E OSD chips with its own 27MHz crystal

Power & Connectivity
- Input: USB-C (C16PIN) for programming and power.
- Buck Converter : LMR14030SDDAR step-down regulators.
- Regulation: RT9013-33GB-MS RT9193-33GB (U6) 3.3V Low-Dropout (LDO) regulator.
- Connectors: JST SH 1.0mm for peripheral communication.

## Design Details
- Software Used: EasyEDA
- Component Size: Primarily 0603 SMD for a balance between compact size and hand-solderability.

## Hardware size

![](https://image-pro.easyeda.com/pullimages/d60df64324594c6193174ee3b50a3936.webp)

## schemetic img

![schemetic.webp](https://image-pro.easyeda.com/pullimages/0d0de48dc2ca4391ac6e4eb93fc41a89.webp)
## Getting Started

1. Open the `.epro` file in EasyEDA or compatible software.
2. Review the schematics and PCB layouts in the `production/` folder.
- `flight controller.epro`: Main project file
- `production/`: Production-related files

3.Download and install the STM32CubeProgrammer or the Betaflight Configurator.

4.If using Betaflight, go to the Firmware Flasher tab.

5.Select the correct target for the STM32F405 target processor.

## Contributing

Feel free to contribute improvements or bug fixes.# flight-controller
