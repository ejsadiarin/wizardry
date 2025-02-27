---
tags:
  - How-To
date: 2025-02-25T16:57
---

<!-- 2025-02-25-1657 (February 25, 2025 04:57:57 PM) -->

# How to Sync Timezone/Time/Clock with Chrony NTP

- this guide uses `time.cloudflare.com` NTP server


1. `sudo systemctl stop chronyd`
1. edit `/etc/chrony.conf`

3. edit the line with `pool ...` to `pool time.cloudflare.com`

4. `sudo systemctl start chronyd`
5. `sudo systemctl restart chronyd` (idk why not)

