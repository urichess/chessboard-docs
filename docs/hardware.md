# 🛠️ Hardware Architecture & Design

This document details the physical layer of the project, covering the **ChessStation** (Processing Hub) and the **USB Board Accessory** (Sensing Layer).

---

## 1. System Overview

The hardware is designed to be modular. The **ChessStation** acts as the host gateway, while the **Board Accessory** acts as a specialized USB peripheral. This decoupling allows the station to support 3rd-party boards (e.g., DGT) and simplifies the iteration of the sensing matrix hardware.



---

## 2. ChessStation (The Brain)

The ChessStation is the central gateway that handles high-level logic, internet connectivity, and the chess engine interface.

* **Computing Unit:** Raspberry Pi 2 / 3 (Broadcom BCM2835/BCM2837).
* **Power Management:** 5V/2.5A DC input via Micro-USB.
* **Storage:** Industrial-grade microSD with a **read-only rootfs** (configured via Yocto) to prevent filesystem corruption during hard power-offs.

> **[PLACEHOLDER: Insert photo of your ChessStation/Raspberry Pi setup here]**
> *Caption: The ChessStation enclosure housing the Raspberry Pi and custom I/O shielding.*

---

## 3. USB Board Accessory (Sensing Layer)

This custom module digitizes a standard chess board using a cost-optimized sensor array.

### 3.1 Piece Detection & Matrix Scanning
The system uses a **Hall Effect Sensor Matrix**. Each of the 64 squares contains a sensor that detects the magnetic field of neodymium magnets embedded in the chess pieces.

* **Sensors:** 64x Hall Effect sensors (Digital/Unipolar).
* **Multiplexing Architecture:** To manage 64 inputs with limited MCU pins, the design utilizes **4x 16-channel Multiplexers** (CD74HC4067). 
* **Scanning Logic:** The STM32 cycles through the select lines of the multiplexers to sample each square. This creates a serialized "bit-map" of the board's current state.

### 3.2 Electrical Schematic (EE)
The schematic focuses on signal integrity and minimizing power consumption of the sensor array.

> **[PLACEHOLDER: Insert your Electrical Schematic/Circuit Diagram here]**
> *Caption: Schematic showing the STM32 pinout, the 4-multiplexer tree, and the 8x8 Hall sensor grid wiring.*

### 3.3 Microcontroller (MCU)
The board is controlled by an **STM32 (L4 or L0 series)**, chosen for its excellent power-to-performance ratio and native USB support.
* **Firmware:** Zephyr RTOS manages the scanning threads and the USB HID/Serial stack.
* **Portability:** The hardware abstraction layer allows the same firmware to run on STM32L476, STM32L0, or Arduino Uno R4 with minimal configuration changes.

> **[PLACEHOLDER: Insert photo of the physical Board Accessory / Sensor Matrix here]**
> *Caption: View of the 8x8 sensor matrix integrated into the board's underside.*

---

## 4. Technical Challenges & Solutions

### Magnetic Cross-talk
**Problem:** Neodymium magnets from adjacent squares could trigger neighboring sensors.
**Solution:** Fine-tuning the sensor threshold in the firmware and implementing a "debouncing" algorithm that requires a stable signal for a set number of scan cycles before confirming a state change.

### Connectivity & Power
By using a dedicated MCU for the board, we offload the real-time scanning tasks from the Raspberry Pi. This ensures that the ChessStation's CPU is fully available for AI analysis and network handling, while the board operates as a low-power "dumb" peripheral until a move is detected.

---

## 5. Bill of Materials (BoM) - Highlights

| Component | Description | Purpose |
| :--- | :--- | :--- |
| **Raspberry Pi** | 2 / 3 B+ | Main CPU / Gateway |
| **STM32L4** | ARM Cortex-M4 | Sensor Controller |
| **A3144** | Hall Effect Sensors (x64) | Piece Detection |
| **CD74HC4067** | 16-Ch Analog Mux (x4) | Matrix Scanning |

---

[Back to Main README](../README.md)
