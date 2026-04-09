# HAWK-3 Flight Controller
---

<img width="2253" height="1229" alt="BG_render" src="https://github.com/user-attachments/assets/fbfa04eb-ea2d-4dc6-9317-175983b1920a" />

This is a compact, custom-designed flight controller project based on the STM32H743VIT6 microcontroller architecture. It is designed for high-performance motion tracking and stable flight processing. low-current LDOs for primary power, this board features a dedicated high-voltage power stage capable of stepping down battery voltage (up to 40V).
low-noise motion sensing, and professional-grade data logging.

This revision transitions the core architecture from legacy STM32F405/F412 microcontrollers to the high-performance STM32H743VIT6. The Hawk 3 focuses on minimizing processing latency, providing exceptionally clean power to sensitive RF equipment, and ensuring long-term compatibility with advanced filtering algorithms.

The Hawk 3 is built around a standard 30x30mm mounting pattern (36x36mm total PCB dimensions) to ensure broad compatibility with existing airframes.

## The architecture of the Hawk 3 was heavily influenced by recent shifts in Betaflight's hardware recommendations, focusing heavily on processing power, latency reduction, and delivering the cleanest possible power for high-end FPV systems.(make sure to check out realese V3)

## Due this new Beta-flight policy (Betaflight is opensource firmware for drone community)

> [!WARNING]
> ### **Betaflight Hardware Design Policy Update**
>
> Betaflight will **no longer accept** new flight controller designs using **STM32 F4 and F7 series** microcontrollers for boards requiring more than **4 motor outputs**.
>
> #### **Why this change?**
> We strive to provide the best flight experience across a wide range of hardware. To maintain this standard, we must limit designs using older microcontrollers that suffer from restrictive **timer and DMA resource** limitations.
>
> #### **Design Recommendations**
> * **STM32 F4/F7:** Should now be reserved strictly for **budget freestyle/racing designs** and **AIOs** limited to 4 motor outputs.
> * **STM32 H743:** With pricing now comparable to the F405, the **H743** is the recommended choice for high-performance, high motor output, and high I/O designs.
>
> #### **Legacy Support**
> * **Existing designs** are not affected by this change.
> * **Future Note:** Features may be restricted on older hardware in future firmware releases if performance overhead becomes a bottleneck.

## Power Delivery Network (PDN) Simulation result (TI WEBENCH Simulations)
This  simulation results for the dual-stage Power Delivery Network (PDN) of the Hawk 3 flight controller. 

The simulations were conducted using the Texas Instruments WEBENCH Power Designer to validate the performance, stability, and startup characteristics of the TPS54335A buck converters utilized for the 5V logic and 9V video power rails.

The objective of these simulations is to ensure the power system can reliably step down raw LiPo battery voltage (8V to 28V) while handling peak current loads from the STM32H743VIT6 MCU, flight peripherals, and sensitive RF video equipment without voltage sag or instability.

Simulation Environment:-
- Tool: TI WEBENCH Power Designer
- Core Component: TPS54335ADDAR (Synchronous Step-Down Converter)
- Output Current Rating: 3.0 A (per rail)

  
### 1. 5V Logic Rail Simulation (Design 1) 
- The 5V rail is responsible for powering the core logic of the flight controller, including the STM32H743VIT6, ICM-42688-P gyro, AT7456E OSD, and the radio receiver.
<img width="1228" height="416" alt="image" src="https://github.com/user-attachments/assets/0039fef2-8a26-4f04-ba2c-47692e258b9e" />

SImualtion Parameters & results
- Input Voltage Range (Vin): 8.0V - 28.0V
- Target Output (Vout): 5.00V
- Target Current (Iout): 3.0A
- Startup Time: 1.80 ms
- Vout Average (Simulated): 4.99 V
- Inductor Max Current: 3.85 A

<img width="1225" height="387" alt="image" src="https://github.com/user-attachments/assets/69ddb42a-ee98-471d-90c5-a41608a6e132" />

<img width="1076" height="757" alt="image" src="https://github.com/user-attachments/assets/7a5e990a-4510-4658-9188-ef63c4ba1a5c" />

