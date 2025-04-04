---
tags:
  - Wizardry
  - Hacking
  - How-To
date: 2024-02-06T18:37
title: Security and Hacking Wizardry
---
<!-- 2024-02-06-1837 (February 6, 2024 6:37 PM) -->

# Security and Hacking Wizardry

## Scanning ports, identify vulnerabilities, services running, etc. 
### Identify Open Ports
- use nmap or any
```bash
nmap -p 1-65535 <ip-addr> # standard scan
nmap -p 1-65535 -T4 -A -v <ip-addr> # aggressive scan with OS detection (also identifies services)
```
- see more about nmap: [nmap-wizardry](nmap-wizardry.md)

### See services running
- once identified, identify the services running behind the ports
```bash
netstat -tulpn
# to find the service running behind the specific port, -n is to show the line numbers
netstat -tulpn | grep -n <port> 
# use sudo if not all processes/services could be identified
sudo netstat -tulpn | grep -n <port> 
# another way to find (all, numeric, ports, udp, tcp, wide):
sudo netstat -anputW
```

- with nmap:
```bash
sudo nmap -sS -sV -O -PN -p-
```

### What the Person would be able to do depends on whats running there, if anything.
> note that, what a person can do depends entirely on what service they're able to reach behind your http port. You could have a web server there, an ftp server, or a tea pot. 
> the possibility of exploitation is fully dependent on the services running behind the ports or you are hosting behind them.
> The port itself does not matter for this, nor does it matter if the connection between client and server is encrypted. 
> All what matters is that there is some application which accepts data on this port and acts on these data in an insecure way.

## Securing
Yes. Block DNS level telemetry with Adguard Home/PiHole and block HTTP level telemetry with Portmaster

AdGuardHome https://adguard.com/en/adguard-home/overview.html

PiHole
https://pi-hole.net/

Portmaster
https://safing.io/

https://www.reddit.com/r/privacy/s/CjyjcVL2gg 
# References:
- [https://security.stackexchange.com/questions/270701/could-someone-hack-into-my-website-through-http-ports](https://security.stackexchange.com/questions/270701/could-someone-hack-into-my-website-through-http-ports)
