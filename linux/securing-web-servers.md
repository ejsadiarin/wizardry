---
tags:
  - Security
date: 2024-04-14T19:42
---
<!-- 2024-04-14-1942 (April 14, 2024 07:42:23 PM) -->

# Securing Web Servers
```
    Use Cloudflare Tunnels for CDN/VPN so no exposed ports

    SSH key access only to the server (one of the first things I did)

    Only open port 80 and 443 and use nginx reverse proxy.
    - redirect all users from any device, cli program or scripts securely to HTTPS (443)

    Don't expose SSH or Docker.

    Keep everything up to date.

Proxmox > LXC containers > turnkey Linux > fail2ban > docker > Nginx proxy manager
^ Repeat the above until the docker 
^ then install the cloud flare tunnel daemon 
^ then expose NPM via CF tunnels.
```

- then see [How to harden reverse proxy](wizardry/linux/harden-reverse-proxy)
  - mentions: `fail2ban`, etc.
- then see geoblocking ips by country
  - see [geoip-shell](https://github.com/friendly-bits/geoip-shell)

# Resources
- [Next step: Self hosting a couple of sites and exposing them to the world... what do I need to know?](https://www.reddit.com/r/selfhosted/s/caXmPv6Gxb)
