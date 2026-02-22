# Prototype V0: USB Chessboard Accessory

This document details the first proof-of-concept iteration of the analog Hall effect sensor-based chessboard. The primary goal of this version was to validate the sensor matrix scanning logic and the signal integrity of the 4-layer PCB design while keeping initial manufacturing costs low.

---

## 1. System Architecture
The system utilizes an 8x8 matrix of analog Hall effect sensors to detect the presence and magnetic polarity of chess pieces.

* **Sensor Array:** 64 analog Hall sensors glued directly to a wooden substrate.
* **Signal Routing:** Hand-soldered wiring connects the sensors to four 16-channel multiplexers (also mounted to the wood).
* **Control Logic:** A custom 4-layer STM32-based control board reads the multiplexed analog signals.

### Sensor Matrix Construction
![Wooden sensor bed with glued sensors and multiplexers](./docs/accessory/v0/sensors-matrix-v0.jpeg)
*Figure 1: 8x8 Hall effect sensors and multiplexers manually fixed to the wooden base.*

---

## 2. Hardware Specifications

### Main Control Board
The core of the system is a **4-layer PCB** designed for high signal integrity and noise reduction. 

| Component | Description |
| :--- | :--- |
| **MCU** | STM32 Microcontroller (ARM Cortex-M) |
| **Filtering** | Integrated **RC filters** on analog input lines |
| **Multiplexers** | 4x 16-channel muxes for 64-point scanning |
| **Debug** | Dedicated **UART** output and **SWD** connector |
| **Expansion** | I2C and SPI headers for external peripherals |
| **Storage** | Unpopulated slot for **Flash Memory** (SMD footprint) |
| **Wireless** | **ESP32** module (for WiFi/BT experiments; to be removed) |

### PCB Assembly
![Hand soldered STM32 and ESP32 board](./docs/accessory/v0/stm-board-v0.jpeg)
*Figure 2: Custom 4-layer PCB. All components, including the STM32 and ESP32, were hand-soldered for this prototype.*

---

## 3. Schematic & Design

The board was designed to handle both the high-speed processing of the STM32 and the wireless capabilities of the ESP32.

![Schematic Preview](./docs/accessory/v0/accessory-stm32-v0.png)
[Click here to download the full Electrical Schematic (PDF)](./docs/accessory/v0/accessory-stm32-v0.pdf)

---

## 4. Construction & Assembly Notes

> [!IMPORTANT]
> This iteration was a "hybrid" build intended to prove the electronics before committing to a full-scale 40cm PCB.

* **Manual Assembly:** Sensors were manually positioned and secured with adhesive to the wood frame. 
* **Wiring Loom:** A complex loom of cables connects the wooden sensor bed to the STM32 board.
* **Cost Optimization:** Only the 4-layer STM32 board was professionally manufactured; the sensor grid was hand-wired to save on large-format PCB fabrication costs during the testing phase.

---

## 5. Evaluation & Next Steps

### Achievements
* Successfully read 8x8 matrix via STM32 ADC.
* Validated the RC filter's effectiveness in reducing noise from the long sensor wires.
* Confirmed UART debug protocols for piece detection.

### V1 Roadmap
Now that the concept is proven, the project will move to a **fully integrated 40cm x 40cm PCB**.
* **Integrated Design:** Move all 64 sensors and the STM32 onto a single large PCB.
* **Simplified BOM:** Remove the ESP32 to focus on a streamlined USB-only interface.
* **Manufacturing:** Eliminate manual wiring to improve durability and reduce assembly time.

---
*Created by [Your Name/GitHub Handle]*
