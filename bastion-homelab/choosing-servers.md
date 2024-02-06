---
title: Choosing Servers for Homelab
date: 2024-02-05-1705 (February 05, 2024 5:05 PM)
tags:
- Homelab
- Server
---

# Choosing Servers
- Servers can be anything:
  - Old laptops
  - Old PCs
  - Raspberry Pi (e.g. Raspberry Pi 4)
  - Micro PCs (e.g. Dell Optiplex 7040 micro)
- Multiple servers connected are called "clusters"

# Recommended Specs to have
- CPU:
  - **minimum:** i5 or i7 atleast 4th gen or higher
    - e.g. Intel 6500T, i7-4770, i5-6500
  - **recommended:** i5 or i7 atleast 8th or 9th gen or higher
- atleast 1TB SATA SSD
- atleast 32GB RAM (16GB minimum) memory
- Support for atleast ESXi 7
- Ceph storage
## Important to Upgrade
- RAM and Storage:
  - e.g. 16GB RAM, 500GB SSD, 1TB HDD or higher
  > Beef up the ram and the storage so that you can host as many apps as VMs / containers as you want. These are great as compute nodes. I use 3080s for compute and a NAS for bulk storage.
  > Replace M.2 drive with a bigger drive. It will allow you to host most IO-demanding VMs and containers.
  > Replace SATA HDD with SATA SSD to host other services. Format and mount storage as XFS/EXT4 or LVM volumes.
  > To extend storage further, use any external HDD drives connected to 4x USB 3.1 ports.

# For starters
These Servers are the best bang for your buck:
- **Dell Optiplex Line:**
  - Dell Optiplex 7040 micro with a Intel 6500T CPU, 16GB Ram, 500Gb SSD, 1TB HDD with Proxmox.
  - Dell Optiplex 7010 SFF
  - Dell Optiplex 7090
  - Dell Optiplex 9010, 9020, 9020 SFF
- HP Gen4 800 EliteDesk (SFF)

# What to run on these servers?
- Proxmox (for virtualization)
  - best to run multiple services on a single server machine (via VM)
  - easy to use and setup
- Kubernetes (for container orchestration)
  - for more advanced users
  - can be used to run multiple services on a single server machine (via pods)
- Docker with Portainer containers and start spinning up containers for services
  - Docker to run the Portainer containers
  - services can be anything as long as it's containerized (e.g. backup system like Borg, Immich, or Kopia)
- Anything for future projects
### Services
You may also opt-in to not use VMs and use one service per server machine:
- TrueNAS (for storage)
- Jellyfin (for media server, streaming)
- Nextcloud (for cloud storage)
- Syncthing (for file sync)
- Immich (for backup photos and videos)
- GitLab (for code hosting)
- Gitea (for code hosting)
- pfSense/opnSense (for router/firewall)
- Jenkins (for CI/CD)
- Grafana (for monitoring)
- Prometheus (for monitoring)
- ArgoCD (for GitOps)
- FluxCD (for GitOps)
- Tailscale (for VPN)
- Nginx (for proxy)
- Traefik (for proxy)
- piHole (for ad-blocking DNS)
- piVPN (for VPN)
- Unifi Controller (for managing Unifi devices)
- Home Assistant (for home automation)
- Plex (for media server, streaming)
- Transmission (for torrenting)
- Sonarr (for TV shows)
- Radarr (for movies)
- Lidarr (for music)

### References
- [https://www.reddit.com/r/homelab/comments/nv1tww/which_optiplex_do_people_recomend_to_start_with/](https://www.reddit.com/r/homelab/comments/nv1tww/which_optiplex_do_people_recomend_to_start_with/)
> I run a Dell Optiplex 7040 micro with a Intel 6500T CPU, 16GB Ram, 500Gb SSD, 1TB HDD with Proxmox.
  I am running Home Assistant, piHole, piVPN, Unifi Controler, TrueNAS and use ~5-10 % CPU and 8GB Ram. So plenty resources for future projects. Cost me about 300€ in Germany. Uses about 16-18W with my typical load on it.
