---
date: 2025-03-27T01:21
title: How to use Bridge Networking in `virt-manager` (Qemu-KVM)
tags: 
    - Linux
    - Virtualization
    - How-To
---
<!-- 2025-03-27-0121 (March 27, 2025 01:21:29 AM) -->

# How to use wireless Bridge Networking in `virt-manager` (QEMU-KVM)

> [!IMPORTANT]
> **THIS IS TECHNICALLY A WORKAROUND using usermode for networking (so not exactly bridged)**
> if you have ethernet, follow the official ethernet guide. 
> or just use a virtualization manager/client with wireless bridge support like VirtualBox

- ref: [https://forum.endeavouros.com/t/qemu-virt-manager-and-bridge-network/45005/10](https://forum.endeavouros.com/t/qemu-virt-manager-and-bridge-network/45005/10)

## Edit 'default' libvirt network from terminal 
```bash
# user (~/.config/libvirt/qemu/networks/default.xml)
virsh net-edit default

# system (/etc/libvirt/qemu/networks/default.xml)
sudo virsh net-edit default
```

---

## Replace with this working config:

- from `sudo virsh net-dumpxml default`:

> [!NOTE]
> 1. replace `uuid` with your default `uuid` (the already existing one, do not use the `uuid` below)
> 2. you can remove the `host` tags

```xml
<network>
  <name>default</name>
  <uuid>263c026a-b2a3-47e6-af9f-ac8f6848d4e2</uuid>
  <forward mode='nat'>
    <nat>
      <port start='1024' end='65535'/>
    </nat>
  </forward>
  <bridge name='virbr0' stp='on' delay='0'/>
  <mac address='52:54:00:92:8e:30'/>
  <ip address='192.168.122.1' netmask='255.255.255.0'>
    <dhcp>
      <range start='192.168.122.2' end='192.168.122.254'>
        <lease expiry='24' unit='hours'/>
      </range>
      <host mac='52:54:00:5d:52:ea' name='windows-11' ip='192.168.122.101'/>
      <host mac='52:54:00:91:37:db' name='debian-cin' ip='192.168.122.110'/>
      <host mac='52:54:00:e7:a2:3f' name='debian-kde' ip='192.168.122.111'/>
      <host mac='52:54:00:40:0e:ab' name='debian-gnome' ip='192.168.122.112'/>
      <host mac='52:54:00:a9:33:cf' name='arch-iso' ip='192.168.122.120'/>
      <host mac='52:54:00:40:20:7e' name='arch-cin' ip='192.168.122.121'/>
      <host mac='52:54:00:8c:07:95' name='arch-kde' ip='192.168.122.122'/>
      <host mac='52:54:00:9f:62:85' name='arch-gnome' ip='192.168.122.123'/>
      <host mac='52:54:00:52:80:33' name='ubuntu-cin' ip='192.168.122.130'/>
      <host mac='52:54:00:92:ce:ef' name='ubuntu-kde' ip='192.168.122.131'/>
      <host mac='52:54:00:0f:c6:79' name='ubuntu-gnome' ip='192.168.122.132'/>
      <host mac='52:54:00:5b:75:8e' name='fedora-cin' ip='192.168.122.140'/>
      <host mac='52:54:00:06:d5:b4' name='fedora-kde' ip='192.168.122.141'/>
      <host mac='52:54:00:33:bf:85' name='fedora-gnome' ip='192.168.122.142'/>
      <host mac='52:54:00:8b:2e:b8' name='opensuse-cin' ip='192.168.122.150'/>
      <host mac='52:54:00:3e:82:db' name='opensuse-kde' ip='192.168.122.151'/>
      <host mac='52:54:00:3e:e8:c6' name='opensuse-gnome' ip='192.168.122.152'/>
      <host mac='52:54:00:2b:1b:18' name='gentoo-systemd' ip='192.168.122.160'/>
      <host mac='52:54:00:3e:b7:82' name='gentoo-term' ip='192.168.122.161'/>
      <host mac='52:54:00:69:d7:27' name='gentoo-cin' ip='192.168.122.162'/>
      <host mac='52:54:00:ad:1d:b8' name='gentoo-kde' ip='192.168.122.163'/>
      <host mac='52:54:00:57:e2:49' name='gentoo-xfce' ip='192.168.122.164'/>
      <host mac='52:54:00:25:e5:a5' name='nixos-kde' ip='192.168.122.171'/>
    </dhcp>
  </ip>
</network>
```

---

## Enable `QEMU/KVM user session` mode in `virt-manager`

- Open `virt-manager` --> File
- Add Connection --> select `QEMU/KVM user session` --> Add connection
- Create new VM/guest by right clicking the connection
- Provision desired resources (RAM, CPU cores, etc.)
- Then just "Next" or "Done" everything
