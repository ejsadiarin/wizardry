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
- portability is core
  - Containers and VMs
  - Raspberry Pi 4 and 5 - cheap, quiet, and low power

# Setup Architecture
- [Tailscale](https://tailscale.com/) (VPN)
  - devices are connected via tailnet
  - controlled via tailscale control server web interface (not a hub and spoke)
- [Immich](https://immich.app/docs/overview/introduction) (Backup photos and videos)
  - self-hosted backup solution for photos and videos
- [Photoprism](https://) (Backup photos and videos)
  - self-hosted backup solution for photos and videos
- [Syncthing](https://syncthing.net/) (File Sync)
  - sync files between devices
  - note that this is strictly for syncing files (includes photos and videos) and not for backup
  - suitable for photos, videos, and notes (md, obsidian)
  - login via CLI: `syncthing` (web gui)
- [Nextcloud](https://nextcloud.com/) (Cloud Storage)
  - self-hosted cloud storage
  - for documents and other files
- [Vaultwarden]()

## What do I need right now?
- Dell Optiplex Line or Lenovo M900 Mini PC (Intel i5 6500T with 16 GB RAM) or HP EliteDesk
- PI 5 (Raspberry Pi 5) 8GB
  - with 500 GB external SSD
  - running: Immich, Nextcloud, and Vaultwarden
  - shares directories via SMB for data like Proxmox ISOs, Docker volumes, and personal files
- PI 4 (Raspberry Pi 5) 4GB
  - running: 
    - docker containers,
    - arr stack (radarr, sonarr, lidarr, jackett, and transmission) - idk if i need
    - proxmox vm running LXC container with PiHole,
    - vm for "core services" like nginx proxy manager, GitHub runner, Grafana, and InfluxDB server
    - 3 node k3s cluster for learning Kubernetes

- Storage (TrueNAS) for backups and central storage (should be immutable)

## Backups
- should consist of 4: physical copy, cloud copy, and local copy (self-hosted and/or synced)
1. USB drives or external HDD/SSD
  - important photos and videos
2. Immich running on docker (self-hosted via NAS)
  - self-hosted
  - important photos and videos
3. Nextcloud (in the cloud)
  - documents and other files
4. Syncthing devices (connected via tailnet)
  - "local" synced copy
