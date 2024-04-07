---
tags:
  - Docker
  - Security
  - Debug
  - Containers
  - How-To
date: 2024-02-22T21:30
title: Docker overrides ufw and iptables rules by injecting it's own rules
---
<!-- 2024-02-22-2130 (February 22, 2024 9:30 PM) -->

# Docker overriding UFW rules and iptables issue

Yes. This is a very well-known issue: [https://www.google.com/search?q=docker+ufw&hl=en&gl=en](https://www.google.com/search?q=docker+ufw&hl=en&gl=en) . Sadly, most 'get things up and running' guides completely omit it.

This is how I deal with it:

I'm running nginx baremetal - on the host machine (because I like it this way. No one stops you from running nginx in container as well, it's even better because it simplifies setup/migration). All of my apps are in Docker containers.

For every app that supports sockets, I'm using unix sockets:

`proxy_pass http://unix:/home/nextcloud/.socket/php-fpm.sock;`

Where sockets are not supported, I use http ports:

`proxy_pass http://127.0.0.1:8000;`

First, I create a **separate** network for each app, so they cannot talk to each other. No app is using Docker default network. Some apps also are restricted from reaching the internet (to do so, add internal: true under net)

**Important!** Second, make sure that your ports are attached to `127.0.0.1`, and not to `0.0.0.0` as it is by default - because on many OS [Docker overrides UFW rules](https://stackoverflow.com/questions/30383845/what-is-the-best-practice-of-docker-ufw-under-ubuntu) and allows the containers to be reachable from the internet. Especially disastrous if it's a VPS (and not a homelab server behind NAT and a firewall/tailscale); and the authentication is done by nginx and not the container itself.

```yaml
version: '3.9'

networks:
  net:
    driver: bridge
    driver_opts:
      com.docker.network.bridge.name: '${APP_NAME}-br'

services:
  webdav:
    # ...
    ports:
      - 127.0.0.1:8000:80
    networks:
      - net
```

Third, wherever possible, the containers withing the docker-compose service communicate with each other via sockets in named volumes, no need to expose these on the host itself:

```yaml
services:
  apache:
    # ...
    depends_on:
      - db
    volumes:
      - dbsocket:/var/run/mysqld/

  db:
    # ...
    volumes:
      - dbsocket:/var/run/mysqld-socketdir/
      - ./conf/mariadb.conf:/etc/mysql/conf.d/70-mariadb.cnf
      - ${DB_SQLINITDIR}:/docker-entrypoint-initdb.d/
      - ${DB_DATADIR}:/var/lib/mysql/

volumes:
  dbsocket:
```

# Alternative Solution
- use [ufw-docker](https://github.com/chaifeng/ufw-docker) 

# References:
- [this reddit post](https://www.reddit.com/r/selfhosted/comments/1atjsra/til_docker_overrides_ufw_and_iptables_rules_by/)
- [this answer](https://www.reddit.com/r/selfhosted/comments/1atjsra/comment/kqz8i90/?utm_source=share&utm_medium=web2x&context=3)
