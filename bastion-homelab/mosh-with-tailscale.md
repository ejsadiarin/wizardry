---
id: mosh-with-tailscale
aliases: []
tags:
  - Tailscale
  - Mosh
  - Linux
  - Homelab
  - Bastion
  - How-To
date: 2024-02-07-2235 (February 7, 2024 at 10:35 PM)
title: Mosh with Tailscale
---

# Using Mosh instead of SSH with Tailscale

- setup mosh and tailscale on the remote server first:
  - **NOTE:** idk if needed to open ports since tailscale has some sort of NAT traversal that allows connections to be made without opening ports and without ssh keys
```bash
tailscale up
# then login
# add tailscale ACL tags if necessary
# allow mosh port range: 60000:61000/udp on firewall (can have custom port on ranging from 60000 to 61000)
sudo ufw allow 60000:61000/udp # or any firewall manager (firewalld, iptables, etc.)
```

- enable tailscale ssh on the remote server:
```bash
tailscale set --ssh
```
- use mosh to connect:
```bash
mosh tailscale-username@tailscale-ip
# or if have custom port for mosh:
mosh -p 60123 tailscale-username@tailscale-ip
```
