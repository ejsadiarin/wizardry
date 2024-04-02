---
id: cloudflare-zero-trust-guide
aliases: []
tags:
  - Cloudflare
  - How-To
date: 2024-03-25-1508 (Mrach 25, 2024 3:08 PM)
title: Securely gain access to locally self-hosted services with Cloudflare Zero Trust
---

# Securely gain access to locally self-hosted services with Cloudflare Zero Trust
If you dig here a bit, I had some... troubles with Oracle cloud hosting, so I decided to go full on-premise, homelab self-hosting. But as you can imagine, I'd like to have access to some services, like Jellyfin or Zabbix from outside, not only from my own network.

This guide is the result of me searching for the best and most secure solution to that problem. It's not THE BEST, it's not THE MOST SECURE, as always you should use your own head and judgement. But I think for non-critical applications, such as self-hosted Zabbix should be more than fine.

What will be used here is Cloudflare Zero Trust, which is available for free on Cloudflare account. Note - I know for sure this works if you have domain registered via CF, not sure and no way to check if it's possible with different registrators.

So first things first - what it is and how it works?
I'll explain only bits important for this guide. So we will use Zero Trust Tunnel and Zero Trust Application Access.

Zero Trust Tunnel is essentially a site-to-site VPN between your network and Cloudflare Zero Trust servers. It enables CF to access your resources via local IP address, resolve them and assign them its own public IP. It takes your local IP addresses, creates a CNAME for your domain, then routes all traffic via CF public IPv4 and IPv6 addresses via their proxy to your designated local IPv4 addresses. If you nslookup your hostname, you'll only get CF from their IP Ranges

Zero Trust Application Access is a way to secure access to your applications, essentially enforcing going through loops and hoops on CF-hosted authentication page, before you can access even the login screen of your service

Let's setup a Tunnel
The way ZT Tunnel is set up is, you go from your Dashboard to Zero Trust -> Networks -> Tunnels. Here you can find a detailed instruction on how to install and connect cloudflared daemon, that acts as a connector and gateway to your home network. If you use virtualization, like Proxmox, I recommend setting up a small VM/CT, to act as your connector.
Once this is set up you Configure it and add Public Hostname. Here you can add local IP addresses of your services. And here are some caveats:

You want to select HTTP, not HTTPS. Cloudflare Zero Trust adds its own SSL/TLS reverse proxy, so in the end your services are behind HTTPS. If you have ONLY HTTPS (like with Proxmox) you want to select HTTPS, and in TLS settings enable "No TLS Verify" and "HTTP2 connection".

You HAVE to change default port from 80 to something else. For some reason, if your service is hosted on port 80, CF doesn't add it own SSL/TLS (eg. PiHole, where you can easly change it to something like 8100).

Now you can access your services from outside with hostnames you set up, but it's still not very secure - if you can access them, everyone can access them. And yes, if you're using a strong, complicated, random password the risk is minimized, but there are still exploits one can use. So let's fortify them further.

Cloudflare Zero Trust Access - suprisingly strong tool

Now what Access is I already explained. But what I didn't specify, how powerful it actually is. When you set it up and type in your service URL, you get redirected to cloudflareaccess.com domain, requiring you to authenticate. By default you have only access to OTP authentication via e-mail - you type in your email, are sent an access OTP, and only when you type it in, you get access for several minutes/hours/days. However, with ZT Access you have at least for or five levels of authentication:

You can set up multiple authentication methods: OTP, login via numerous sites (Facebook, GitHub, LinkedIn), OAuth2 (Google, Azure, Google Workspaces), OneLogin, OpenID, with timeout spanning from 1 minute to 1 month

You can restrict who can use these authentication methods, based on their e-mail addess, geolocation, IP range, service token

You can require user to state a justification on why they want to access the service, with manual review and accept

You can require using WARP (Cloudflare's own "sort of VPN", available at 1.1.1.1) to even access these authentication methods, and can also be connected with policies and restrictions from point 2

You can set up multiple WARP client restrictions, like does the user have encrypted hard drive, does it have a particular file, with particular name in specified location on their PC, does the user use WARP as is, or is logged in to your Zero Trust organization

So you can essentially set up something like "to access my zabbix, you have to have WARP enabled and logged in into organization, have encrypted hard drive, be located in Germany, your e-mail has to be on foo.bar, and you have to have this picture of a monkey named gibaccess.png on your desktop, then and only then, you can ask me, with proper justification to use your GitHub account to authenticate your access, but only for 1 hour". Suffice to say... it's powerful.

Buuuuut for our purpose I think OTP with restriction to only allow a single email address recieve the code will be more than enough. I will not describe the full process, if you self-host you're smart enough to understand what's going on. The most important - you want to create a new Application, select self-hosted, add domains from your Tunnel Public Hostnames, and set up policies - bare minimum is Include - Everyone, Require - Emails - your email only*.*
Once you set up Application, you have to go back to Tunnels, and reconfigure each Hostname, enabling Access and selecting Application you just created.

And now when you type in your service URL you'll be thrown into Cloudflare Access page, requiring to type in your email. You can type any email, but if you configured policy correctly, the code will only be sent if you provide your email. It'll take any other email, but won't send code.

That's all, hope you like it, and have fun using it :)

# Resources
[GUIDE: How to SECURELY gain access to your locally self-hosted services from outside with Cloudflare Zero Trust](https://www.reddit.com/r/selfhosted/comments/1bf9si9/guide_how_to_securely_gain_access_to_your_locally/?share_id=eLXIreh1-OBs3cjXq89Ow)
