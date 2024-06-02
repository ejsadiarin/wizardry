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

- find own timezone
```bash
timedatectl list-timezones | grep "place"
```

- then set timezone to UTC (always recommended)
```bash
timedatectl set-timezone UTC

```

- (optional) turn on NTP service:
```bash
sudo systemctl enable systemd-timesyncd.service # or sudo systemctl start systemd-timesyncd.service for non-persistent change
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
