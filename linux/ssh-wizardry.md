---
tags:
  - SSH
  - Wizardry
  - How-To
  - Security
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

# Securing SSH
> None of my Arch machines are accessible to the public, but I do use fail2ban on my servers to protect *more* than just SSH. 
> But TBH, for just SSH, fail2ban would be quite far down the list for a smart administrator.

### tldr on securing SSH:

1. Turn off password auth, enable key auth.  That eliminates 99.99% of risk and you could stop here.
2. Change SSHd to a different port. That eliminates 99.99% of *log spam* from random attacks. Feel free to stop here.
3. [Set SSHd to use more modern ciphers](https://infosec.mozilla.org/guidelines/openssh.html) to eliminate many dumb SSH bots
4. [Drop SSH user agents you'll never use](https://news.ycombinator.com/item?id=39852865) to eliminate really dumb SSH  bots
5. Use fail2ban/sshguard to block IPs that have multiple failed attempts. If you've done #1, this isn't  necessary. If you've done #2, this won't prevent many log messages, because there won't be many.

From ref: https://www.reddit.com/r/archlinux/s/smyPx2Bp5F

