---
tags:
  - Security
date: 2024-04-14T19:35
---
<!-- 2024-04-14-1935 (April 14, 2024 07:35:37 PM) -->
  
# Harden Reverse Proxy
- if you want to host something or exposing something to the internet.

## **May harden your reverse proxy in the following areas:**
  
### **SSL/TLS configuration**
- https://www.ssllabs.com/ssltest/
### **HTTP Security Headers**
- https://securityheaders.com/
### **Monitor logs and implement an IPS like fail2ban or crowdsec**
- https://blog.lrvt.de/fail2ban-with-nginx-proxy-manager/
- https://blog.lrvt.de/configuring-fail2ban-with-traefik/

### Implement a firewall in front of your reverse proxy... 
- ensure you only accept packets from Cloudflare's IPv4 and IPv6 ranges. 
- Otherwise, someone may enumerate your origin WAN IP (via censys search or shodan) and bypass CF. 
- Alternatively, directly use CF tunnels and skip port forwarding

### Request wildcard ssl certificates... 
- instead of individual ones to prevent disclosing your subdomains in the common name field of certificate transparency logs (see https://crt.sh).

If your sites are just static, you may skip everything and use GitHub Pages. Just an idea.
