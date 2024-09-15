---
date: 2024-09-15T11:32
tags:
  - Homelab
  - How-To
---

<!-- 2024-09-15-1132 (September 15, 2024 11:32:39 AM) -->

# How to Setup Tailscale VPN with Caddy Reverse Proxy

- use Caddy as a reverse proxy
- then provide it a whitelist of ip addresses/subnets to cover usage patterns
- if you like to use DNS then see below situation

## Say setting up Vaultwarden with a reverse proxy like Caddy

If you can use Tailscale on all your devices, it’s a secure choice.

If you'd like to use DNS:

1. Use a reverse proxy with any web server like NPM, Caddy, etc., and issue an SSL certificate for the domain.
2. Create a DNS entry like vault.yourdomain.com that points to the Tailscale IP in your DNS zone.
3. Set up a proxy pass from IP/SERVICE:PORT to your Vaultwarden instance in your proxy manager.

This should help you resolve your Vaultwarden service from the internet with SSL, but only when connected to tailscale n/w.

P.S. If you have a local DNS server, you can access Vaultwarden from home without needing to connect to Tailscale.

# Disable `/admin` endpoint of the domain

Only allow `192.168.1.1/16` or Tailscale Subnet `100.X.X.X`

- so that it will be restricted only to private IP addresses/own VPN address
