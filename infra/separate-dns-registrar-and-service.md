---
date: 2024-09-14T02:23
tags:
  - Infra
---

<!-- 2024-09-14-0223 (September 14, 2024 02:23:24 AM) -->

# Separate DNS Registrar and DNS Service Provider

A service that your data goes through, is a service that can ban you if this data is not to their liking.

Some legal stuff around that ofc but you get the idea.

Cloudflare had many incidents where it tried to force the customer to opt for a custom plan with their own price offer in order to continue the service with them.

If they block your account in that case, or just make it harder to access, and you have your domains there as well, you won't be able to easily switch the nameservers and go.

If your domains are on a separate service, that service only has your domains, nothing else,

- so very little reason to ban you or try to force you to pay their own custom pricing.

And if cloudflare doesn't play nice, if you account there gets blocked, or whatever else,

- you can easily go to the domain service, change nameservers, and move on.

I'm using cloudflare as an example here because of the recent incidents with them, but really it applies to anything.

> They can suspend your account based on activity going through them. Basically any service can do this not just cloudflare but this is why it's a good idea to separate domain registrar from the service that manages your dns

# What to do?

- use separate like: Cloudflare and NameCheap or Porkbun exclusively

# References

- [This comment](https://www.reddit.com/r/webdev/comments/1bjfqse/comment/lfxuw0f/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button)
