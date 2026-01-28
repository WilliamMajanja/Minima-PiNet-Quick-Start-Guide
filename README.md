Here is the comprehensive **README.md** file for your project. I have structured it for clarity, including code blocks for easy copying and a visual breakdown of the network architecture.

---

```markdown
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

```

---

## 3. Minima Node Installation

Download and launch the official Minima Java executable.

```bash
# Download the latest JAR
wget -O minima.jar [https://github.com/minima-global/Minima/releases/latest/download/minima.jar](https://github.com/minima-global/Minima/releases/latest/download/minima.jar)

# Start the node (Replace 'YOUR_PASSWORD' with a secure choice)
java -jar minima.jar -mdsenable -mdspassword YOUR_PASSWORD -port 9001

```

> **Note:** To keep the node running after closing the SSH session, consider using `screen` or creating a `systemd` service.

---

## 4. Minima RPC Configuration

The RPC (Remote Procedure Call) interface allows external applications (like your Android device) to communicate with the node.

* **Default RPC Port:** `9005`
* **Connectivity Test:** Run this from another device on the same network to verify the node is responding:
```bash
curl http://<YOUR_PI_IP>:9005/status

```



---

## 5. Android Setup (Bass OS)

Bass OS allows you to run a full Android environment on your Pi hardware, perfect for the Minima Android APK.

### Step 1: Flash Bass OS

1. Download the latest image from the [Bliss Bass Project](https://sourceforge.net/projects/bliss-co-labs/files/BlissBass/).
2. Flash it to your **second** SD card.
3. Boot the Pi. You will now be in an Android-based UI.

### Step 2: Install & Connect Minima

1. Open the browser in Bass OS and download the **Minima APK** from [minima.global](https://minima.global).
2. Install the APK (allow "Install from Unknown Sources").
3. **Connect to Node:** * Open the app and navigate to **Settings**.
* Enter the IP address of your Raspberry Pi Node and port `9005` to sync your Incentive ID and account data.



---

## 6. Network & Ports

Ensure your router or firewall does not block the following ports:

| Port | Function | Protocol |
| --- | --- | --- |
| **9001** | P2P Main Network | TCP |
| **9002** | MDS (MiniDAPP System) | TCP |
| **9005** | RPC Interface | TCP |

---

## 🛠 Troubleshooting

* **Java Errors:** Ensure you are using version 17+. Check with `java -version`.
* **RPC Connection Refused:** Ensure the Pi and the Bass OS device are on the same subnet.
* **Performance:** If running Bass OS and the Node on the same hardware, ensure you have adequate cooling (heatsinks/fan).
