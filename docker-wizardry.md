---
tags:
  - Wizardry
date: 2024-04-20T3:53
---
<!-- 2024-04-20 (April 20, 2024 3:53 AM Saturday) -->

# Docker Wizardry


## Recommended Docker Configs

### adjust default address pool
- [see Adjusting Default Address Pool](/wizardry/devops/how-to-adjust-docker-default-address-pool.md)
TLDR: adjust the default pool size to something more sane like /24 (254 usable addresses). You can do this by editing the /etc/docker/daemon.json file and restarting the docker service.
*The file will look something like this:*
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
      "base" : "172.16.0.0/12",
      "size" : 24
    }
  ]
}
```
You will need to "down" any compose files already active and bring them up again in order for the networks to be recreated.

### make sure to scan open ports 
- [see this thread]()

## Docker Networking

You can tell the container to use a IP that matches your actual home network, read the basics of Docker networking: https://docs.docker.com/network/

And then specifically about MACVLAN and IPVLAN networks:

https://docs.docker.com/network/drivers/macvlan/

https://docs.docker.com/network/drivers/ipvlan/

This is probably worth a read too: https://docs.docker.com/network/network-tutorial-macvlan/

Which one to use depends on a lot of factors, its up to you.

Basically you create a Docker network of that type with IP/Subnet that matches your home network (something like 192.168.1.0/24 maybe) and then you start your Firefox container with telling it to join that network, with a specific IP address to use.

## Docker Security
https://docs.docker.com/security/security-announcements/ 

https://github.com/DoTheEvo/selfhosted-apps-docker 

### Rootless Docker

You don't need rootless Docker if you follow simple principles:
  1. Only run images with a default user 1000:1000 (no root!)
  2. Only use bridge, macvlan or ipvlan
  3. Do not use privileged
  4. Use AppArmor profiles if you need cap add capabillities
  5. No access to Docker socket (includes things like Portainer)

*or just use podman (rootless containers by default)*



## Docker Best Practices
- [Docker Defaults Best Practices](https://www.reddit.com/r/selfhosted/s/HWqUMxyZRf)
- [How much Docker Knowledge is Needed for DevOps](https://www.reddit.com/r/devops/comments/1cuvbkt/how_much_docker_knowledge_is_needed_for_a_job_in/)
