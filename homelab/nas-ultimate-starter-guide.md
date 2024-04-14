---
tags:
  - Homelab
date: 2024-04-14T14:38
---
<!-- 2024-04-14-1438 (April 14, 2024 02:38:48 PM) -->

# NAS Ultimate Starter Guide

- I like the Wundertech https://www.wundertech.net/synology-nas-initial-setup-ultimate-guide/ and Spacerex NAS guides.

- Spacerex:
  - https://www.youtube.com/watch?v=T1xW97eyXB8 - complete setup guide
  - https://www.youtube.com/watch?v=o2ck1g3_k3o - remote access guide

# Securing NAS
1. Create a new admin account, and disable the one it comes with
2. use 2FA and complex passwords
3. DON'T EXPOSE NAS TO THE INTERNET! (don't open ports if you don't know what you're doing)
- if there is a reason that you need to expose it then: 
  - You need to understand how to properly secure it and provide "buffer" layers between the greater internet and the actual NAS. There is plenty of information online about how to do this.
4. INSTEAD, use a VPN like Tailscale, plain Wireguard, or something

# Resources
- recommended youtube source: [SpaceRex]()
- more about NAS in thread [here](https://www.reddit.com/r/synology/s/9o5jYe4c6q)
