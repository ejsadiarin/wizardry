---
tags:
  - How-To
date: 2024-02-25T16:05
title: Adjust Docker Default Address Pool
---

<!-- 2024-02-25-1605 (February 25, 2024 4:05 PM) -->

# How to Adjust Docker Default Address Pool

- This is for people who are either new to using docker or who haven't been bitten by this issue yet.

When you create a network in docker it's default size is /20.

- That's 4,094 usable addresses.
- Now obviously that is overkill for a home network.

By default it will use the 172.16.0.0/12 address range but when that runs out, it will eat into the 192.168.0.0/16 range which a lot of home networks use, including mine.

- My recommendation is to adjust the default pool size to something more sane like /24 (254 usable addresses).

You can do this by editing the `/etc/docker/daemon.json` file and restarting the docker service.

**The file will look something like this:**

```json
{
  "log-level": "warn",
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "5"
  },
  "default-address-pools": [
    {
      "base": "172.16.0.0/12",
      "size": 24
    }
  ]
}
```

You will need to "down" any compose files already active and bring them up again in order for the networks to be recreated.

**NOTE for those confused: Every time a network is created, a `/20` is taken out of the `/12`. The config above is adjusted to take a `/24` out of the `/12` instead.**

see [reference](https://www.reddit.com/r/selfhosted/comments/1az6mqa/psa_adjust_your_docker_defaultaddresspool_size/)

# Alternative - use IPv6

If only docker had some way for us to manage IPv6 better.

I just turn off the bridge networking and NAT that it enables, by adding this to `/etc/docker/daemon.json`:

```json
{
  "ipv6": true,
  "iptables": false,
  "ip6tables": false,
  "bridge": "none"
}
```

Then, I use everything in `network_mode: host`. Otherwise, I don't get good IPv6 usage going.
