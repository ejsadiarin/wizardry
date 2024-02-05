---
title: my very own setup architecture
date: 2024-02-05-1209 (February 05, 2024 12:09 PM)
tags:
- Homelab
- Architecture
- Bastion Setup
---

# Philosophy
- everything should be reproducible and disposable
- simple and clear

# Setup Architecture
- [Tailscale](https://tailscale.com/) (VPN)
  - devices are connected via tailnet
  - controlled via tailscale control server web interface (not a hub and spoke)
- [Immich](https://immich.app/docs/overview/introduction) (Backup photos and videos)
  - self-hosted backup solution for photos and videos
- [Syncthing](https://syncthing.net/) (File Sync)
  - sync files between devices
  - note that this is strictly for syncing files (includes photos and videos) and not for backup
  - suitable for photos, videos, and notes (md, obsidian)
  - login via CLI: `syncthing` (web gui)
- [Nextcloud](https://nextcloud.com/) (Cloud Storage)
  - self-hosted cloud storage
  - for documents and other files

## Backups
- USB drives or external HDD/SSD
  - important photos and videos
- Immich running on docker (self-hosted via NAS)
  - important photos and videos
- Nextcloud (in the cloud)
  - documents and other files
- Syncthing devices (connected via tailnet)
  - "local" synced copy
