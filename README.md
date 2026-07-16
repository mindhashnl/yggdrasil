# 🌳 Yggdrasil - Hardware Infrastructure

[![Hardware: KiCad 8/9](https://img.shields.io/badge/EDA-KiCad_9.0-blue.svg)](https://www.kicad.org/)
[![Form Factor: 3U Server](https://img.shields.io/badge/Form_Factor-19%22_3U-orange.svg)]()
[![Status: Prototype](https://img.shields.io/badge/Status-v2.1_Prototype-success.svg)]()

> **⚠️ NOTE:** This repository contains **hardware design files only** (Schematics, PCB Layouts, Gerber files, BOMs, and Mechanical CAD). 
> The software stack is not yet updated to the new hardware.

## 📖 Overview

Yggdrasil is an open-source, modular hardware-in-the-loop (HIL) testing framework designed for continuous integration (CI/CD) of embedded systems. It allows developers to test firmware automatically on real microcontrollers (MCUs) at scale. 

The physical architecture is designed to fit inside a standard **19-inch 3U server rack** and consists of a primary control board, a multiplexing backplane, and hot-swappable target testing blades.

---

## 🧩 System Architecture

The hardware is split into three main modules:

### 1. Mimir (Compute Controller)
The "brain" of the server. It acts as the bridge between the high-level network and the low-level hardware matrix.
* **Core:** Raspberry Pi Compute Module 5 (CM5).
* **Storage:** M.2 NVMe SSD slot for reliable, high-speed storage.
* **Connectivity:** Gigabit Ethernet (RJ45), USB-A, HDMI (for local debugging).
* **Interface:** Connects to the backplane via a custom-pinned PCIe x8 mechanical connector.

### 2. Bifrost (The Backplane)
The routing matrix that connects Mimir to the testing blades. It contains no "smart" compute chips, only routing and switching logic.
* **Switching:** Uses cascaded `74HC595` and `SN74HCS165` shift registers to control `TMUX1511` analog multiplexers.
* **Buses:** Routes shared JTAG, UART, SPI, I2C, CAN, and GPIO lines to any connected blade.
* **Power:** Accepts 12V input and steps it down to 5V and 3.3V rails for the entire system using TPSM33625 modules.
* **Capacity:** Supports up to 16 daughter boards.

### 3. Valkyrie (Daughter Blades)
The Device-Under-Test (DUT) modules. These are inserted into the front of the server.
* **Form Factor:** 110mm x 170mm (excluding PCIe fingers). Fits in custom HDD-style hot-swap trays.
* **Standardization:** Connects to Bifrost using a custom-pinned PCIe x16 mechanical connector.
* **Included Reference Designs:**
  * **Valkyrie-ESP32S3:** A complete testing blade featuring an ESP32-S3, status LEDs, physical buttons, and a hardware identification EEPROM.
  * **Valkyrie-Test:** A diagnostic board with LEDs mapped to every PCIe lane to visually verify multiplexer switching logic.

---
