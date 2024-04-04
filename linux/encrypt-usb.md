---
id: encrypt-usb
aliases: []
tags:
  - Linux
  - Security
  - Encryption
  - USB
  - How-To
date: 2024-02-17T22:05
title: Encrypt USB (with LUKS and cryptsetup)
---
<!-- 2024-02-17-2205 (February 17, 2024 10:05 PM) -->

# Daily Usage (via CLI)
- Opening the Encrypted USB:
```bash
# open the encrypted partition
cryptsetup open /dev/<usb-PART> <name> # e.g. cryptsetup open /dev/sdb1 ejsmain
# mount
mount /dev/mapper/<name> /mnt # e.g. mount /dev/mapper/ejsmain /mnt
# check
lsblk
```

- Closing the Encrypted USB:
```bash
# unmount
umount /mnt
# encrypt again
cryptsetup close <name> # e.g. cryptsetup close ejsmain
# check
lsblk
```

# Creating an Encrypted USB (with LUKS and cryptsetup)
1. Create a partition on the USB drive. (if not yet setup)
```bash
sudo su
lsblk
fdisk /dev/<usb-DISK> # e.g. fdisk /dev/sdb
# fdisk shell:
d
n
# select all default options, just enter till created new partition
w # write and save
lsblk
```
2. Encrypt the partition with LUKS.
```bash
cryptsetup luksFormat /dev/<usb-PART> # e.g. cryptsetup luksFormat /dev/sdb1
# cryptsetup shell:
YES
# enter master passphrase
```
3. Open and create a filesystem on the encrypted partition.
```bash
# open
cryptsetup open /dev/<usb-PART> <name> # e.g. cryptsetup open /dev/sdb1 ejsmain
# create filesystem (using btrfs here)
mkfs.btrfs /dev/mapper/<name> # e.g. mkfs.btrfs /dev/mapper/ejsmain
```
4. Mount the encrypted partition.
```bash
mount /dev/mapper/<name> /mnt # e.g. mount /dev/mapper/ejsmain /mnt/
# check
lsblk
# you can now treat it as a normal mounted USB drive
```
5. When done, unmount and close the encrypted partition.
```bash
umount /mnt
cryptsetup close <name> # e.g. cryptsetup close ejsmain
```

# References
- [from Luke Smith](https://www.youtube.com/watch?v=ZNaT03-xamE)
