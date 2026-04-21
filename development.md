---
layout: page
title: Development
permalink: /development/
---

## Development workflow

1. Clone upstream source and install dependencies.
2. Run local dev server and validate desktop/API flows.
3. Build and test DApp integration paths.
4. Validate permissions, bridge APIs, and event handling.
5. Submit changes through pull requests.

## Key implementation areas

- **Desktop shell:** window/taskbar and app lifecycle
- **Backend services:** API endpoints, DApp management, orchestration
- **Bridge layer:** secure postMessage contracts and permission checks
- **Kernel/system services:** network, logging, platform internals

## Recommended contribution focus

- Reliability and observability improvements
- Security hardening and permission controls
- DApp SDK/tooling enhancements
- Raspberry Pi performance optimization

## Documentation expectations

- Keep interface contracts explicit and versioned
- Document any new permissions and security implications
- Include troubleshooting guidance for operational changes
