---
id: ssh-nc
aliases: []
tags:
  - Wizardry
  - How-To
date: 2024-02T22:18
title: SSH + nc
---
<!-- 2024-02-22-2218 (February 22, 2024 10:18 PM) -->

# SSH and nc Combo Magic Spell
`ssh` in connection with `nc`

> 1. Seriously... `ssh` + `nc` combo is insanely powerful if you know how to use it. 
    Port-forwarding anything anywhere, forward tunnels, reverse tunnels, firewall hole-punching, forwarding traffic forward and and backward through a corporate HTTP proxy, being able to act as HTTP proxy if need be ... 
    The things `ssh` can do when coupled with `nc` are insane. A CISO's nightmare... if only they knew the true power of ssh ...

> 2. I’ve just recently setup a reverse ssh tunnel + socat forward to allow people inside one firewall access computers inside another firewall, transparently. I had to use an external relay to get it to work because of corporate it policies, but it works really well. Transparent rdp to a windows machine inside a nat’ed network from another heavily firewalled network, all over a secure reverse ssh tunnel.
    - I've done a separate post on the method I used for this: https://www.reddit.com/r/linux/comments/1ak27fb/how_to_forward_any_service_over_a_reverse_ssh/

> 3. Yes, port forwarding with ssh -ND is quite powerful. You are just one ssh -ND 2022 host from a SOCKS proxy at localhost:2022!
    You can use ssh -D to route traffic via a socks4/5 proxy. For example configure your web browser to use the port in proxy settings, and all your traffic goes through ssh to the remote host. D for 'dynamic'
    My understanding of ssh -L is that it forwards tcp ports and unix sockets, which to my layman's understanding is similar, but a bit more limited. I use -L to bind remote guis to my localhost - mainly syncthing's gui.

> 4. -D sets up a socks proxy where an incoming connection (listen on the remote side, connections are made from the remote side( includes instruction on where to go. This requires that the client be socks aware (or it can be made so with proxychains) but it can go anywhere.

# SSH magics 
- port forward from local to remote
`ssh -L 8080:localhost:80 user@remote`
- port forward from remote to local
`ssh -R 8080:localhost:80 user@remote`

# Can use nginx as a reverse proxy
- more configurable

### Check listening ports first via `netstat`
`netstat -tupln` - checks the listening ports (can prefix with `sudo`)

# References
- [Gold mine commands](https://www.reddit.com/r/linux/comments/1ajslo3/what_are_your_most_valuable_and_loved_command/?share_id=jCXi6jsOro0-56gY9wXD9)
- [yt vid BugsWriter](https://www.youtube.com/watch?v=lKIRHDY7OhU)
