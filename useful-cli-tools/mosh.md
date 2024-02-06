---
title: Mosh - The Mobile Shell
date: 2024-02-06-1707 (February 6, 2024 5:07 PM)
tags:
- Linux
- SSH
---

# Mosh - The Mobile Shell
- great alternative to `ssh`
- UDP version of `ssh`

# How it works?
(1) establish connection to the remote server via `ssh`
(2) starts `mosh-server` process on that remote server
(3) `mosh-server` process establishes listener or listens for UDP packets and sends a AES-128 secret key back to the client over `ssh`
(4) SSH connection shutdowns and begins a terminal session via UDP
more details here: [How Mosh Works?](https://github.com/mobile-shell/mosh?tab=readme-ov-file#how-it-works)

# References
- [https://github.com/mobile-shell/mosh?tab=readme-ov-file#how-it-works](https://github.com/mobile-shell/mosh?tab=readme-ov-file#how-it-works)
