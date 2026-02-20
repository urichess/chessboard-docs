# ♟️ Modular Physical-to-Online Chess Ecosystem

A modular platform designed to bridge the gap between physical and digital chess. 
It enables users to play, analyze, and train on online servers using a physical board, providing a distraction-free "unplugged" experience.
It also maintains the power of cloud-based AI analysis.

---

## 🏗️ Architecture Overview

The system is architected into three independent, decoupled modules to ensure scalability and hardware flexibility.


### 1. ChessStation (The Brain)
An embedded gateway based on **Yocto Project** that acts as the central intelligence hub.
* **Core:** Python application running on a custom, minimal Linux distribution optimized for **Raspberry Pi (2 & 3)**. It includes local engine support (Stockfish) for offline analysis.
* **Scalability:** Built using Yocto/OpenEmbedded to ensure hardware-agnosticism, allowing seamless migration to other SoCs (e.g., i.MX6/8) with minimal BSP changes.
* **Workflow:** Manages real-time game state validation, move detection, and synchronization with online chess server APIs.
* **Broad Compatibility:** Supports industry-standard **DGT (USB)** boards and the custom low-cost board accessory.

### 2. USB Board Accessory (The Hardware)
A cost-effective sensor module (approx. 200€ vs 600€ for commercial alternatives) designed to digitize any wooden or plastic competition board.
* **Sensing:** 8x8 Hall effect sensor matrix for piece detection via magnetic induction.
* **Firmware:** Built on **Zephyr RTOS**, providing a robust Hardware Abstraction Layer (HAL). Currently supports **STM32L4**, **STM32L0**, and **Arduino Uno R4**.
* **Bootloader:** Integrated **U-Boot** for reliable system boot management.
* **Hardware Stack:** 4x multiplexers controlled by an STM32 MCU, transmitting data via a proprietary serial protocol over USB.

### 3. Cloud Analysis Server
A backend infrastructure designed for data persistence and advanced computational analysis.
* **AI Engine:** Analysis API developed with **Python** and **LangGraph** to manage complex, stateful AI-driven analysis workflows.
* **Business Logic:** Architecture ready for user authentication, game history persistence, and Elo tracking (SaaS-ready).

---
## 🛠️ Engineering Decisions & "The Why"

* **Graphics Optimization (Kivy on SDL2):** I chose to run Kivy directly on top of SDL2 to minimize memory footprint and CPU overhead. This ensures a fluid 60fps UI even on a Raspberry Pi 2, which is critical for a "premium" feel.
* **Modular Hardware/Software Decoupling:** I intentionally separated the *ChessStation* (gateway) from the *Board Accessory* (sensors). While a dedicated Kernel Driver (`/dev/chessboard`) was an alternative, this modular approach allows the system to remain compatible with 3rd-party hardware (DGT) while simplifying hardware maintenance and iterations.
* **RTOS Selection (Zephyr):** Choosing Zephyr for the sensor module ensures a modern, thread-safe environment. Its superior HAL makes the project resilient to supply chain shifts, as switching MCUs requires only minor configuration changes rather than a full code rewrite.
* **Industrial Linux Standard (Yocto):** Avoiding generic distros (like Raspbian) in favor of Yocto allows for a professional, read-only, and reproducible build system. This ensures the *ChessStation* is treated as an appliance rather than a general-purpose PC.

---

## 📖 Deep Dive Documentation

* [**Hardware & EE Design**](docs/hardware.md): Schematics, Hall effect matrix design, and PCB photos.
* [**Software & UI Architecture**](docs/software.md): Yocto recipes, Kivy/SDL2 implementation, and LangGraph logic.

---

## 🔧 Tech Stack

| Layer | Technologies |
| :--- | :--- |
| **Operating Systems** | Yocto Project (Linux), Zephyr RTOS |
| **Graphics / UI** | Kivy, SDL2 (Direct Framebuffer) |
| **Languages** | Python, C, C++ |
| **Hardware** | STM32 (L0/L4), Raspberry Pi, 7" IPS Touchscreen, Hall Sensors |
| **AI & Backend** | LangGraph, Python API |

---

## 🚀 Development Roadmap
- [ ] Implement USB/OTA Firmware Update (DFU) for the board accessory.
- [ ] Database integration for game history and user profiles.
- [ ] DGT Boards full support
---

## 🔧 Tech Stack

| Layer | Technologies |
| :--- | :--- |
| **Operating Systems** | Yocto Project (Linux), Zephyr RTOS |
| **Languages** | Python, C, C++ |
| **Hardware** | STM32 (L0/L4), Raspberry Pi, Hall Effect Sensors, Multiplexers |
| **AI & Backend** | LangGraph, Python API, AI Analysis Engines |
| **Bootloaders** | U-Boot |
