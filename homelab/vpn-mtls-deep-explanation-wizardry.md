---
date: 2024-09-15T11:41
tags:
  - Wizardry
---

<!-- 2024-09-15-1141 (September 15, 2024 11:41:49 AM) -->

# TLDR

- Both methods (VPN and mTLS) have pros and cons, so choose what to use based on usecases
  - see [Usecases for VPN](#usecases-for-vpns)
  - see [Usecases for mTLS](#usecases-for-mtls)
- NOTE: that _some apps_ don't support mTLS, but it is getting supported quickly (Immich recently added support for mTLS)

Simply put,

> Mutual TLS is not the solution for everyone. I have a very simple usecase, about 50 different web apps need to be exposed, so I just use mTLS.
>
> At the same time, I do have WireGuard if I need SSH access. My other users do not need SSH, so I only give them access to the web services over mTLS.

# Some "Misunderstanding" about these Services

1. If you use TailScale, it is only an outbound connection from your home so no ports are exposed.

   - the coordination server still open ports (temporary UDP ports) to make connections seem P2P
   - With TailScale, TailScale itself exposes ports (Ports are exposed at TailScale).
   - You authenticate and connect to those ports, which then connect you back to the reverse connection from your home.
   - If your security requirements and threat model allow for using TailScale then it's totally fine to use it, - but the idea that TailScale doesn't expose ports is a half-truth.

see a deep dive of how tailscale works in a section [here](#deep-dive-wireguardtailscale-vpn)

2. If you use a reverse proxy the way OP does, attackers will be able to scan your web server, identify web server vulnerabilities, and pop into your network!

   - No. mTLS requires the attacker to have a valid private key to authenticate to the reverse proxy.
     - If a valid private key and certificate are not there, then the attacker cannot begin scanning the web app.
     - The mTLS handshake happens before the attacker can probe the web service.
     - If you don't believe me, use [WireShark and see how a TLS connection works](https://www.youtube.com/watch?v=a9eVf2uleaA).
   - Even over regular TLS, you will see that the TLS connection happens first, before any HTTP traffic is transmitted.
   - Better yet, host your own mTLS instance, scan 443 without a private key and see what data you get back.

3. If you expose a port, even if it requires a private key to connect to it, you are less secure than if you use WireGuard, which requires an authenticated packet before it responds.

   - No. WireGuard allows you to avoid confirming or denying that a port is open,
     - since it's over UDP and most systems don't respond if you try to interact to a nonexistent service over UDP.
   - This, on its own, does not make WireGuard more secure than say TCP OpenVPN or mTLS.
   - It does, however, prevent people looking at your IP address from knowing if you are running some sort of authentication-required service.
   - If this increases your risk, then you can choose to use WireGuard, instead, but this is not the case for a vast majority of people.

For more information on mTLS, see [Hello mTLS](https://smallstep.com/docs/mtls/) by the awesome people at Smallstep.

They also have a cool tutorial on using Yubikeys with mTLS [here](https://smallstep.com/blog/access-your-homelab-anywhere/) to connect back to the homelab, similar to how OP is running his homelab.

The great part about using Yubikeys for mTLS is it allows you to have a hardware-backed, two-factor authentication method at layer 6, rather than traditional MFA which is at layer 7.

- This allows MFA with a lower attack surface, since the attacker can't look for any web vulnerabilities to bypass MFA.

## Deep Dive Wireguard/Tailscale VPN

Tailscale does not open ports through your firewall settings, but

- it does use [NAT Traversal](https://tailscale.com/blog/how-nat-traversal-works)
- with a technique called [UDP hole punching](https://en.wikipedia.org/wiki/UDP_hole_punching).

- Here is a Whitepaper that also describes how this works: [https://bford.info/pub/net/p2pnat.pdf](https://bford.info/pub/net/p2pnat.pdf)

The short summary is that your firewall will usually allow arbitrary outbound connections over UDP,

- but since UDP doesn't allow the firewall to know the state of the connection,
- when an outbound connection occurs, the firewall will simply keep the NAT mapping in memory and let traffic flow back to your host over that UDP port.

If you have an intermediary (like Tailscale) then you'd get your homelab's NAT mapping from Tailscale, and be able to connect back to your homelab.

If you've ever made a Whatsapp or Signal call, they also do UDP hole punching which gets you a direct connection to who you're calling, even behind NAT.

- Yes, it is IP-specific. I think the idea is that after you get the info from Tailscale, Tailscale would inform BOTH you (as in the client) and the homelab to connect to each other given your respective ports and IPs. When they do, that would then cause the hole punch.

> The port will only accept packets from the intended recipient, and that's not an "open port" in the usual meaning.

- This is how WireGuard works. If you open the port on your firewall to WireGuard, the WireGuard "port only accept packets from the intended recipient" (meaning cryptographically-authenticated clients). So, functionally, there is no difference between Tailscale and a self-hosted WireGuard instance.

- You are trusting wireguard to properly discard any malicious packet from the internet, though.

> If there's a bug in wireguard or in the OS you'd be a fairly easy target. With tailscale the malicious packet would have to come from one of the peer IPs.

- **A bug like the one you describe would need to be in the crypto algorithm itself, which I would argue is unlikely.**
- Take a look at the WireGuard code, it is fairly simple to read and it's short enough to do it in a few hours.
  - If you've ever done AES-256-GCM (or another crypto protocol with crypto-auth), you'd notice that any unauthenticated message causes an error to occur; i.e., unlike other crypto modes that do not do authentication, you won't get garbled text back.
  - WireGuard's crypto is similar in this aspect. Any message that is not cryptographically encrypted with a key that is trusted by WireGuard is discarded at the crypto-level.

If you've ever used authenticated encrypted radio, it's a very similar principle.

### Usecases for VPNs

- plain SSH access rather than a web shell
- A better solution when accessing an entire network rather than accessing individual services

## Deep Dive mTLS

For mTLS, you would generate a cert for each person you need to access your homelab (and if that's only you, then it's just 1 cert you need to generate).

Then when you access any service, you don't need to start a VPN connection first, you just open the service.

For me it's a lot more convenient to go mTLS the majority of the time. When I want to connect to my entire home network I'll use a VPN.

The setup is very easy, for Caddy you just change a site's config to look like this:

```caddyfile
https://securesite.mydomain.com {
    tls {
        client_auth {
        mode require_and_verify
            trusted_ca_cert_file /data/client_certs/client.crt
        }
    }
    reverse_proxy internal_server.lan:3020
}
```

- The initial setup, among other things, needs a valid CA setup(likely a personal/private one), so that part's a bit complicated if you don't know how that works.
- Issuing a cert to each device is fairly straightforward, though, and not much more work than you'd need to do to configure each device for a VPN or such.

### Usecases for mTLS

1. When proxying through Cloudflare, I use [mTLS](https://developers.cloudflare.com/ssl/origin-configuration/authenticated-origin-pull/) to safeguard my origin servers.
2. When you don't need plain SSH access (rather than a web shell) on the host (use VPN solution if you want SSH access)
3. If all you're doing is providing web apps for yourself and others, mTLS should be great for that,
   - especially if you use PIV since that gives you hardware-backed MFA.

# Other things

> There is more thought put into security here than 90% of businesses.
>
> I think most people are fine exposing a reverse proxy and building up to 2FA, no attacker really cares about a jellyfin server.

# References

[This amazing post from s professional security engineer on r/selfhosted](https://www.reddit.com/r/selfhosted/comments/1ffytgc/in_response_to_i_expose_all_my_services_to_open/?share_id=5MHNWFiq1Ix-m2w45bkSN&utm_content=1&utm_medium=android_app&utm_name=androidcss&utm_source=share&utm_term=5)
