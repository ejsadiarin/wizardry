---
tags:
  - DevOps
date: 2024-04-17T22:59
---
<!-- 2024-04-17-2259 (April 17, 2024 10:59:14 PM) -->

# DevOps Prerequisites

## Networking Basics
- there can be multiple Network Interfaces and Adapters in one Machine

### Port-forwarding
- HOST (or LOCAL) and GUEST (or REMOTE)
- if want a service from GUEST (ex. ssh on port 22 from GUEST) to HOST
  - then port-forward port 22 (from GUEST) to port 2222 (from HOST)
  - now we can access the ssh of guest on port 22 via the port 2222 of HOST using loopback address of HOST (`127.0.0.1`):
  ```bash
  # assuming we only have a NAT adapter
  host> ssh 127.0.0.1 -p 2222
  ```
- same concept applies to all machines, VMs, etc.

### Network Interfaces
- think en0p1s3, wlan0, tailscale0, etc.
  - one for: ethernet (physical connection), wireless connection, and vpn mesh network interface 

### Adapters
- configures how machines connect with each other
- NOTE THAT: there can be 2 or more adapters in one VM/machine (think Host-only+NAT)

insert pic here

**Different Types:**
1. Host-only Network 
- VMs/Machines can see and communicate with each other
  - VMs/Machines have their own IP addresses (in HOST/Internal Network)
  - the Host machine acts as a router
- is private or internal only (other machines in LAN or WAN cannot see the VMs/Machines)
- no internet connectivity
- can connect with other VMs/Machines without port-forwarding (think ssh)

2. NAT
- VMs/Machines CANNOT see and communicate with each other (isolated)
  - the Host machine acts as a router (one way outbound traffic only or internal machine to internet only)
  - VMs/Machines DO NOT HAVE their own IP addresses
- the Host machine acts as a router
- HAVE internet connectivity
- CANNOT connect with other VMs/Machines without port-forwarding (think ssh)

3. NAT Network
- VMs/Machines can see and communicate with each other
  - the Host machine acts as a router
  - VMs/Machines have their own IP addresses (in HOST/Internal Network)
- HAVE internet connectivity
- CANNOT connect with other VMs/Machines without port-forwarding (think ssh)

4. Bridge
- VMs/Machines can see and communicate with each other
  - VMs/Machines have their own IP addresses (in LAN)
  - treated as a separate device/machine
- is public or part of the LAN, which means other machines in the same LAN can access the VMs/Machines
- HAVE internet connectivity

