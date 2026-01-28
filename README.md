# Minima Node & Bass OS Setup Guide 🚀

This repository provides a step-by-step walkthrough for deploying a **Minima Node** on a Raspberry Pi and integrating it with an Android-based environment via **Bass OS**.

---

## 📋 Table of Contents
1. [Hardware Requirements](#1-hardware-requirements)
2. [Raspberry Pi Base Setup](#2-raspberry-pi-base-setup)
3. [Minima Node Installation](#3-minima-node-installation)
4. [Minima RPC Configuration](#4-minima-rpc-configuration)
5. [Android Setup (Bass OS)](#5-android-setup-bass-os)
6. [Network & Ports](#6-network--ports)

---

## 1. Hardware Requirements
* **Device:** Raspberry Pi 4 or 5 (4GB RAM minimum recommended).
* **Storage:** 2x High-speed microSD cards (32GB+). 
  * *Card A: For the headless Minima Node.*
  * *Card B: For the Bass OS (Android) interface.*
* **Connectivity:** Ethernet cable or stable 2.4/5GHz Wi-Fi.

---

## 2. Raspberry Pi Base Setup
To ensure the node runs efficiently, we use a 64-bit Lite environment.

1. **Flash OS:** Use [Raspberry Pi Imager](https://www.raspberrypi.com/software/) to flash **Raspberry Pi OS Lite (64-bit)**.
2. **Pre-configure:** Use `Ctrl+Shift+X` in the imager to:
   * Enable **SSH**.
   * Set a hostname (e.g., `minima-pi`).
   * Configure Wi-Fi.
3. **Install Dependencies:**
   Connect via terminal: `ssh pi@minima-pi.local`
   ```bash
   sudo apt update && sudo apt upgrade -y
   sudo apt install openjdk-17-jre-headless -y
