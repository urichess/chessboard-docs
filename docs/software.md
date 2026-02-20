# 💻 Software Stack & Systems Engineering

The project is architected as a distributed system, balancing real-time embedded constraints on the board with high-level AI orchestration in the cloud.

---

## 1. ChessStation (Embedded Linux)

The ChessStation runs a custom Linux distribution engineered for low latency and high reliability.

### Operating System: Yocto Project
* **Optimized Kernel:** Tailored to the Raspberry Pi architecture, removing unnecessary drivers to minimize boot time and attack surface.
* **Systemd Integration:** Custom services manage the application lifecycle, ensuring a "boot-to-GUI" experience and robust error recovery.
* **Graphics Stack:** The UI runs **Kivy directly on SDL2**. By bypassing heavy display servers (X11/Wayland), we achieve maximum performance and direct access to the framebuffer.
* **Chess Toolkit:** Pre-installed and cross-compiled tools including **Stockfish** for local engine evaluation and standard chess libraries.

### Application Logic (Python)
* **Connectivity:** Manages OAuth2/Token authentication with **Lichess API** and handles remote game state synchronization.
* **Peripheral Management:** Asynchronous handling of the USB Board connection, processing incoming movement frames and resolving board state discrepancies.
* **Hybrid Analysis:**
    * **Local:** Immediate evaluation using the integrated Stockfish engine for offline use.
    * **Remote:** Offloads complex requests to the Cloud AI for deep strategic insights.

---

## 2. Chessboard Accessory (Firmware)

The sensor module operates on **Zephyr RTOS**, chosen for its modularity and industrial-grade hardware abstraction.

* **Sensor Fusion & Filtering:** Reads 64 analog/digital signals through a multiplexed tree. The firmware implements a digital filter to eliminate magnetic noise and prevent false positives from adjacent squares.
* **Calibration Engine:** Includes a custom calibration routine. Values are processed and stored in **Non-Volatile Storage (Flash)** to ensure sensor accuracy across different environmental conditions.
* **Communication:** Implements a proprietary USB protocol to report board state changes with minimal latency.
* **Developer Interface:** A dedicated **Debug UART** is implemented, allowing real-time monitoring and the execution of debug commands for testing the sensor matrix.

---

## 3. Cloud & AI Ecosystem

The backend provides the heavy-duty intelligence that the embedded hardware cannot process locally.

* **Server Architecture:** High-performance API built with **Python FastAPI** and **Uvicorn**, designed for asynchronous request handling.
* **LangGraph AI Agent:** A stateful agentic workflow that goes beyond simple engine scores:
    * **Natural Language Interaction:** Can discuss games, answer complex chess questions, and explain positional advantages.
    * **Deep Analysis:** Processes full game to identify better plans, errors and blunders.
    * **Current Status:** The ChessStation is currently integrated to receive positional explanations, with full game discussion features in development.
* **Security (Pending):** Implementation of a centralized OAuth2 authentication method for user management and personalized data storage.

---

## 🛠️ Software Design Summary

| Component | Responsibility | Stack |
| :--- | :--- | :--- |
| **ChessStation** | HMI & Synchronization | Yocto, Kivy, SDL2, Stockfish |
| **Board** | Data Acquisition | Zephyr RTOS, C, STM32 HAL |
| **Cloud** | Cognitive Analysis | FastAPI, LangGraph, LLMs |

---

[Back to Main README](../README.md)
