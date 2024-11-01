---
tags:
  - Wizardry
date: 2024-04-20T3:53
---

<!-- 2024-04-20 (April 20, 2024 3:53 AM Saturday) -->

# Docker Wizardry

- see "self-hostable" apps in [selfh.st](https://selfh.st/apps/) and [awesome-selfhosted](https://awesome-selfhosted.net/)
- see [Docker Deep Dive](https://medium.com/@furkan.turkal/how-does-docker-actually-work-the-hard-way-a-technical-deep-diving-c5b8ea2f0422)

## Recommended Docker Configs

### adjust default address pool

- [see Adjusting Default Address Pool](/wizardry/devops/how-to-adjust-docker-default-address-pool.md)
  TLDR: adjust the default pool size to something more sane like /24 (254 usable addresses). You can do this by editing the /etc/docker/daemon.json file and restarting the docker service.
  _The file will look something like this:_
- in `/etc/docker/daemon.json`, change `default-address-pools` --> `size` to 24

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

### make sure to scan open ports

- [Security PSA - on using Docker on publicly accessible host](https://www.reddit.com/r/selfhosted/comments/1cv2l3q/security_psa_for_anyone_using_docker_on_a/?share_id=opdJEA9xzpu0nmaaf1_3-&utm_content=1&utm_medium=android_app&utm_name=androidcss&utm_source=share&utm_term=1)
- [see docker-overrides-ufw-rules](./devops/docker-overrides-ufw-rules.md)

_Gist: Docker opens ports on your machine on its own and overrides firewall configurations (as long as the container is
running)_

TLDR: give Docker an explicit interface like `127.0.0.1:8080:80`, since just doing `ports: 80:80` will make it listen to
`0.0.0.0`, which means all interfaces including your public interface

#### PRIORITIZE

1. **Always verify your security setup.** It your case, a simple port scan would show this immediately. Having automated scans is even better.
2. This is a very old issue, but people keep doing this. Sadly, most tutorials focus on 'let get it running ASAP', and not on 'let's get it running securely'.
3. My solution is to expose only 22 (or whatever tech are you using to access your server), 80 and 443. All other stuff talks to reverse proxy via unix sockets (link).
4. `127.0.0.1:8080:80` is a must; regardless of whatever you use to talk to reverse proxy.
5. Don't use default Docker network. Each app stack should either get it's own network or no network at all. If networking is required, at least make it `internal` so the app won't have outbound internet access (most apps don't need it, frankly). Even if you end up with a compromised image, it won't be able to do much harm. The less attack surface is, the better.
6. This applies regardless of whether it's a VPS or a device in LAN: Zero Trust

#### More details

> Why is `127.0.0.1:8080:80` a must? is it to help prevent getting locked out?

- Because when you bind like `8080:80`; it implies `0.0.0.0:8080:80` - and your container will be available on all interfaces - exactly what you're making PSA about :)
- Never write just `8080:80`, always add `127.0.0.1` before (except for cases when you're exposing something really meant to be public, like `80` or `443)`

#### Allow containers LAN access but not general Internet access

Yes. Docker will modify your iptables rules, but you can modify them further to allow certain containers LAN access but not general internet access.

There should be a way to do this by using the virtual interfaces that Docker defines, instead of using hard coded IPs, but I spent quite a while figuring this out and it still works so I just went with it

First I create two docker networks, restricted and proxy, via

```bash
docker network create restricted --subnet 172.27.0.1/16 --ip-range 172.27.1.0/24

docker network create proxy      --subnet 172.28.0.1/16 --ip-range 172.28.1.0/24
```

Docker adds rules to iptables in the `DOCKER-USER` chain. You can simply add the following rule

```bash
-I DOCKER-USER -s 172.27.0.0/16 -m set ! --match-set docker dst -j REJECT --reject-with icmp-port-unreachable
```

I'm using ipset here to define the `docker` list of IP addresses. You could do something like

```bash
-I DOCKER-USER -s 172.27.0.0/16 ! -d 192.168.1.0/24 -j REJECT --reject-with icmp-port-unreachable
```

For each container you want to allow LAN but not WAN access, put it on the restricted network. Otherwise, put it on the proxy network.

### enable IPv6 Docker config

ref: https://docs.docker.com/config/daemon/ipv6/

## Docker Networking

1. `bridge`
2. `host`
3. `MACVLAN`
4. `NAT` ??

You can tell the container to use a IP that matches your actual home network, read the basics of Docker networking: https://docs.docker.com/network/

And then specifically about MACVLAN and IPVLAN networks:

[https://docs.docker.com/network/drivers/macvlan/](https://docs.docker.com/network/drivers/macvlan/)

[https://docs.docker.com/network/drivers/ipvlan/](https://docs.docker.com/network/drivers/ipvlan/)

This is probably worth a read too: https://docs.docker.com/network/network-tutorial-macvlan/

Which one to use depends on a lot of factors, its up to you.

Basically you create a Docker network of that type with IP/Subnet that matches your home network (something like 192.168.1.0/24 maybe) and then you start your Firefox container with telling it to join that network, with a specific IP address to use.

## Docker Security

Read THIS: [see Docker overriding UFW rules and iptables issue](../devops/docker-overrides-ufw-rules.md)

- see full original ufw-docker docs here: [Solving UFW and Docker Issues](https://github.com/chaifeng/ufw-docker?tab=readme-ov-file#solving-ufw-and-docker-issues)

- [https://docs.docker.com/security/security-announcements/](https://docs.docker.com/security/security-announcements/)

- [https://github.com/DoTheEvo/selfhosted-apps-docker](https://github.com/DoTheEvo/selfhosted-apps-docker)

- see above on [make-sure-to-scan-open-ports](#make-sure-to-scan-open-ports)

### Rootless Docker

You don't need rootless Docker if you follow simple principles:

1. Only run images with a default user 1000:1000 (no root!)
2. Only use bridge, macvlan or ipvlan
3. Do not use privileged
4. Use AppArmor profiles if you need cap add capabillities
5. No access to Docker socket (includes things like Portainer)

_or just use podman (rootless containers by default)_


## Docker Best Practices

> From someone who run containers commercially on the scale of a few thousand containers.

1. Use Volumes instead of Bind mounts
  - Bind mounts are static. Meaning you have to prepare the correct UID/GID and folder structure, where as volumes do this all for you

2. For Docker Networking: basically all containers should only communicate internally, so `internal:true`, 
  - **only the services that expose an actual service should talk to a reverse proxy on the node that is exposed via MACVLAN/IPVLAN/OVS to a VLAN on your network.**
  - Treat the host like a hypervisor and each container as a VM (avoid using the host).

  > The host network should only be used to manage the host, everything else goes in MACVLAN/IPVLAN/OVS/overlay. Just like you do with a hypervisor. 
  >
  > I would do PiHole like this: MACVLAN > Traefik UDP/TCP :53 > PiHole.
  >
  > I’m fully aware everyone else does: Host > Traefik UDP/TCP :53 > PiHole, 
  > - **but** people need to learn to not treat the host as an actual thing that accepts connections.

3. Use compose.yaml or CLI over GUI 
  - Portainer Problem: Using portainer means you give full root access to all your containers and your host to portainer itself, which is a huge security issue since any zero day in portainer compromises your entire node and all containers and all data. I recommend not to use portainer at all. Use compose.yaml

### Good Reads

- [Docker Defaults Best Practices](https://www.reddit.com/r/selfhosted/s/HWqUMxyZRf)
- [How much Docker Knowledge is Needed for DevOps](https://www.reddit.com/r/devops/comments/1cuvbkt/how_much_docker_knowledge_is_needed_for_a_job_in/)
