---
layout: page
title: Architecture
permalink: /architecture/
---

## System architecture overview

Minima-PiNet-OS combines a lightweight host with modular subsystems for blockchain, DApps, AI, and secure orchestration.

## Core layers

1. **Hardware Layer**
   - Raspberry Pi ARM64 target platform
   - Optional hardware acceleration paths (GPU/NPU)

2. **Isolation & Virtualization Layer**
   - LXC-based workload isolation
   - Resource boundary controls for deterministic behavior

3. **Security & Attestation Layer**
   - Integrity verification of critical boot/config state
   - Zero-trust design for mutable edge environments

4. **Networking Layer**
   - WireGuard-oriented private communication patterns
   - Segmented service exposure for reduced attack surface

5. **Application Layer**
   - Minima node integration
   - DApp runtime host + desktop orchestration

## Key components mapped from upstream structure

- `server.ts`: API surface, DApp serving, service orchestration
- `components/apps/*`: DApp Store and host frames
- `services/*`: bridge, network, logs, and platform services
- `types/*`: typed contracts for DApps, kernel, and security
- `kernel/`, `hal/`, `boot/`: low-level system control zones
- `PiNetOS/dapps/`: example DApps and templates

## Design principles

- Keep the base system minimal and explicit
- Isolate workloads by default
- Prefer permission-based interfaces for DApps
- Ensure operational observability for edge deployments
- Maintain compatibility with Raspberry Pi imaging workflows
