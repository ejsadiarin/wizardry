---
tags:
  - Linux
date: 2024-04-16T23:30
---
<!-- 2024-04-16-2330 (April 16, 2024 11:30:37 PM) -->

# Encrypting Hard Drive Partition with LUKS
- for usb drives see [encrypt-usb](wizardry/linux/encrypt-usb.md)

### 1. see partitions using fdisk
```bash
sudo fdisk -l
# or
lsblk
```

### 2. select partition to wipe
```bash
sudo fdisk /dev/sdX # sdX here is any
# fdisk shell:
d
n
# select all default options, just enter till created new partition
Y
w # write and save
lsblk
```

### 3. Encrypt with LUKS
```bash
sudo cryptsetup luksFormat /dev/<usb-PART> # e.g. cryptsetup luksFormat /dev/sdb1
# cryptsetup shell:
YES
# enter master passphrase

```

### 4. Give a name to the partition and format filesystem
```bash
# open
sudo cryptsetup luksOpen /dev/<sdX> <name> # e.g. cryptsetup open /dev/sdb1 sda1
# see mapper
sudo fdisk -l
# create filesystem (using btrfs here)
sudo mkfs.btrfs /dev/mapper/<name> # e.g. mkfs.btrfs /dev/mapper/sda1
```
- you can also format the filesystem to standard ext4
```bash
sudo mkfs.ext4 /dev/mapper/<name>
# since ext4 reserve some space by default, remove this space if you don't run your system on this partition
sudo tune2fs -m 0 /dev/mapper/<name>
```

- after this, the partition is now ready!

### 5. Mount the partition
```bash
sudo mount /dev/mapper/<name> /mnt/<name> # e.g. mount /dev/mapper/sda1 /mnt/hdd
# check
lsblk
# you can now treat it as a normal mounted USB drive
```

### 6. use `chown` to change owner so that we do not need to sudo everytime
```bash
sudo chown -R `whoami` :users /mnt/<name>
# or just
sudo chown -R `whoami` /mnt/<name>
```

### 7. Unmount and Close after using the disk
```bash
sudo umount /dev/mapper/<sdX>
sudo cryptsetup luksClose <name> # e.g. cryptsetup close sda1
```

# References
- [Encrypt your Hard Drive in Linux](https://www.youtube.com/watch?v=ch-wzDyo-wU&t=152s&ab_channel=AverageLinuxUser)
