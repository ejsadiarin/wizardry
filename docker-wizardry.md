---
tags:
  - Wizardry
date: 2024-04-20T3:53
---
<!-- 2024-04-20 (April 20, 2024 3:53 AM Saturday) -->

# Docker Wizardry


## Recommended Docker Configs
- [Adjusting Default Address Pool](/wizardry/devops/how-to-adjust-docker-default-address-pool.md)
  - TLDR: in `/etc/docker/daemon.json`, change `default-address-pools` --> `size` to 24

## Rootless Docker

You don't need rootless Docker if you follow simple principles:
  1. Only run images with a default user 1000:1000 (no root!)
  2. Only use bridge, macvlan or ipvlan
  3. Do not use privileged
  4. Use AppArmor profiles if you need cap add capabillities
  5. No access to Docker socket (includes things like Portainer)

*or just use podman (rootless containers by default)*

# Docker Networking

You can tell the container to use a IP that matches your actual home network, read the basics of Docker networking: https://docs.docker.com/network/

And then specifically about MACVLAN and IPVLAN networks:

https://docs.docker.com/network/drivers/macvlan/

https://docs.docker.com/network/drivers/ipvlan/

This is probably worth a read too: https://docs.docker.com/network/network-tutorial-macvlan/

Which one to use depends on a lot of factors, its up to you.

Basically you create a Docker network of that type with IP/Subnet that matches your home network (something like 192.168.1.0/24 maybe) and then you start your Firefox container with telling it to join that network, with a specific IP address to use.
## See Docker Security
https://docs.docker.com/security/security-announcements/ 

https://github.com/DoTheEvo/selfhosted-apps-docker 

## Docker Best Practices
https://www.reddit.com/r/selfhosted/s/HWqUMxyZRf 
