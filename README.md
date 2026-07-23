# PiNet-OS Quick Start Guide

> **Turn a Raspberry Pi into a self-sovereign edge cloud — blockchain-native, AI-accelerated, zero-trust by default.**

This guide walks you through setting up **Minima-PiNet-OS** on a Raspberry Pi, running a full Minima L1 blockchain node, forming a cluster, and accessing the web desktop.

**Architect:** William Majanja
**License:** MIT (PiNet-OS) / Apache 2.0 (Minima Core) / Unlicense (Coffee Protocol CPIP)
**Version:** PiNet-OS v3.0.0 | Minima Core v1.1.2 | CPIP v5.0.0

---

## Quick Start (One-Liner)

```bash
curl -fsSL https://raw.githubusercontent.com/WilliamMajanja/Minima-PiNet-Os/main/bin/pinet-setup | bash && cd Minima-PiNet-Os && bin/pinet setup && bin/pinet start --role master
```

Or clone manually:

```bash
git clone --depth 1 https://github.com/WilliamMajanja/Minima-PiNet-Os.git && cd Minima-PiNet-Os && bin/pinet setup && bin/pinet start --role master
```

Access the dashboard at `http://<your-pi-ip>:3000` — default creds: `pinet` / `pinet` (change immediately).

---

## Table of Contents

