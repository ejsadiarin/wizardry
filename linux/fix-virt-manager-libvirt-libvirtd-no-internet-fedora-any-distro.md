---
date: 2025-05-16T03:11
title: `virt-manager` fix no internet issue via /etc/libvirt/network.conf
tags: 
    - Linux
    - How-To
---
<!-- 2025-05-16-0311 (May 16, 2025 03:11:51 AM) -->

# `virt-manager` fix no internet issue via /etc/libvirt/network.conf

_as of [2025-05-16 03:30] - [May 16, 2025]:_

1. in `/etc/libvirt/network.conf`, edit the `firewall_backend` to `iptables` (`firewall_backend = "iptables"`)

> [!NOTE]
> yes even if you use nftables for your firewall (like in fedora). Just edit `firewall_backend` to `iptables`.

```conf
# should be
firewall_backend = "iptables"
```

2. restart `libvirtd` service via systemctl

```bash
sudo systemctl restart libvirtd
```

3. open virt-manager --> choose your VM --> "options" (the "i" icon) -->  --> make sure to use "Virtual Network 'default' NAT"
- no need to use "virtio"

> [!IMPORTANT]
> MAKE SURE to have an IP Address (automatically provisioned)
> - if not then most likely you'll have no internet available **BUT start up VM either way -> might provision only after first VM startup**.

4. Profit. Ez.

# References

- [https://discussion.fedoraproject.org/t/virtual-machine-manager-networking-broke-after-update/137308/6](https://discussion.fedoraproject.org/t/virtual-machine-manager-networking-broke-after-update/137308/6)
- [https://bbs.archlinux.org/viewtopic.php?id=291898](https://bbs.archlinux.org/viewtopic.php?id=291898)
