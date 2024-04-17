---
tags:
  - How-To
  - Homelab
date: 2024-04-17T15:47
---
<!-- 2024-04-17-1547 (April 17, 2024 03:47:00 PM) -->

# GUIDE: How to Setup AdGuard Home - Network-wide Ad Blocking

*this guide is from [Akash](https://akashrajpurohit.com):*
- As I mentioned in my previous post, this week I am sharing about AdGuard Home, a network wide ad blocking that I am using in my home lab setup.

Blog: https://akashrajpurohit.com/blog/adguard-home-network-wide-ad-blocking-in-your-homelab/

I started with Pi-hole and then tried out AdGuard Home and just never switched back. Realistically speaking, I feel both products are great and provide similar sets of features more or less, but I found AGH UI to be a bit better to the eyes (this might be different from people to people).

The result of using this since more than a year now is that I am pretty happy that with little to no config on client devices, everyone in my family is able to leverage this power. 

# Pair with Tailscale
- [Pair this with Tailscale](https://akashrajpurohit.com/blog/adguard-home-tailscale-erase-ads-on-the-go/) and I have ad blocking even when I am not inside my home network, this feels way too powerful, and I heavily use this whenever I am travelling or accessing untrusted network. # Integrate Ansible playbooks to Keep Two Instances in Sync
- see [adguardhome-sync docker](https://github.com/bakito/adguardhome-sync)

# Integrate Ansible
Running two instances of any DNS solution on different hardware is a must, unless you configure your DHCP to give out a public DNS as a backup.

Also, Adguard Home has a really good API, so I keep my two instances in sync with an Ansible playbook. In fact I only ever log into the GUI to admire the stats, all my config is defined in an Ansible Inventory. This also means I can blow up the containers running it and I can have Ansible rebuild everything from the ground up.

Moreover, I use keepalived which creates a virtual IP and can fail between my pihole instances. I have one instance in a vm and another on a raspberry pi. Separate hardware is nice so you can update one and still have internet. Techno Tim has a good video on a setup https://technotim.live/posts/keepalived-ha-loadbalancer/

I use orbital sync to keep consistency between the cold and hot instances

# References/Resources
- [See main BLOG](https://akashrajpurohit.com/blog/adguard-home-network-wide-ad-blocking-in-your-homelab/)
- [Reddit Post](https://www.reddit.com/r/selfhosted/comments/1btx890/guide_adguard_home_network_wide_ad_blocking_in/?share_id=CG2oV77F6N3pJWt6OY8LB&utm_content=1&utm_medium=android_app&utm_name=androidcss&utm_source=share&utm_term=5)
