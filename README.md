# PiNet Quick Start Guide 🚀

This guide walks you through setting up a Raspberry Pi as a Minima PiNet node and connecting to it from an Android environment.

---

## Table of Contents
1. [Hardware Requirements](#1-hardware-requirements)
2. [Prepare Raspberry Pi OS](#2-prepare-raspberry-pi-os)
3. [Install and Run Minima](#3-install-and-run-minima)
4. [Verify RPC Access](#4-verify-rpc-access)
5. [Android Setup (Bass OS)](#5-android-setup-bass-os)
6. [Network Ports](#6-network-ports)
7. [Troubleshooting](#7-troubleshooting)

---

## 1. Hardware Requirements
- **Raspberry Pi:** Pi 4 or Pi 5 (4GB RAM or higher recommended)
- **Storage:** Two microSD cards (32GB+ recommended)
  - **Card A:** Raspberry Pi OS + Minima node
  - **Card B:** Bass OS (Android environment)
- **Network:** Ethernet or stable Wi-Fi

---

## 2. Prepare Raspberry Pi OS
1. Use [Raspberry Pi Imager](https://www.raspberrypi.com/software/) to flash **Raspberry Pi OS Lite (64-bit)** to Card A.
2. In Imager advanced settings (`Ctrl+Shift+X`), configure:
   - SSH enabled
   - Hostname (example: `minima-pi`)
   - Wi-Fi credentials (if needed)
3. Boot the Pi and connect:

```bash
ssh pi@minima-pi.local
```

4. Update packages and install Java:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install openjdk-17-jre-headless -y
```

---

## 3. Install and Run Minima
Download the latest Minima JAR and start the node:

```bash
wget -O minima.jar https://github.com/minima-global/Minima/releases/latest/download/minima.jar
java -jar minima.jar -mdsenable -mdspassword YOUR_PASSWORD -port 9001
```

> Replace `YOUR_PASSWORD` with a strong password and never store real passwords in version control.

To keep Minima running after disconnecting SSH, use `screen`, `tmux`, or a `systemd` service.

---

## 4. Verify RPC Access
Minima RPC is used by external apps to query your node.

- **Default RPC port:** `9005`
- Test from another device on the same network:

```bash
curl http://<YOUR_PI_IP>:9005/status
```

---

## 5. Android Setup (Bass OS)
### Step 1: Flash Bass OS
1. Open the [Bliss Co-Labs Bass OS project](https://sourceforge.net/projects/bliss-co-labs/files/BlissBass/) and navigate to the latest `BlissBass` release folder.
2. Download the Raspberry Pi image file for your device model.
3. Flash Card B.
4. Boot Raspberry Pi with Card B.

### Step 2: Install Minima App and Connect
1. Download the latest Minima APK from [minima.global](https://minima.global).
2. Install the APK (allow unknown sources if prompted).
3. In the app settings, set:
   - Node IP = your Pi node IP
   - Port = `9005`

---

## 6. Network Ports
Allow these ports on your local firewall/router:

| Port | Purpose | Protocol |
| --- | --- | --- |
| `9001` | Minima P2P | TCP |
| `9002` | Minima MDS | TCP |
| `9005` | Minima RPC | TCP |

---

## 7. Troubleshooting
- **Java errors:** confirm Java 17+ with `java -version`.
- **RPC connection refused:** verify IP, port, and same subnet.
- **Poor performance/overheating:** use heatsink/fan and stable power supply.
