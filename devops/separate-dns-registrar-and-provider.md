---
date: 2024-09-14T02:23
tags:
  - Infra
---

<!-- 2024-09-14-0223 (September 14, 2024 02:23:24 AM) -->

# Separate DNS Registrar and DNS Provider

A service that your data goes through, is a service that can ban you if this data is not to their liking. Some legal stuff around that ofc but you get the idea.

Cloudflare had many incidents where it tried to force the customer to opt for a custom plan with their own price offer in order to continue the service with them.

If they block your account in that case, or just make it harder to access, and you have your domains there as well, you won't be able to easily switch the nameservers and go.

If your domains are on a separate service, that service only has your domains, nothing else,

- so very little reason to ban you or try to force you to pay their own custom pricing.

And if cloudflare doesn't play nice, if you account there gets blocked, or whatever else,

- you can easily go to the domain service, change nameservers, and move on.

I'm using cloudflare as an example here because of the recent incidents with them, but really it applies to anything.

> They can suspend your account based on activity going through them. Basically any service can do this not just cloudflare but this is why it's a good idea to separate domain registrar from the service that manages your dns

# Differences between DNS Registrar and DNS Provider (DNS Management Service)

1. DNS Registrar

   - A domain registrar is the service or company where you register your domain name (e.g., Porkbun, GoDaddy, Namecheap).
     - They manage the legal ownership of your domain, ensuring it’s properly registered and renewed.
   - Role: They primarily handle domain-related functions like renewing the domain, transferring it to another registrar, or updating the WHOIS contact info.
   - No direct control over DNS data: Registrars typically don't deal with how your domain points to specific IP addresses (that's DNS management) but rather give you the ability to point your domain to a DNS server of your choice.

2. DNS Provider (DNS Management Service)

   - A DNS provider handles where your domain's DNS records (A records, CNAMEs, MX records, etc.) are stored and managed. This is the service that tells the internet which servers your domain should resolve to. Cloudflare, for instance, provides DNS management along with additional services like DDoS protection and CDN.
   - Role: They control the actual functioning of your domain on the internet by resolving names (like www.example.com) to IP addresses. If you use Cloudflare’s DNS management, for instance, they control where your domain resolves on the network level.
   - Service Control: If you’re relying on a DNS service like Cloudflare, and they decide to suspend your account, you could lose control over how your domain is resolved, potentially cutting off access to your site.

## Why It’s Important to Separate Them

- Flexibility: If you use a DNS registrar that’s separate from your DNS management service, you can quickly change your nameservers and move your domain to another DNS provider if the DNS management service suspends or blocks your account.

- Reduced Risk: Keeping your domain registration with a company that focuses solely on domain ownership (e.g., Namecheap) reduces the chance of your domain being caught up in service-level disputes or issues (like pricing disputes or account suspensions) with the DNS management service.

- Easy Transition: If Cloudflare, for example, suspends your account, and you have both your domain registration and DNS management with them, you could have trouble switching nameservers and accessing your domain. By separating the two, you can easily go to your domain registrar, change the nameservers to a new DNS provider, and regain control.

# What to do?

- use separate like: Cloudflare and NameCheap or Porkbun exclusively

# References

- [This comment](https://www.reddit.com/r/webdev/comments/1bjfqse/comment/lfxuw0f/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button)
