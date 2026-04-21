---
layout: page
title: Security
permalink: /security/
---

## Security model

Minima-PiNet-OS follows a zero-trust posture for edge deployments:

- Verify integrity early and continuously
- Minimize default exposure
- Isolate execution contexts
- Gate privileged operations behind explicit permissions

## Security controls

### Integrity and attestation

- Hash and verify critical boot/config artifacts
- Detect unauthorized state drift
- Anchor or audit verification results where available

### Network protection

- Use encrypted peer links for east-west traffic
- Keep host management planes private by default
- Expose only required service ports

### Runtime isolation

- Isolate workloads using container boundaries
- Separate high-risk services from control-plane functions
- Apply resource limits to reduce blast radius

### DApp security

- Enforce permission-declared bridge access
- Reject over-privileged manifests in review process
- Validate package source and transport integrity

## Operator hardening checklist

- Rotate credentials/keys on schedule
- Lock down SSH and management endpoints
- Keep dependencies and base image updated
- Audit logs for anomalous service behavior
