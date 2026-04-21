---
layout: page
title: Installation
permalink: /installation/
---

## Installation paths

### 1) Recommended: Flash prebuilt image

1. Download the latest `PiNetOS-RaspberryPi.img` from [upstream releases](https://github.com/WilliamMajanja/Minima-PiNet-Os/releases/latest).
2. Open Raspberry Pi Imager and choose **Use custom**.
3. Select the downloaded image and flash it to your target storage.
4. Boot the Pi and complete first-boot initialization.

### 2) Local development setup

1. Clone the upstream repository.
2. Install dependencies with `npm install`.
3. Start development mode with `npm run dev`.
4. Open `http://localhost:3000`.
5. Optionally run desktop shell with `npm run electron:dev`.

## First boot checklist

- Confirm networking and hostname
- Verify expected services are running
- Validate access to DApp Store and core desktop workflows
- Check system health (temperature, memory, storage)

## Post-install hardening checklist

- Change default credentials and rotate keys
- Restrict management interfaces to trusted subnets
- Enable/update firewall rules for intended service ports only
- Review **[Security]({{ '/security/' | relative_url }})** controls

## Related

- **[Operations]({{ '/operations/' | relative_url }})**
- **[Troubleshooting]({{ '/troubleshooting/' | relative_url }})**
