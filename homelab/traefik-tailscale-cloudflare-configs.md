---
date: 2024-10-26T22:16
tags: 
- Homelab
- Wizardry
---
<!-- 2024-10-26-2216 (October 26, 2024 10:16:30 PM) -->

# Traefik Tailscale Cloudflare Homelab

## Exposing/adding public services to Traefik

- example service to add: `blog.example.com` for blog page


Cloudflare
- add A or CNAME record: 
    Name: blog (which is blog.example.com)
    Target: <Server-IP> of the blog (public, or)

Traefik
- add rule for `blog.example.com` route via labels:
```yaml
services:
    blog:
        image: your-blog-image
        labels:
            - "traefik.enable=true"
            - "traefik.http.routers.blog.rule=Host(`blog.example.com`)"
            - "traefik.http.routers.blog.entrypoints=websecure"
            - "traefik.http.routers.blog.tls.certresolver=letsencrypt"
```
- add in traefik config:
```yaml
# under command:
- --entrypoints.websecure.http.tls.domains[1].main=${BLOG_DOMAIN}
- --entrypoints.websecure.http.tls.domains[1].sans=${SANS_BLOG_DOMAIN}
```
- then in `.env`
```
BLOG_DOMAIN=example.com
SANS_BLOG_DOMAIN=blog.example.com
```

## Firewall Configuration

- Make sure to open all ports first
long story short: i configured my firewall to only accept on ports 22, 80, and 443 AND the foking acme challenge ain't resolving (or rather dns port 53 is 'misbehaving'), basically i cannot get new lets encrypt certs because of it.

##  
