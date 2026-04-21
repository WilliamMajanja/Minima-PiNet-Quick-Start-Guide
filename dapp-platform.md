---
layout: page
title: DApp Platform
permalink: /dapp-platform/
---

## DApp runtime model

The platform supports three DApp categories:

- **TypeScript DApps** (`.zip`, `.tar.gz`)
- **React dashboard apps** (URL-loaded)
- **Classic Minima MiniDapps** (`.mds.zip`)

All DApps run in isolated host contexts with permission-driven capabilities.

## Installation flows

- **Install from URL** for hosted packages
- **Sideload** for development/testing and custom manifests

## Bridge capabilities

Permission-scoped bridge methods include:

- Wallet balance and token send operations
- Minima RPC command execution
- Maxima contact and messaging workflows
- Cluster state and system metrics retrieval
- File listing/read access (where allowed)
- Notification dispatch

## Event-driven integration

The platform can push runtime events to active DApps, such as:

- New block updates
- Balance updates
- Messaging events
- Cluster and system-state changes

## DApp development starting points

- Create a manifest with explicit permissions
- Build to supported package format
- Validate behavior via sideload installation
- Use least-privilege permissions for production distribution

## Related

- **[Development]({{ '/development/' | relative_url }})**
- **[Security]({{ '/security/' | relative_url }})**
