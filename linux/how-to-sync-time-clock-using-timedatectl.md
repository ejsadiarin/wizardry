---
tags:
  - How-To
date: 2024-06-02T03:00
---

<!-- 2024-06-02-0300 (June 02, 2024 03:00:26 AM) -->

# How to Sync Time/Clock using timedatectl

## BEST WAY

- check status

```bash
timedatectl
```

- stop systemd-timesyncd.service if necessary:

```bash
sudo systemctl stop systemd-timesyncd.service
```

- find own timezone

```bash
timedatectl list-timezones
# or
timedatectl list-timezones | grep "place"
```

- then set timezone to UTC (always recommended)

```bash
timedatectl set-timezone UTC

```

- turn on NTP service:

```bash
timedatectl set-ntp true
```

- make sure that the ntp server/pool in `/etc/systemd/timesyncd.conf` works

  - see [NTP Pool Project](https://www.ntppool.org/en/) for list of NTP pool servers worldwide
  - read documentation on NTP (Network Time Protocol) on specific distro
  - **NOTE: if the NTP pool doesn't work, system clock will NOT be synced!**

- _example configs below:_

```conf
# in /etc/systemd/timesyncd.conf:

NTP=0.asia.pool.ntp.org time.cloudflare.com # and more...
FallbackNTP=time.cloudflare.com # fallback ntp server
```

```conf
# in /etc/ntp.conf

# add NTP servers like this
server 0.asia.pool.ntp.org
server 1.asia.pool.ntp.org
server 2.asia.pool.ntp.org
server 3.asia.pool.ntp.org
```

- enable and restart systemd-timesyncd.service

```bash
sudo systemctl enable systemd-timesyncd.service # or sudo systemctl start systemd-timesyncd.service for non-persistent change
sudo systemctl restart systemd-timesyncd.service
```

## If the UTC is correct but the hardware clock is not

ref: https://bbs.archlinux.org/viewtopic.php?id=154992

```bash
sudo hwclock --systohc --verbose
```

## Manually with `set-time`

- use `set-time` flag/option like this (HH:mm:ss):

```bash
timedatectl set-time 17:07:07
```

- if failed with:

```bash
Failed to set time: NTP unit is active
```

- then disable NTP service first

```bash
sudo systemctl disable --now chronyd
```

#
