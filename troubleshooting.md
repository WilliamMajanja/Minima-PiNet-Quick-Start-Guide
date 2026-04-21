---
layout: page
title: Troubleshooting
permalink: /troubleshooting/
---

## Common issues

### Image fails to boot

- Re-check image integrity and flashing process
- Verify target board/storage compatibility
- Confirm power supply and thermals are sufficient

### Services start but DApps fail

- Confirm DApp package format is supported
- Validate manifest and required permissions
- Check backend logs for bridge/serve errors

### Cannot reach Minima APIs

- Verify listening ports and firewall rules
- Check node startup and synchronization state
- Confirm host/network routing between client and node

### Performance degradation

- Inspect thermals and CPU throttling
- Check memory pressure and storage IO saturation
- Reduce unnecessary background workloads

### Cluster communication issues

- Validate WireGuard peer state and key material
- Confirm consistent MTU/routing configuration
- Check time sync across nodes

## Escalation checklist

- Capture logs and system metrics
- Record exact firmware/image version
- Document reproducible steps and expected vs actual behavior
- Attach environment details when opening an issue upstream