<img width="1046" height="739" alt="image" src="https://github.com/user-attachments/assets/f63f63b7-d5ad-4033-90de-8079995ca529" />

### Analysis
The simulation confirms that the calculated feedback resistor network (R1 = 52.3k Ohm, R2 = 10k Ohm) produces a highly accurate and stable output of 4.99V. The startup time is extremely fast at 1.80 milliseconds, ensuring the MCU and gyro initialize instantly upon battery connection. The maximum inductor current peaks at 3.85A during startup, which provides a safe margin above the continuous 3A rating, ensuring the inductor will not saturate during initial power-on surges.


### 2. 9V Video Rail Simulation (Design 2)
   he 9V rail is a completely isolated power system dedicated solely to the Video Transmitter (VTX) and the FPV camera. This isolation is critical to prevent electrical noise generated by the motors and ESCs from introducing interference into the analog video feed.
   <img width="1024" height="320" alt="image" src="https://github.com/user-attachments/assets/2844ffa4-8a22-4108-8898-65b8d9ca2350" />

Simulation Parameters & Results:-

- Input Voltage Range (Vin): 12.0V - 28.0V
- Target Output (Vout): 9.00V
- Target Current (Iout): 3.0A
- Startup Time: 1.80 ms
- Vout Average (Simulated): 8.99 V
- Inductor Max Current: 3.85 A

<img width="744" height="794" alt="image" src="https://github.com/user-attachments/assets/6616bd1d-4ffd-4f33-8842-c9b57240a148" />
<img width="736" height="615" alt="image" src="https://github.com/user-attachments/assets/37500468-d103-48e8-8df6-aa621b086fc2" />



### Analysis
Using the updated feedback calculation (R1 = 100k Ohm, R2 = 9.76k Ohm), the 9V rail regulates perfectly at 8.99V. Like the 5V rail, it achieves full regulation in 1.80 ms. This fast initialization ensures the VTX receives stable power before attempting to transmit, preventing initial boot-up static or channel drift. The inductor handles the peak inrush current smoothly at 3.85A.



## Hardware Specifications

core processing
- MCU: STM32H743VIT6 architecture
- Clock: 8MHz External Resonator
- Data Logging : W25Q256JVEIQ 32MB Flash (Dedicated SPI2 Bus for Blackbox logging)

Sensors
- IMU: ICM-42688-PC (High-precision 6-axis motion tracking)
- Barometer: BMP388
- Video Overlay : AT7456E OSD chips with its own 27MHz crystal (Analog On-Screen Display)

Power Delivery Network (PDN) & Connectivity
- Input: USB-C (C16PIN) for programming and power.
- 5V Logic Rail: TPS54335A Buck Converter (R1 = 52.3k Ohm, R2 = 10k Ohm). Powers the MCU, Gyro, OSD, and Receiver.
- 9V Video Rail: TPS54335A Buck Converter (R1 = 100k Ohm, R2 = 9.76k Ohm). Powers the Video Transmitter (VTX) and FPV Camera exclusively.
- Regulation: RT9013-33GB-MS RT9193-33GB (U6) 3.3V Low-Dropout (LDO) regulator.
- Input Protection: Integrated TVS Diode on the VBAT input to clamp active-braking voltage spikes.
- Connectors: JST SH 1.0mm for peripheral communication.

## Design Details
- Software Used: EasyEDA
- Component Size: Primarily 0603 SMD for a balance between compact size and hand-solderability.

## Hardware size

![](https://image-pro.easyeda.com/pullimages/d60df64324594c6193174ee3b50a3936.webp)

## schemetic img
V1
![schemetic.webp](https://image-pro.easyeda.com/pullimages/0d0de48dc2ca4391ac6e4eb93fc41a89.webp)
V2
![](https://image-pro.easyeda.com/pullimages/0d0de48dc2ca4391ac6e4eb93fc41a89.webp)
v3
1[](https://image-pro.easyeda.com/pullimages/16aaaf0372ce4537be50f0459a6b3f1c.webp)

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
