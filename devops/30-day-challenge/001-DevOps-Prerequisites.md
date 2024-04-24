---
tags:
  - DevOps
date: 2024-04-17T22:59
---
<!-- 2024-04-17-2259 (April 17, 2024 10:59:14 PM) -->

# DevOps Prerequisites
- Linux
- Networking
- Application Basics (Compiling, Building, Packaging)
- Source Control Management (Git)
- Web Server
- Database Basics
- Security
- General Prerequisites like YAML and JSON/JSON Path
- 2 Tier Application

## Networking Basics
- there can be multiple Network Interfaces and Adapters in one Machine

### Switching, Routing, and Default Gateway

- Switching --> can be the `192.168.1.0`
  - Switch - where all devices connect via the ethernet (eth0 network interface)
- Routing --> how devices connect and communicate within a network
  - Routers - commonly configured as the "`.1`" `192.168.1.1`

#### Default Gateway (commonly as Routers)
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


*Commands Summary:*
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

#### Port-forwarding
- HOST (or LOCAL) and GUEST (or REMOTE)
- if want a service from GUEST (ex. ssh on port 22 from GUEST) to HOST
  - then port-forward port 22 (from GUEST) to port 2222 (from HOST)
  - now we can access the ssh of guest on port 22 via the port 2222 of HOST using loopback address of HOST (`127.0.0.1`):
  ```bash
  # assuming we only have a NAT adapter
  host> ssh 127.0.0.1 -p 2222
  ```
- same concept applies to all machines, VMs, etc.

#### Network Interfaces
- think en0p1s3, wlan0, tailscale0, etc.
  - one for: ethernet (physical connection), wireless connection, and vpn mesh network interface 

#### Adapters
- configures how machines connect with each other
- NOTE THAT: there can be 2 or more adapters in one VM/machine (think Host-only+NAT)

insert pic here

##### Different Types
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
  - VMs/Machines DO NOT HAVE their own IP addresses the Host machine acts as a router
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

### DNS
- main purpose is to resolve IPs of domains

*how to add domains mapped with IP addresses?*
- say we want to ping `db` with IP: `192.168.1.11`
- edit `/etc/hosts` file to configure the domain names mapped to their specific IPs
![[Pasted image 20240419171809.png]]

#### Name Resolution
- in Host A:
	- it doesn't care whatever the hostname is in Host B
		- the `/etc/hosts` file is the only source of truth in Host A (only needs the IP address)
- all servers/hosts should have proper configuration pointing to the IP addresses of other servers/hosts (but what if it became big/you have A LOT of servers/hosts at scale?)
![[Pasted image 20240419171906.png]]

#### why DNS?
- Name Resolution --> editing `/etc/hosts` file for **every** server is TEDIOUS and repetitive at scale (imagine having 7769 servers/vms and connecting them altogether)

*until we have decided that we need to move this into ONE server only called:*
#### DNS server
- instead of multiple `/etc/hosts` file edits for every server, we only have ONE that is stored inside this DNS server
- every server is pointed to this DNS server whenever we want to resolve a domain name/subdomain

*How to point HOST to a DNS server?*
- we look at a server's `/etc/resolv.conf`
	- and configure the nameserver and IP address (of the DNS server)
![[Pasted image 20240419173435.png]]

*in what case can we use /etc/hosts?*
- for test servers
- for any cases we do not need other servers to know about
![[Pasted image 20240419173815.png]]

*in HOST A, what if we have configured the same test server IP in local /etc/hosts and in the remote DNS server?*
- `/etc/nsswitch.conf` - determines the order of where to look at first when resolving hostnames/IPs
	- below we see the `hosts: files dns` as the order, 
		- meaning HOST A will first look at local `/etc/hosts` files
    - if there is no entry, then HOST A will look at the DNS server
![[Pasted image 20240419174050.png]]

*we can forward unknown hosts to a Name Server (DNS server) like in Google's 8.8.8.8*
- instead of having `nameserver: 8.8.8.8` in HOST A (telling HOST A to also connect to Google's DNS server)
	- we can have a DNS server that forwards all unknown requests to Google's DNS server `8.8.8.8`

![[Pasted image 20240419182758.png]]

#### Domain Name
given: `www.google.com`
- www -> subdomain
- google -> domain name itself
- .com -> Top Level Domain Name
- . -> root

![[Pasted image 20240419182631.png]]

#### How DNS servers work to resolve domain names?
*in public:*
- from a client or server request (say `apps.google.com`):
1. request goes through local `Org DNS` first
	- if doesn't find the domain name there (or when domain name isn't cached), it goes outside:
2. to the `Root DNS` in the public
3. then to the Top Level DNS `.com DNS` (can be `.net DNS`, `.org DNS`, etc.)
4. finally, to the `Google DNS` that will resolve the request to `apps.google.com` 
  - then it goes back to local again
**NOTE: the `org DNS` will ==cache the domain name== after it is resolved successful so that it is fast and less overhead when the domain name is requested again**
![[Pasted image 20240419183325.png]]


##### Search Domain

*in private/local (`org DNS`, think companies or homelabs):*

- **`org DNS` - has `/etc/hosts` configured to have IPs mapped to subdomains:**
in `/etc/hosts`:
```conf
192.168.1.10  web.mycompany.com
192.168.1.11  db.mycompany.com
192.168.1.12  nfs.mycompany.com
192.168.1.13  web-1.mycompany.com
192.168.1.14  sql.mycompany.com
```

- **servers/hosts - has `/etc/resolv.conf` configured to know which DNS server to connect to**

###### without `search` configured in `/etc/resolv.conf`
in `/etc/resolv.conf`:
```conf
nameserver   192.168.1.100
```
- ping will only work if it matches the whole domain
```bash
# based on the config above in `/etc/hosts`
ping web # will not work
ping web.mycompany.com  # will work
```

###### with `search` configured in `/etc/resolv.conf` (mapped to root_domain_name: `mycompany.com`)
in `/etc/resolv.conf`:
```conf
nameserver   192.168.1.100
search       mycompany.com prod.mycompany.com
```
- HOST will search in `mycompany.com` OR `prod.mycompany.com`
- the subdomains of `mycompany.com` are automatically configured/mapped
  - so any requests of the host to `mycompany.com` will be resolved
- using `ping` will automatically resolve the subdomain, even if root domain name is included
```bash
# can ping both successfully:
ping web
ping web.mycompany.com
```
![[Pasted image 20240419192928.png]]

#### Commands for DNS servers
- `ping` - most simple, will resolve the local `/etc/hosts` file
- `nslookup` - queries domain name from a DNS server
  - will **only** query domain names FROM A DNS SERVER
  - does not consider entries in the **local** `/etc/hosts` file
  ```bash
  host> nslookup www.google.com
  Server: 8.8.8.8
  Address: 8.8.8.8#53

  Non-authoritative answer:
  Name:    www.google.com
  Address: 172.217.0.132
  ```
- `dig` - same like `nslookup`
  - more detailed output

#### DNS Summary
**best setup:**
- `/etc/resolv.conf` - when configuring DNS (on HOST)
	- tells which DNS server to connect to
- `/etc/hosts` - when configuring domain names (on DNS server)
  - on `org DNS`, etc.
	- maps IP addresses to domain names, including subdomains

## Application Basics (Compiling, Building, Packaging)
### Java
- `jdb`
- `javac`
- `jar`
- `javadoc`
### Nodejs
- `node`
- `npm`
- `nvm`
### Python
- `pip` or `pipx`
## Source Control Management (Git)
## Web Server
## Database Basics
## Security
## General Prerequisites like YAML and JSON/JSON Path
## 2 Tier Application
