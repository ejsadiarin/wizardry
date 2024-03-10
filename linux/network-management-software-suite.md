---
title: Network Management Software Suite
date: 2024-03-10-1540 (March 10, 2024 3:40 PM)
tags: 
- Networking
---

# Basic Network Management Suite
- monitor (and set up the right alerts):
  - network devices uptime
  - network utilization (bandwidth on some links)
  - internet access latency (`smokeping` or similar)
  - keep track of increased error counters and interface counters

- should consist of:
### 1. IPAM (IP Address Management) 
- `netbox`, `phpIPAM`
### 2. NMS (Network Monitoring System)
- LibreNMS (has alerting as well)
### 3. SYSLOG server
- can be any linux box running `rsyslog` or `syslog-ng`
- `graylog` if have serious volume of data
### 4. DNS (nameserver)
- network devices should ALWAYS have a DNS entry.

## Depending on size of the business, also have:
### Centralized AAA server
- RADIUS (freeradius)
### Jumphost
- a jumphost to one server from which you accept logins to network management infrastructure
### VPN-server
- for remote connections
- independent from whatever the business uses
### Ansible (Configuration Management)
- building configs, patching configs, updating firmware, etc.

## Also learn how to:
- Interface with web-APIs (Python, Go, etc.)

## TAKE NOTE:
### Keep the `IPAM` the **Source of Truth**
- if reality and IPAM doesn't match, adjust reality (reality needs to adapt)
- if something can be done with data from IPAM, do it with data from IPAM