1. [Overview](#1-overview)
2. [Hardware Requirements](#2-hardware-requirements)
3. [Quick Start: Flash & Boot (Raspberry Pi)](#3-quick-start-flash--boot-raspberry-pi)
4. [Quick Start: Spawnable Runtime (Any Linux)](#4-quick-start-spawnable-runtime-any-linux)
5. [Quick Start: Local Desktop Demo](#5-quick-start-local-desktop-demo)
6. [Minima Blockchain Node](#6-minima-blockchain-node)
7. [Forming a PiNet Cluster](#7-forming-a-pinet-cluster)
8. [CPIP Security Provider](#8-cpip-security-provider)
9. [Coffee Protocol (CPIP) — Standalone Server](#9-coffee-protocol-cpip--standalone-server)
10. [Minima Whitepaper Summary](#10-minima-whitepaper-summary)
11. [Network Ports Reference](#11-network-ports-reference)
12. [Configuration Reference](#12-configuration-reference)
13. [Troubleshooting](#13-troubleshooting)
14. [Architecture Overview](#14-architecture-overview)
15. [Version History & Roadmap](#15-version-history--roadmap)

---

## 1. Overview

**Minima-PiNet-OS** (v3.0.0) is a zero-bloat, blockchain-native operating system that turns an $80 Raspberry Pi into a self-sovereign edge cloud. It combines:

- **Minima L1 Blockchain Node** — Full node in <500 MB, providing cryptographic identity, P2P networking, and the Maxima encrypted message bus
- **CPIP Security Provider** (The Coffee Protocol v5.0.0) — AES-256-GCM, ECDSA P-256, HMAC-SHA256 RPC auth, Hybrid ECDH + 1nf1D3L Kyber ML-KEM-768 post-quantum KEM, ITF Defense
- **k3s Edge Cluster** — Lightweight Kubernetes for containerised workloads
- **On-Device AI** — Hailo-8L NPU (13 TOPS) for local LLM inference (Ollama/GGUF)
- **Browser Desktop** — FastAPI + Jinja2 web UI with 23+ built-in apps
- **Confidential Computing** (v3.0.0) — Arm CCA / RISC-V AP-TEE enclaves, zkVM proofs, edge compute marketplace

---

## 1. Hardware Requirements

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| **Compute** | Raspberry Pi 4 (4 GB) | **Raspberry Pi 5 (16 GB)** x3 |
| **AI Accelerator** | ARM NEON (CPU) | **Hailo-8L NPU (13 TOPS)** |
| **Storage** | 16 GB microSD (Class 10) | 128 GB NVMe SSD (PCIe Gen 3) |
| **Network** | Gigabit Ethernet | Gigabit Ethernet + WireGuard mesh |
| **Power** | USB-C 5V/3A | Official RPi 27W USB-C PSU |
| **Cooling** | Passive heatsink | Active cooler (required for RPi 5) |

**Supported Pi Models:** Pi 5, Pi 4, Pi 3, Pi 2, Pi 1, Pi Zero, Pi Zero 2 W, CM3, CM4  
**RISC-V (Experimental):** StarFive VisionFive 2 (JH7110)  
**AI Accelerator (Optional):** Hailo-8L NPU (13 TOPS)

A reference 3-node cluster (3x Pi 5 16GB + Hailo-8L + NVMe) costs under $1,500 fully assembled.

---

## 2. Quick Start: Flash & Boot (Raspberry Pi)

### Step 1: Download the Image

Download `PiNetOS-RaspberryPi.img` from the [latest release](https://github.com/WilliamMajanja/Minima-PiNet-Os/releases/latest).

### Step 2: Verify Integrity

```bash
sha256sum --check SHA256SUMS.txt
```

### Step 3: Flash the Image

Use **Raspberry Pi Imager** (recommended):
1. Click **CHOOSE OS** → **Use custom** → select `PiNetOS-RaspberryPi.img`
2. Click **CHOOSE STORAGE** → select your SD card or NVMe
3. (Optional) Click the gear icon to pre-configure Wi-Fi, hostname, and SSH
4. Click **WRITE**

Or use `dd`:
```bash
sudo dd if=PiNetOS-RaspberryPi.img of=/dev/sdX bs=4M status=progress conv=sync,noerror
sync
```

### Step 4: First Boot

1. Insert the flashed storage into your Raspberry Pi and power on
2. First-boot provisioning takes ~2 minutes
3. Access the dashboard at `http://<pi-ip>:3000`

**Default credentials:** username `pinet`, password `pinet` — change immediately with `passwd`.

---

## 3. Quick Start: Spawnable Runtime (Any Linux)

PiNet-OS runs as a contained environment on **any existing Linux distro** on Raspberry Pi. No dedicated image flashing required.

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/WilliamMajanja/Minima-PiNet-Os.git
cd Minima-PiNet-Os

# 2. Run the setup script (installs Java, Python, downloads Minima)
bin/pinet setup

# 3. Start PiNet-OS
bin/pinet start --role master    # For the first node (master)
# OR
bin/pinet start --role worker --master <address>  # For worker nodes
```

### What Setup Does

1. Checks and installs **Java 17+** (for the Minima node)
2. Checks and installs **Python 3.11+** (for the FastAPI/Jinja web desktop)
3. Downloads the **Minima JAR** to `~/.pinet/bin/minima.jar`
4. Installs the Python desktop's `requirements.txt`
5. Generates node identity and initial configuration

### Managing the Runtime

```bash
pinet status             # Show runtime status
pinet stop               # Stop all services
pinet logs --follow      # Tail service logs
pinet shell              # Attach to PiNet-OS session
pinet cluster            # Show cluster topology
pinet open               # List available apps
pinet detect             # Detect Pi model and hardware
pinet config show        # View configuration
```

### Accessing the Desktop

Open a browser and navigate to:
```
http://<pi-ip>:3000
```

---

## 4. Quick Start: Local Desktop Demo

Run the full web desktop UI locally in under two minutes — no Raspberry Pi required.

```bash
# 1. Clone and enter the repo
git clone https://github.com/WilliamMajanja/Minima-PiNet-Os.git
cd Minima-PiNet-Os

# 2. Install Python dependencies
pip install -r requirements.txt

# 3. Copy and configure environment
cp .env.example .env

# 4. Start the desktop server
python run.py
```

Open **http://localhost:3000** — you get the full desktop, system monitor, terminal, and cluster manager. Hardware-backed routes degrade gracefully when no Pi/NPU is present.

---

## 5. Minima Blockchain Node

Minima is a decentralized L1 blockchain with a UTXO model, MMR proof database, covenants, MAST, and post-quantum signatures (WOTS+). Every user runs a full node.

### Running Minima Standalone

```bash
# Download the latest Minima JAR
wget -O minima.jar https://github.com/WilliamMajanja/minima-core/releases/latest/download/minima.jar

# Start with default parameters (connects to mainnet)
java -jar minima.jar

# With MDS (MiniDAPP System) enabled
java -jar minima.jar -mdsenable -mdspassword YOUR_PASSWORD -port 9001

# Private test-net
java -jar minima.jar -nop2p -test -genesis
```

### CLI Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| `-port` | P2P port | 9001 |
| `-data` | Data directory | ~/.minima |
| `-p2pnodes` | Initial peer list | spartacusrex.com:9001 |
| `-mdsenable` | Enable MiniDAPP System | off |
| `-mdspassword` | MDS password | auto-generated |
| `-solo` | Solo mode (testing) | off |
| `-clean` | Wipe previous data | off |
| `-megammr` | MegaMMR exchange mode | off |
| `-test` | Test mode (faster blocks) | off |
| `-genesis` | Create genesis block | off |
| `-nop2p` | Disable P2P networking | off |

### Connecting to the Network

```bash
# Add a peer
peers action:addpeers peerslist:spartacusrex.com:9001

# Or from CLI
java -jar minima.jar -p2pnodes spartacusrex.com:9001

# Or from a peer list file
java -jar minima.jar -p2pnodes https://spartacusrex.com/minimapeers.txt
```

### Key Blockchain Parameters

| Parameter | Mainnet | Test Mode |
|-----------|---------|-----------|
| Block speed | ~50 seconds | ~20 seconds |
| Confirm depth | 3 blocks | 3 blocks |
| Cascade frequency | 100 blocks | 3 blocks |
| Cascade start depth | 1024 blocks | 32 blocks |
| MMR proof history | 256 blocks | 256 blocks |
| SQL TxPoW DB retention | 3 days | 3 days |
| RAM mempool retention | 1 hour | 1 hour |
| Archive retention | 50 days | 50 days |
| Pulse frequency | 10 minutes | 10 minutes |

### Minima Core Security

The Minima Core codebase has undergone a comprehensive security audit with **80 CodeQL alerts remediated** across 7 categories (SSRF, path traversal, SQL injection, RSA-OAEP, RSA key size, AES-CBC, static IV). All 278 tests pass (244 existing + 34 security validation tests). The node has been verified on mainnet (v1.0.46.8, block 2,219,786+).

---

## 6. Forming a PiNet Cluster

PiNet-OS clusters use the **Maxima protocol** (Minima's encrypted P2P bus) for all coordination. No central API server, no shared database — just peer-to-peer messages between Minima nodes.

### Step 1: Start the Master Node

On the first Pi, start as master:

```bash
pinet start --role master
```

This will:
1. Start the local Minima node (P2P port 9001, RPC port 9005)
2. Start the cluster manager (API port 9090)
3. Start the web desktop (port 3000)
4. Initialize the cluster state
5. Begin broadcasting heartbeats

### Step 2: Join Worker Nodes

On each additional Pi, join the cluster:

```bash
pinet start --role worker --master <master_maxima_address>
# OR
pinet join <master_maxima_address>
```

### Step 3: Verify the Cluster

```bash
pinet cluster
```

Or open the browser desktop at `http://<any-node-ip>:3000` and navigate to the Cluster Manager app.

### Cluster Protocol

All messages are encrypted and sent via Maxima using the `pinet-cluster` application ID. CPIP provides additional CoffeeCipher v3 (AES-256-GCM) payload encryption and ECDSA P-256 node identity via `AUTH_CHALLENGE`/`AUTH_RESPONSE`.

| Message | Direction | Purpose |
|---------|-----------|---------|
| `CLUSTER_JOIN_REQUEST` | Worker -> Master | Request to join |
| `CLUSTER_JOIN_ACCEPT` | Master -> Worker | Acceptance + peer list |
| `CLUSTER_HEARTBEAT` | All -> Master | Liveness + metrics (every 10s) |
| `CLUSTER_STATE_UPDATE` | Master -> All | Topology changes |
| `CLUSTER_EXEC_REQUEST` | Master -> Worker | Run a workload |
| `CLUSTER_EXEC_RESULT` | Worker -> Master | Workload result |
| `CLUSTER_METRICS` | All -> Master | System metrics |
| `NODE_DEREGISTER` | Any -> Master | Graceful departure |

### Node Health

The master monitors all nodes via heartbeat timeouts:
- **Active**: Heartbeat received within 30 seconds
- **Stale**: No heartbeat for 30-60 seconds
- **Offline**: No heartbeat for >60 seconds

### On-Chain Provenance

Every significant cluster event is recorded as a Minima burn transaction:
- Node joins/leaves, role changes, workload submissions, configuration changes

```bash
curl http://localhost:3000/api/cluster/provenance
```

### Enterprise Workloads

Submit workloads to specific nodes:

```bash
curl -X POST http://localhost:3000/api/cluster/exec \
  -H "Content-Type: application/json" \
  -d '{"targetNodeId": "pinet-worker-1", "command": "python3", "args": ["inference.py"]}'
```

### Scaling

PiNet-OS clusters scale horizontally — add more Pi 5 nodes and join them:

```bash
# On each new Pi:
pinet setup
pinet join <master_address>
```

---

## 7. CPIP Security Provider

**The Coffee Protocol (CPIP) v5.0.0** is the primary security provider for all Minima nodes. It is enabled by default (`CPIP_ENABLED=1`) and provides:

| Primitive | Algorithm | Standard | Role |
|-----------|-----------|----------|------|
| CoffeeCipher v3 | AES-256-GCM + HKDF-SHA256 | FIPS 197 / SP 800-56C | Data at rest, RPC payload |
| ECDSA P-256 | SHA256withECDSA | FIPS 186-4 | Node identity, attestation |
| ECDH P-256 | ECDH secp256r1 | FIPS 186-4 / SP 800-56A | Key exchange |
| HybridKEM | ECDH P-256 + 1nf1D3L Kyber ML-KEM-768 | Hybrid (FIPS + Non-FIPS) | Post-quantum key encapsulation |
| 1nf1D3L Kyber | ML-KEM-768 (η=3) | Non-FIPS | PQ KEM (research variant) |
| HMAC-SHA256 | RPC token auth | FIPS 180-4 | Replaces Basic Auth |
| ITF Defense | Probe blocking, IP blacklist | — | Active defense |
| FIPS Self-Tests | Power-on KATs | FIPS 140-2/3 | Startup verification |

CPIP does **not** implement RSA-KEM. The hybrid KEM uses ECDH P-256 + Kyber (not RSA).

### CoffeeCipher v3

AES-256-GCM (FIPS 197) with 12-byte nonce and 16-byte authentication tag. Key derivation via HKDF-SHA256 with domain-separated info strings. Format: `nonce(12B) || ciphertext || GCM-tag(16B)`. Backward-compatible with v1 and v2 messages.

### 1nf1D3L Kyber (Post-Quantum KEM)

Available via `b4dm4n_cw.py`: n=256, k=3, q=3329, eta1=3, eta2=3, du=10, dv=4. Domain tag: "1NF1D3L-KYBER-V1". NTT twiddle perturbation for side-channel resistance. Coffee recipe binding. Sizes: PK=1184B, SK=2400B, CT=1120B, SS=32B.

### HybridKEM (Classical + Post-Quantum)

ECDH P-256 + 1nf1D3L Kyber, combined via HKDF-SHA256. Secure if EITHER classical ECDH OR PQ Kyber component holds. PK~1251B, SK~2432B, CT~1187B, SS=32B.

### CPIP Configuration

| Variable | Default | Description |
|----------|---------|-------------|
| `CPIP_ENABLED` | 1 | Enable CPIP security provider |
| `CPIP_FIPS` | 0 | FIPS 140-2/3 mode (blocks startup on self-test failure) |
| `CPIP_DEFENSE_ENABLED` | 1 | Enable ITF Defense (probe blocking, IP blacklisting) |
| `CPIP_RPC_AUTH` | 1 | HMAC-SHA256 token auth for Minima RPC calls |
| `CPIP_PQ_TLS` | 0 | Enable post-quantum TLS (hybrid ECDH + Kyber-768) |
| `CPIP_PQ_HYBRID` | 1 | Hybrid classical + PQ key exchange |
| `CPIP_RECIPE` | espresso | Coffee recipe for KDF domain separation (Minima uses `minima`) |
| `CPIP_SSL` | 1 | Enable HTTPS (TLS) — on by default |
| `CPIP_SSL_AUTO` | 1 | Auto-generate self-signed certificate |
| `CPIP_COVERT_KEY` | (auto) | Encryption passphrase for covert channel |
| `CPIP_MESH` | 1 | Enable mesh networking |

### CPIP ITF Defense

The CPIP ITF Defense provides active API-layer protection:
- **Probe blocking**: Returns HTTP 418 "I'm a teapot" to known scanning tools
- **Pentest tool fingerprinting**: Detects 16 security tools (Burp Suite, Nmap, SQLMap, Nikto, etc.)
- **IP blacklisting**: Rate-limited exponential ban duration (base 1 hour, up to 24 hours)
- **Stealth mode**: Togglable at runtime via API
- **Port hopping**: Mesh ports change on configurable intervals
- **Defense policy groups**: Anti-ISP (STUN, UPnP, DNS tunnel, WSS, DoH), Anti-Stingray (IMSI-catcher detection, cell/RF/signal scans), Anti-Surveillance (DPI evasion, traffic obfuscation, metadata strip, exploit kit detection), Net-Neutrality (bandwidth monitoring, protocol masquerade, fragmentation, throttle detection, jitter injection)
- All defense vectors are independently togglable at runtime without restart

## 8. Coffee Protocol (CPIP) — Standalone Server

CPIP runs as a standalone server implementing RFC 2324 (HTCPCP) and RFC 7168 (HTCPCP-TEA). It serves as the security provider for Minima and also operates independently as a multi-transport mesh communication system.

### Quick Start — Standalone

```bash
# 1. Start the server (SSL, auto-cert, and HTTP->HTTPS redirect on by default)
./server.py

# 2. Open the web dashboard
#    https://localhost:4180/dashboard

# 3. Brew coffee/tea
curl -k -X BREW https://localhost:4180/coffee
curl -k -X BREW https://localhost:4180/tea
curl -k -X WHEN https://localhost:4180/

# 4. Use the CLI
./cpip status
./cpip brew tea "milk;variety=whole, sugar;variety=honey"

# 5. Check mesh network
./cpip mesh status
./cpip mesh peers

# 6. Send a covert message
./cpip covert encode "hello world"

# 7. Post-Quantum KEM
./b4dm4n_cw.py keygen -o mykeys
./b4dm4n_cw.py encaps mykeys.pk -o ct.bin
./b4dm4n_cw.py decaps mykeys.sk ct.bin

# 8. Hybrid ECDH + Kyber
./b4dm4n_cw.py keygen -a hybrid -o hybrid
./b4dm4n_cw.py encaps -a hybrid hybrid.pk -o hyb_ct.bin
./b4dm4n_cw.py decaps -a hybrid hybrid.sk hyb_ct.bin

# 9. Enable all transports
CPIP_SAT=1 CPIP_RADIO=1 CPIP_MOBILE=1 ./server.py

# 10. Run without SSL
CPIP_SSL=0 ./server.py
```

### Mesh Transport Architecture

| Transport | Env Flag | Port | Description |
|-----------|----------|------|-------------|
| LAN Mesh | `CPIP_MESH=1` | 4191 | UDP heartbeat mesh on local network |
| Satellite | `CPIP_SAT=1` | 4195 | Internet-wide UDP relay with GPS coordinates |
| Radio | `CPIP_RADIO=1` | Unix socket | LoRa SPI, KISS TNC serial, or simulation |
| Mobile | `CPIP_MOBILE=1` | 4196 | 4G/5G WWAN mesh with signal telemetry |
| Covert Channel | `CPIP_COVERT=1` | — | Data embedded in Accept-Additions brew headers |

### Web Dashboard

CPIP includes a single-page dashboard at `/dashboard` with 14 tabs: Brew, Mesh, Covert, ITF Defense (418/stealth/blacklist), Crypto, IR (incident response), Signal, Diag, Anti-ISP, Anti-Stingray, Anti-Surveillance, Net Neutrality, Schedule, and History.

## 9. Minima Whitepaper Summary

The Minima whitepaper (`minima-core-main/Minima_wp_v9.pdf`) describes the protocol's key innovations:

- **Tx-PoW Blockchain**: Every transaction is PoW-mined (~10s average work per device). When a Tx-PoW value is high enough (~1 Tx-PoW every 10s), it also constitutes a block. Users' transactions build the chain — no separate miners.

- **Complete Nodes**: Every user runs a full node that both validates AND constructs the blockchain. Mining centralization is eliminated because there are no paid miners. All users secure the network.

- **GHOST Consensus**: Transactions are added to blocks even when they themselves are blocks. The GHOST rule allows consensus with faster block times than a simple longest-chain rule.

- **Maxima**: Layer-2 messaging protocol for peer-to-peer encrypted communication. Not the focus of the whitepaper (v9 covers Layer 1).

- **UTXO Model**: Like Bitcoin, but data is stored in an MMR (Merkle Mountain Range) Proof DB where users only track their own coins.

- **KISSVM**: Custom scripting language supporting covenants, state machines, MAST, quantum-secure signatures, and merkle proof checks.

- **Cascade Proof Chain**: Tree-based chain pruning — old blocks are compressed into cascade nodes (32 levels, 256 nodes each, starting at depth 1024).

- **Adaptive Block Scaling**: Block difficulty adjusts based on transaction volume. More transactions = more security.

- **Quantum Security**: All signatures are quantum-secure (WOTS+ / SPHINCS+) from inception.

The full whitepaper is at `minima-core-main/Minima_wp_v9.pdf`. The comprehensive security remediation whitepaper is at `minima-core-main/WHITEPAPER.md` (IEEE-formatted, 803 lines). The CPIP Blackpaper is at `The Coffee Protocol/blackpaper.md` (475 lines, v5.0.0).

---

## 10. Network Ports Reference

| Port | Service | Description |
|------|---------|-------------|
| 3000 | Web Desktop | Browser-based control plane (FastAPI) |
| 4180 | CPIP Security | ITF Defense + crypto API sidecar |
| 9001 | Minima P2P | Blockchain peer-to-peer networking |
| 9002 | Minima MDS File | MiniDAPP System file server |
| 9003 | Minima MDS CMD | MiniDAPP System command server |
| 9004 | Minima MDS | MiniDAPP System (SSL) |
| 9005 | Minima RPC | Blockchain node JSON-RPC API |
| 9090 | Cluster API | Go cluster manager API |
| 4180 | CPIP HTTP | Coffee Protocol server / ITF Defense |
| 4181 | CPIP HTTP Redirect | HTTP-to-HTTPS redirect |
| 4190 | CPIP Discovery | UDP pot discovery |
| 4191 | CPIP Mesh | LAN UDP mesh heartbeat |
| 4192-4194 | CPIP Latent | Port-knocking latent ports |
| 4195 | CPIP Satellite | Internet-wide UDP relay |
| 4196 | CPIP Mobile | 4G/5G WWAN mesh |
| 11434 | Ollama | Local LLM inference (optional) |

---

## 11. Configuration Reference

All runtime configuration is controlled via environment variables. Copy `.env.example` to `.env` and edit as needed.

### Core

| Variable | Default | Description |
|----------|---------|-------------|
| `PINET_DESKTOP_PORT` | 3000 | Web desktop / API server port |
| `PINET_HOST` | 0.0.0.0 | Bind address |
| `PINET_RELOAD` | false | Hot reload (development only) |
| `PINET_MINIMA_P2P_PORT` | 9001 | Minima P2P base port |
| `PINET_MINIMA_RPC_PORT` | 9005 | Minima RPC port |
| `MINIMA_RPC_URL` | http://127.0.0.1:9005 | Minima RPC endpoint override |

### CPIP Security

| Variable | Default | Description |
|----------|---------|-------------|
| `CPIP_ENABLED` | 1 | Enable CPIP security provider |
| `CPIP_FIPS` | 0 | FIPS 140-2/3 mode |
| `CPIP_DEFENSE_ENABLED` | 1 | Enable ITF Defense |
| `CPIP_RPC_AUTH` | 1 | HMAC-SHA256 token auth for RPC |
| `CPIP_PQ_TLS` | 0 | Post-quantum TLS (ECDH + Kyber-768) |
| `CPIP_PQ_HYBRID` | 1 | Hybrid classical + PQ key exchange |
| `CPIP_TOKEN_TTL` | 300 | RPC token TTL in seconds |
| `CPIP_PROVIDER_URL` | http://127.0.0.1:4180 | CPIP sidecar endpoint |

### AI & LLM Gateway

| Variable | Default | Description |
|----------|---------|-------------|
| `PINET_LLM_GATEWAY` | 1 | Enable on-device LLM gateway |
| `PINET_LLM_GATEWAY_URL` | http://127.0.0.1:11434 | Ollama API URL |
| `PINET_LLM_DEFAULT_MODEL` | llama3.2:3b | Default LLM model |
| `PINET_LLM_FALLBACK_GEMINI` | 1 | Fall back to Gemini cloud |

### LXC Multi-Tenant Quotas (v1.3.0)

| Variable | Default | Description |
|----------|---------|-------------|
| `PINET_LXC_QUOTA` | 1 | Enable multi-tenant LXC quotas |
| `PINET_LXC_MAX_TENANTS` | 16 | Maximum LXC tenants per node |

### TPM Key-Wrap (v1.3.0)

| Variable | Default | Description |
|----------|---------|-------------|
| `PINET_TPM_KEYWRAP` | 1 | Enable TPM 2.0 hardware key-wrap |

### Remote Attestation (v2.0.0)

| Variable | Default | Description |
|----------|---------|-------------|
| `PINET_ATTESTATION` | 1 | Enable formal remote attestation |

### v3.0.0 Features

| Variable | Default | Description |
|----------|---------|-------------|
| `PINET_ENCLAVE` | 1 | Enable confidential computing enclaves |
| `PINET_ENCLAVE_TEE_TYPE` | auto | TEE type (cca, riscv-tee, auto) |
| `PINET_ZK_PROVER` | 1 | Enable zkVM prover |
| `PINET_MARKETPLACE` | 1 | Enable decentralized marketplace |
| `PINET_MARKETPLACE_MAX_LISTINGS` | 100 | Max listings per node |
| `PINET_MARKETPLACE_ESCROW_TOKENS` | minima | Escrow token |

---

## 12. Troubleshooting

| Symptom | Fix |
|---------|-----|
| **Java errors** | Confirm Java 17+ with `java -version` |
| **RPC connection refused** | Verify IP, port, and same subnet |
| **Poor performance/overheating** | Use heatsink/fan and stable power supply |
| **Port 3000 already in use** | Set `PINET_DESKTOP_PORT=3001` in `.env` |
| **`pip install` fails** | Ensure Python 3.11+ is installed (`python3 --version`) |
| **Pi not booting** | Re-flash the SD card; verify power supply (27W USB-C) |
| **Web UI not loading on Pi** | Check `sudo systemctl status pinet-desktop` on the Pi |
| **Minima node offline** | Run `sudo systemctl restart minima` on the Pi |
| **SSH refused** | Run `sudo systemctl enable --now ssh` on the Pi |
| **No display output** | Use micro-HDMI port 0 (closest to power) |
| **Checksum mismatch** | Re-download the `.img` — file may be corrupted |
| **Can't access web dashboard** | Check `http://<pi-ip>:3000`. Ensure Pi is on same network |

---

## 13. Architecture Overview

### Two-Layer Design

**Layer 1: PiNet-OS Runtime (Linux)** — A lightweight runtime that spawns on any Linux distro on Raspberry Pi:
- CLI launcher (`bin/pinet`) — single POSIX shell script for lifecycle management
- Minima node — blockchain identity, trust, and Maxima P2P bus
- Go cluster manager — heartbeats, node health, workload execution, metrics
- Systemd services for production deployment

**Layer 2: Browser Desktop (Python)** — A visual control plane served locally:
- FastAPI backend — API server, WebSocket terminal, cluster endpoints
- Jinja2 desktop UI — server-rendered desktop with 23+ built-in apps
- Real-time updates via WebSocket channels

### Trust Model

PiNet-OS implements a **zero-trust, locally-anchored** trust model:

- **Identity**: Each node generates an ECDSA P-256 keypair via CPIP. The public key is registered as a Minima address. All Maxima P2P messages are signed and encrypted.
- **Attestation**: TPM 2.0 PCR values (boot ROM -> bootloader -> kernel -> config) are hashed and recorded on the Minima blockchain. Any tamper is detectable by comparing on-chain PCRs with live measurements.
- **Key Management**: CPIP master keys can be PCR-sealed to the TPM. The sealed blob decrypts only when the system boot path matches the sealing PCR values.
- **Network**: All cluster traffic traverses encrypted WireGuard tunnels. Kubernetes NetworkPolicy enforces default-deny per namespace.

### Technology Stack

| Layer | Technology |
|-------|-----------|
| Web server | FastAPI (Python 3.11+) — async, typed, auto-documented |
| Frontend | Jinja2 templates + vanilla JS/CSS |
| Blockchain | Minima L1 (Java) — full node in <500 MB |
| P2P bus | Maxima — end-to-end encrypted message protocol |
| State verification | RMP + RNPE-2 — compressed state proofs |
| Security provider | CPIP v4.0.2 (AES-256-GCM, ECDSA P-256, RSA-KEM-2048, HMAC-SHA256, Kyber PQ KEM, ITF Defense) |
| PQ-TLS | Hybrid ECDH P-256 + Kyber-768 (ML-KEM-768) |
| Hardware key-wrap | TPM 2.0 PCR-sealed CPIP master keys |
| Cluster orchestration | k3s (lightweight Kubernetes) + Go cluster manager |
| Workload isolation | LXC with cgroups v2 quotas (up to 16 tenants) |
| Storage | IPFS (content-addressed, blockchain-anchored) |
| Networking | WireGuard mesh + nftables + fail2ban |
| Disk encryption | LUKS2 with TPM 2.0 key sealing |
| AI runtimes | TensorFlow Lite, ONNX Runtime, GGUF (llama.cpp), Hailo SDK |
| LLM gateway | Ollama (llama.cpp/GGUF) with Hailo-8L NPU + Gemini fallback |
| Remote attestation | TPM 2.0 PCR-based, anchored to Minima blockchain |
| RISC-V support | StarFive VisionFive 2 (JH7110) — experimental |

---

## 14. Version History & Roadmap

| Version | Status | Key Features |
|---------|--------|-------------|
| **v1.1.0** | Released Q2 2026 | Stable Pi 5 cluster, FastAPI desktop, security hardening, DApp platform, Raspberry Pi image |
| **v1.2.0** | Released Q2 2026 | CPIP security provider, signed OTA updates, Hailo-8L pipelines, federated DApp store, RMPE-2 provenance |
| **v1.3.0** | Released Q4 2026 | On-device LLM gateway (Ollama + Hailo-8L), multi-tenant LXC quotas, TPM 2.0 hardware key-wrap, CPIP post-quantum TLS (ECDH + Kyber-768) |
| **v2.0.0** | Released Q4 2026 | RISC-V reference board (VisionFive 2), deterministic image rebuilds, formal remote attestation spec |
| **v3.0.0** | Released Q4 2026 | Confidential computing enclaves (Arm CCA / RISC-V AP-TEE), verifiable compute proofs (zkVM/RISC Zero), decentralized edge compute marketplace |

### v3.0.0 Features

- **Confidential Computing Enclaves** — Arm CCA and RISC-V AP-TEE hardware-backed enclaves with cryptographic measurement and TEE-signed attestation anchored to Minima blockchain
- **Verifiable Compute Proofs (zkVM)** — RISC Zero zkVM integration for STARK -> SNARK proof compression with on-chain verification
- **Decentralized Edge Compute Marketplace** — Peer-to-peer marketplace for leasing edge compute resources with Minima escrow, attestation binding, and on-chain reputation

### Performance Characteristics (Raspberry Pi 5, 16 GB + Hailo-8L + NVMe)

| Metric | Value |
|--------|-------|
| Web desktop cold start | ~1.2 s |
| API throughput (GET /api/health) | ~4500 req/s |
| LLM inference (llama3.2:3b, NPU) | ~45 tokens/s |
| LLM inference (llama3.2:3b, CPU NEON) | ~8 tokens/s |
| Minima node sync (full chain) | ~3 min |
| LXC tenant creation | ~800 ms |
| TPM seal/unseal | ~150 ms / ~120 ms |
| PQ-TLS handshake | ~95 ms |
| Attestation report generation | ~200 ms |
| ZK proof generation (simple program) | ~5 s |
| ZK proof verification | ~200 ms |
| Power consumption (idle) | ~7.5 W |
| Power consumption (AI inference) | ~12 W |

---

## References

- **PiNet-OS Repository**: https://github.com/WilliamMajanja/Minima-PiNet-Os
- **Minima Core**: https://github.com/WilliamMajanja/minima-core
- **CPIP (The Coffee Protocol)**: https://github.com/WilliamMajanja/CPIP-
- **Minima Global**: https://www.minima.global/
- **Minima Whitepaper**: Included in `minima-core-main/Minima_wp_v9.pdf`
- **Security Whitepaper**: `minima-core-main/WHITEPAPER.md` (IEEE-formatted, 803 lines)
- **PiNet-OS Whitepaper**: `Minima-PiNet-Os/WHITEPAPER.md` (383 lines, v3.0.0)

---

*PiNet-OS is MIT licensed. Minima Core is Apache 2.0 licensed. Architected by William Majanja.*
