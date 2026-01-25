---
date: 2026-01-26T00:04
title: Bluetooth
tags:
    - How-To
---

<!-- 2026-01-26-0004 (January 26, 2026 12:04:50 AM) -->

# Bluetooth

Bluetoothctl => connect and pair via bluetooth (tab completion is available for MAC_addresses)
ref: https://wiki.archlinux.org/title/bluetooth

check if bluetooth is on
`systemctl status bluetooth.service`
`systemctl start bluetooth.service` --> start bluetooth

- can use "enable" but not ideal since bluetooth will turn on when startup

`bluetoothctl` --> enter bluetooth shell
`devices` --> see devices (if no showing, do a "scan on" first)
`scan on` --> scan devices with bluetooth turned on
`agent on` --> default-agent should be appropriate on most cases
`pair <MAC_address>` --> pair device via bluetooth, use tab for completion

- `trust <MAC_address>` --> if using devices without a PIN, need to manually trust it before it can reconnect
  successfully
- `connect <MAC_address>` --> establish a connection

    ```bash
    OnePlus Buds 3 40:72:18:27:93:F2
    ```

    - Just `connect 40:72:18:27:93:F2`
