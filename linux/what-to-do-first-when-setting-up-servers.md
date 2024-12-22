---
tags:
  - How-To
  - Wizardry
date: 2024-07-17T3:52
---
<!-- 2024-07-17 (July 17, 2024 3:52 AM Wednesday) -->

# What to do first when Setting up Servers 

1. Everything through Ansible:
- Ansible and SSH keys
- FIREWALL
- update and upgrade
- install docker through Ansible
- one Ansible role per docker stack:
	- Docker + Portainer or lazydocker - that way I've got a bit of an interface to work with.
		- Portainer, for update notifs and restart containers
		- Dockge, for managing compose files
	- Traefik - get the configs in order how I want them along with the base.
	- Authentik - add in my custom flows / stages, and then I start adding in my other containers and also link up Portainer to OAuth, and a middleware for Traefik.
	- Dozzle - for logs

2. Then *Reverse Proxy* (any: Traefik, NPM, etc.) so that way I can set up local domains and access stuff that way instead of trying to get around with ip/ports which is a pita.

3. Automate scripts and essentials (can be done through Ansible):

My setup is really simple if I were to deploy a new server: 

Scripted steps:

* SSH Keys
* Firewall rules (if needed)
* cron jobs
* updates+packages (docker)
* timezone
* Data folder structure
* Portainer
* custom rsync script
* nas mount folders
* custom motd
* nas mounts to fstab

Manual steps:

* copy data manually from backup (this should include portainer+all container data
* update single ip in cloudflare with new local ip that will redirect all local CNAME's (all cloudflare exposed services will use internal DNS and need no updates)

# Reference
- https://www.reddit.com/r/selfhosted/s/lac99L7gJw 

