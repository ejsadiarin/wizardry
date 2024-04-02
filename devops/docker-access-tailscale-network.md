---
id: docker-access-tailscale-network
aliases: []
tags:
  - Docker
  - Tailscale
  - How-To
date: 2024-03-03-1823 (March 3, 2024 6:23 PM)
title: Docker Containers Access Tailscale Network
---

# Docker Containers Access Tailscale Network
- docker containers as part of tailnet (tailscale network)

## Solution 1: Create a Docker Network that uses the host's Tailscale interface
- Something like:
```bash
docker network create --driver=macvlan --subnet=100.64.0.0/10 --gateway=100.64.0.1 --ip-range=100.64.0.2/24 -o parent=tailscale0 tailscale
```
- Your containers will use the network to connect to other hosts on the Tailscale network.
See https://docs.docker.com/network/macvlan/

## Solution 2: Use the host's network namespace
- If you create a docker network (let's call it "private-net") and place your tailscale container and the monitor container in that network, you can "advertise the routes" for that network's subnet via your tailscale up command, and then you turn it on in the tailscale admin.

- The tailscale up command would be something like:
```bash
docker exec tailscale tailscale up --authkey=<authkey> --advertise-routes=<subnet/mask>
```
- Then log into the tailscale admin, and to the right of your tailscale node in the list of "Machines" click the "...", then "Edit route settings...", and enable <subnet/mask> under "Subnet routes".

**So, 2 parts:**
1. "advertise routes" with the private docker network subnet and mask.
2. Enable the subnet route in the tailscale admin.

### If there is a permission error with Solution 2:
**Problem:**
> When I run the command though, I get
>
> ```bash
> docker exec tailscale tailscale up --advertise-routes=172.10.0.0/16 --authkey=<MY_AUTHKEY>
> Warning: net.ipv6.conf.all.forwarding is disabled.
> Subnet routes won't work without IP forwarding. 
> See https://tailscale.com/kb/1104/enable-ip-forwarding/ 
> Success.
> ```
>how do I enable the ip forwarding from inside the container?
>
> If I try to execute the commands per the guide:
> ```bash
> echo 'net.ipv4.ip_forward = 1' | sudo tee -a /etc/sysctl.conf
> 
> echo 'net.ipv6.conf.all.forwarding = 1' | sudo tee -a /etc/sysctl.conf
> 
> sudo sysctl -p /etc/sysctl.conf
> ```
>
> I get:
> ```bash
> bash-5.1# echo 'net.ipv4.ip_forward = 1' | tee -a /etc/sysctl.conf
> net.ipv4.ip_forward = 1
> 
> bash-5.1# echo 'net.ipv6.conf.all.forwarding = 1' | tee -a /etc/sysctl.conf
> net.ipv6.conf.all.forwarding = 1
> 
> bash-5.1# sysctl -p /etc/sysctl.conf
> sysctl: error setting key 'net.ipv4.ip_forward': Read-only file system
> sysctl: error setting key 'net.ipv6.conf.all.forwarding': Read-only file system
> ```

**Solution:** 
- run as sudo/root in the host (not in the container)
```bash
echo 'net.ipv4.ip_forward = 1' | sudo tee -a /etc/sysctl.conf
echo 'net.ipv6.conf.all.forwarding = 1' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p /etc/sysctl.conf
```

# References:
- [https://www.reddit.com/r/Tailscale/comments/swa1dk/docker_containers_access_to_tailscale_network/?onetap_auto=true](https://www.reddit.com/r/Tailscale/comments/swa1dk/docker_containers_access_to_tailscale_network/?onetap_auto=true)
