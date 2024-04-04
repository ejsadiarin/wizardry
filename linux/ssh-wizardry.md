---
id: ssh-wizardry
aliases: []
tags:
  - SSH
  - Wizardry
  - How-To
date: 2024-03-12T15:07
title: SSH Wizardry
---
<!-- 2024-03-12-1507 (March 12, 2024 3:07 PM) -->

# SSH Wizardry (The Only Guide You'll Ever Need)
- install `openssh`

- to make a machine "`ssh`-able", enable `sshd`:
```bash
# for systemd
sudo systemctl start sshd
sudo systemctl enable sshd # to start on boot
```

- with `tailscale`, you have a VPN that can be used to `ssh` into your machine from anywhere in the world.
  - even behind a NAT or firewall, and without `authorized_keys`
```bash
tailscale status

# on any terminal (even on android termux)
ssh <username>@<tailscale-ip>

# from machine to termux
ssh -p 8022 <username>@<tailscale-ip>
```
