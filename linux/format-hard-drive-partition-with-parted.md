---
date: 2025-10-26T01:41
title: How to format hard drive partition with `parted`
tags: 
    - Linux
    - How-to
---
<!-- 2025-10-26-0141 (October 26, 2025 01:41:28 AM) -->

# How to format hard drive partition with `parted`

> [!NOTE]
> THIS WILL **ERASE** (wipe and format) the existing drive

**Make sure to install necessary dependencies first**:
  - `exfatprogs` (for mkfs.exfat) 
  - `parted` (for partitioning)

**Check drives and partitions first**:
```bash
lsblk
# or use fdisk for more details (see fs type, etc.)
fdisk -l
```

#### 1. Create a GPT partition table and a single partition

```bash
sudo parted /dev/sdX
(parted) mklabel gpt
```

* Now, just type `mkpart` and press Enter:

```bash
mkpart
```

* It will now ask you questions one by one. This avoids the error.

    * Partition name? []?
        * Just press Enter (you don't need a name).

    * File system type? `[ext2]`?
        * Just press Enter to accept the default. This does not matter, as we will format it in the next step.

    * Start?
        * Type 1MiB and press Enter.

    * End?
        * Type 100% and press Enter.

- Then `quit` parted

#### 2. Format the new partition as exFAT

```bash
# Find the partition name (usually sdX1)
lsblk /dev/sdX

# Format with name
sudo mkfs.exfat -n "ColdStorage" /dev/sdX1
```


#### 3. Create mountpoints

```bash
sudo mkdir -p /mnt/sdX1

sudo mount /dev/sdX1 /mnt/sdX1
```

### 4. (OPTIONAL for "Always-connected" Drives) Add to `/etc/fstab`

> [!NOTE]
> ONLY do this if the drive will be ALWAYS CONNECTED (not recommended for pluggable drives like USB, etc.)
> - external drives (with intention to be used as permanent, always-connected storage) are perfectly OK to be added to `/etc/fstab`

1. Get UUIDs of the drives:

* with `blkid`
```bash
sudo blkid
```

- output should look like:
```log
/dev/sda1: LABEL="ColdStorage" UUID="ABCD-1234" BLOCK_SIZE="512" TYPE="exfat"
/dev/sdb1: LABEL="LinuxData"   UUID="a1b2c3d4-..." BLOCK_SIZE="4096" TYPE="ext4"
```

* or with `lsblk`
```bash
lsblk -o NAME,UUID
```

2. Get your id and group
```bash
id -u
id -g
```
* most likely will output `1000` for both

3. Open & add lines to `/etc/fstab` with `vim` (replace UUIDs accordingly):

* For `exfat` drive (using the uid/gid options):
  * pass for `exfat` is always `0`

```fstab
# <device>                               <mount point> <type>  <options>                                <dump> <pass>
UUID=ABCD-1234                           /mnt/sda      exfat   defaults,nofail,uid=1000,gid=1000         0      0
```

* If another drive is `ext4` (no uid/gid needed):
  * pass for `ext4` is always `2`

```fstab
# <device>                               <mount point> <type>  <options>                                <dump> <pass>
UUID=a1b2c3d4-...                        /mnt/sdb      ext4    defaults,nofail                           0      2
```

4. Mount all that isn't mounted: `sudo mount -a`
* If no errors, then drives will now mount at boot

---

## Add labels (name) to existing drives

* Use "KDE Partition Manager"
* Select drive -> Properties -> Label
* OK -> then Apply
