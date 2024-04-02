---
id: setup-mosh
aliases: []
tags:
  - Mosh
  - Linux
  - Server
  - How-To
date: 2024-02-06-1847 (February 6, 2024 6:47 PM)
title: How to Setup Mosh
---

# Use Cases
- perfect combination with `tmux` for persistent sessions
- `mosh` preserves connection even when switching networks or resuming from sleep

# Setup
- install `mosh` on BOTH client and remote server
```bash
sudo apt install mosh # or any based on package manager
```
- `ssh` into the remote server
  - NOTE: `mosh` requires `ssh` to establish the initial connection for auth

## On remote server do these steps
- allow `mosh` port: 60000:61000/udp on firewall
  - `mosh` searches for open ports in this range (60000 to 61000)
  ```bash
  # use `ufw` or `firewalld` or `iptables` or any firewall manager

  # check if firewall is active/running
  sudo ufw status
  # allow mosh port range
  sudo ufw allow 60000:61000/udp
  # for individual connection open only 1 port: 
  sudo ufw allow 60000:60001/udp # can be sudo ufw allow 60123/udp
  # check if opened
  sudo ufw status numbered
  # if not, reload firewall
  sudo ufw reload
  # check if opened
  sudo ufw status numbered
  ```

## On client do these steps
- connect to the remote server via `mosh`
```bash
mosh username@remote-server-ip
# or if have custom port:
mosh -p 60123 username@remote-server-ip
```
- should connect now

# Next Steps - Optional
