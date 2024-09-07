---
tags:
  - Homelab
  - Self-host
date: 2024-02-05-1209 (February 05, 2024 12:09 PM)
title: my very own setup architecture
---

<!-- 2024-02-05-1209 (February 05, 2024 12:09 PM) -->

# Philosophy

- everything should be reproducible and disposable (redundancy)
- simple and clear
- portability is core (quiet, low power, and portable)
  - run everything as containers and/or VMs
  - Mini PCs (Dell Optiplex, Lenovo M720/M900, HP EliteDesk)
    - use atleast 8th gen intel i5 or i7
  - Raspberry Pi 4 and 5 - cheap, quiet, and low power
- devices are just "interfaces", the data is the core (and the data is backed up in multiple places)
- all configuration must be as `code` (`IaC`)
  - yaml, conf, ini, toml, json, hcl, code, etc.
- automated configurations via ansible

# Services

### Essentials

- [Tailscale](https://tailscale.com/) (VPN)
  - devices are connected via tailnet
  - controlled via tailscale control server web interface (not a hub and spoke)
  - with Mullvad VPN for exit nodes as a service
  - NOTE: can self-host coordination server via [Headscale]()
- [PiHole](https://pi-hole.net/) (DNS)
  - self-hosted DNS server with Ad block
- [Adguard Home](https://pi-hole.net/) (DNS)
  - modern alternative to PiHole (see [how-to-setup-adguard-home](./how-to-setup-adguard-home.md))
- [NTFY](https://ntfy.rtfd.io/) (Notifications)
  - notification server for desktop and mobile
  - notifications for long running commands
- [Apprise](https://github.com/caronc/apprise) (Notifications)
  - encompasses all notification (including ntfy, etc.)
  - cli, can integrate to scripts/python
  - can send to apps, email, sns, desktop os, mobile, etc.
- [Proton Suite]()
  - everything google-alternative:
    - emails,
  - with end-to-end encryption
- [Obsidian]()
  - note-taking and rendering everything markdown
  - NOTE: don't use Obsidian Sync, use Syncthing instead
- [Caddy]() (Reverse Proxy)
  - for reverse proxy (see [harden-reverse-proxy](./harden-reverse-proxy.md))
  - dns resolution, fail2ban, etc.

### Photos (One of these)

- [Immich](https://immich.app/docs/overview/introduction) (Backup photos and videos)
  - self-hosted backup solution for photos and videos
- [Photoprism](https://) (Backup photos and videos)
  - self-hosted backup solution for photos and videos
- [Ente]()
  - cloud-based with end-to-end encryption gallery
- [Aves]()
  - local google photos alternative

### Syncs

- [Syncthing](https://syncthing.net/) (File Sync)
  - sync files between devices
  - note that this is strictly for syncing files (includes photos and videos) and not for backup
  - suitable for photos, videos, and notes (md, obsidian)
  - login via CLI: `syncthing` (web gui)
- [Nextcloud](https://nextcloud.com/) (Cloud Storage)
  - run in a docker container or a VM
  - self-hosted cloud storage (main storage)
  - for documents, bookmarks, contact, and calendar
- [Filebrowser](https://github.com/filebrowser/filebrowser) (Web Filebrowser)
  - run in a docker container

### Git

- [Gitea](https://gitea.com/) (Git Server)
- [Gitlab](https://gitlab.com/) (Git Server)

### Monitoring/Observability

- [Prometheus]()
  - for monitoring logs and then sending to Grafana to create graphs and charts
- [Grafana]()
  - for making dashboards
- [OneUptime]()
  - complete open-source observability platform

## What do I need right now?

- Dell Optiplex Line or Lenovo M900 Mini PC (Intel i5 6500T with 16 GB RAM) or HP EliteDesk
- Synology NAS (DS220+ or DS420+) --> run as NFS volume
  - run as NFS volume for Proxmox
  - can run Nextcloud
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

`3-2-1 Rule`: 3 copies, 2 onsite, 1 offsite

1. USB drives or external HDD/SSD

   - important photos and videos

2. Immich running on docker (self-hosted via NAS)

   - self-hosted
   - important photos and videos

3. Nextcloud (in the cloud)

   - documents and other files

4. Syncthing devices (connected via tailnet)

   - "local" synced copy

### Backup Tools

- rsync
- restic
- rclone to backblaze b2

# Network Architecture
