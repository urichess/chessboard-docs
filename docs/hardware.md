# 🛠️ Hardware Architecture & Design

This document details the physical layer of the project, focusing on the modular interface between the **ChessStation** (Processing Hub) and the **USB Board Accessory** (Sensing Layer).

---

## 1. System Overview

The hardware is built on a **decoupled architecture**. The ChessStation acts as a universal host gateway, while the Board Accessory functions as a specialized USB peripheral. 

This separation allows for:
* **Interoperability:** Support for different sensing technologies (Hall Effect, Reed switches, or 3rd-party DGT boards).
* **Reliability:** Real-time sensor scanning is offloaded to a dedicated MCU, preventing OS-level jitter from the Raspberry Pi.



---

## 2. ChessStation (The Brain)

The ChessStation is the central gateway and user interface hub. It handles high-level game logic, Stockfish engine analysis, and internet connectivity.

* **Computing Unit:** Raspberry Pi 2 / 3 (Broadcom BCM2835/BCM2837).
* **Display:** 7-inch IPS Capacitive Touchscreen (1024x600 HD).
    * **Interface:** HDMI for video + USB for touch capacitive feedback.
* **OS Environment:** Custom **Yocto-based Linux** with a **read-only rootfs** to prevent filesystem corruption during hard power-offs.
* **UI Stack:** Optimized **Kivy on SDL2** running directly on the framebuffer for minimum latency.

> ![Chess UI Preview](./assets/display-ui.jpg)
> *Caption: The 7-inch IPS interface providing real-time analysis and game controls.*

---

## 3. USB Board Accessory (Sensing Layer)

The Board Accessory digitizes a physical chessboard using a cost-optimized analog sensor array. While the core logic remains consistent, the physical implementation is being iterated.

### 3.1 Piece Detection & Matrix Scanning
The system utilizes an **8x8 Hall Effect Sensor Matrix**. Each of the 64 squares contains a sensor that detects the magnetic field of magnets embedded in the chess pieces.

* **Scanning Logic:** The MCU cycles through the select lines of **4x 16-channel Multiplexers** (CD74HC4067).
* **Signal Integrity:** A 4-layer PCB design with dedicated ground planes and **RC filters** ensures clean analog-to-digital conversion even with the electromagnetic interference inherent in large matrices.
* **MCU:** STM32 (ARM Cortex-M) handling high-speed scanning and USB-HID/Serial communication.

### 3.2 Evolution of the Accessory
The design has moved from a hand-wired proof-of-concept to a fully integrated large-format PCB.

* **[View Prototype V0 (Proof of Concept)](../accessory/v0/accessory-v0.md):** Details on the initial 8x8 glued sensor matrix and hand-soldered 4-layer STM32/ESP32 board.
* **Future Version (V1):** Integrated 40cm x 40cm PCB combining sensors and logic into a single unit.

---

[Return to Project README](../README.md)
