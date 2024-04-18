---
tags:
  - DevOps
date: 2024-04-17T22:59
---
<!-- 2024-04-17-2259 (April 17, 2024 10:59:14 PM) -->

# DevOps Prerequisites

## Networking Basics
- there can be multiple Network Interfaces and Adapters in one Machine

## Switching, Routing, and Default Gateway

- Switching --> can be the `192.168.1.0`
  - Switch - where all devices connect via the ethernet (eth0 network interface)
- Routing --> how devices connect and communicate within a network
  - Routers - commonly configured as the "`.1`" `192.168.1.1`

### Default Gateway (commonly as Routers)
- `default` or `0.0.0.0` - means any IP
- `ip route add <destination> via <gateway>`
	- can be read as: from `<gateway>` to `<destination>`
	- `<gateway>` can send packets to `<destination>`
`route` outputs something like this kernel routing table:
```bash
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.2.1     255.255.255.0     UG    600    0        0 wlan0
default         192.168.2.1     255.255.255.0     U     0      0        0 eth0
```
![[Pasted image 20240418213506.png]]
- see example when having 2 routers
	- one for internal
	- one is connected to the internet
![[Pasted image 20240418214046.png]]


### **Commands Summary:**
- `ip link` - lists all network interfaces
- `ip addr` - lists all IP addresses assigned to those network interfaces
  - useful for seeing the IP address of the current machine (assigned/attached to the network interfaces)
- `ip addr add 192.168.1.10/24 dev eth0` - add/set IP address on the network interfaces
*NOTE: to persist changes, edit `/etc/network-interfaces` file or something similar instead*
- `ip route` or `route` - lists the routing table
- `ip route add 192.168.1.0/24 via 192.168.2.1` - adds routing from gateway (`via <gateway>`) to destination (`... add <destination>`)
- `cat /proc/sys/net/ipv4/ip_forward` - checks if the host forwards IP (common if host is configured as a router)
  - 1 if true

*practical scenario:*
- ssh into a multiple machines to add their own IP addresses to a network interface
- and then try to ssh machines with the same IP range on each other (say app01 should be able to ssh into app02 and vice versa)
```bash
host> ssh app01
app01> sudo ip addr add 172.16.238.15/24 dev eth0
app01> exit
host> ssh app02
app02> sudo ip addr add 172.16.238.16/24 dev eth0
app02> exit
# now test if app01 can ssh into app02 and vice versa
host> ssh app01
app01> ssh app02
# ... should be successful
app01> exit
host> ssh app02
app02> ssh app01
# ... should also be successful (ssh from app02 to app01)
```
- now what if app03 and app04 are in different IP range (say `172.16.239.0/24`)
```bash
# add IP ranges to app03 and app04 first (skipping exits...)
host> ssh app03
app03> sudo ip addr add 172.16.239.15/24 dev eth0
host> ssh app04
app03> sudo ip addr add 172.16.239.16/24 dev eth0
```
- how to ping app03 from app01 and vice versa (both servers in different IP ranges)?
--> then use host as a router:
```bash
# host IP address are 172.16.238.10/24 and 172.16.239.10/24
host> ip route add 172.16.238.0/24 via 172.16.239.10
host> ip route add 172.16.239.0/24 via 172.16.238.10
host> ssh app01
app01> ping app03 # should be successful
host> ssh app03
app03> ping app01 # should be successful
```

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


