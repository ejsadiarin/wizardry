---
id: access-and-security-rule
aliases: []
tags:
  - Security
date: 2024-04-04T14:12
---
<!-- 2024-04-04-1412 (April 04, 2024 02:12:47 PM) -->

# Access and Security Rule
 This is my current policy:  


1. YOUR exclusive access to the local infrastructure and services: Use TailScale, WireGuard, or similar.  


2. PUBLIC access to one or more locally-hosted services: Use Cloudflare Tunnels.  


3. RESTRICTED access to one or more local services to a small, controlled group of people: Use Cloudflare Tunnels + Cloudflare Applications.  


All provide remote access without needing to expose any ports. A benefit of a Cloudflare Application is that the authentication happens at Cloudflare's servers, so my server is never touched until the user passes the Application authentication. Also, I set up some Access Rules (such as from what countries a user can connect) to further restrict access.  


Bonus tip: I have Kasm installed locally behind a Cloudflare Tunnel + Application with several "Server Workspaces" defined that point to several local resources (PCs, Servers.) This lets me remotely connect securely to these resources via RDP, VNC, and SSH through a Web Browser.

I would check out [Cloudflare tunnels](https://www.cloudflare.com/products/tunnel/). 

I purchased a cheap domain name to access my services on. All the traffic is routed via cloudflare tunnels. 

In the cloudflare dashboard you can enable SSL and cloudflare will issue certificates for the domain and encrypt the traffic between its server and the web browser.
