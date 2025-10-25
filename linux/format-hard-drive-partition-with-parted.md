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
sudo parted /dev/sdX --script mklabel gpt
sudo parted /dev/sdX --script mkpart primary 0% 100%
```

#### 2. Format the new partition as exFAT

```bash
# Find the partition name (usually sdX1)
lsblk /dev/sdX

# Format
sudo mkfs.exfat /dev/sdX1
```


#### 3. Create mountpoints

```bash
sudo mkdir -p /mnt/sdX1

sudo mount /dev/sdX1 /mnt/sdX1
```

### 4. (OPTIONAL for automatic mounts) Add to `/etc/fstab` for Persistence

Get UUIDs:

```bash
lsblk -o NAME,UUID
```

Add lines to `/etc/fstab` (replace UUIDs accordingly):

```
# /etc/fstab
UUID=xxxx-xxxx  /mnt/sda1  exfat  defaults  0  0
UUID=yyyy-yyyy  /mnt/sdb1  exfat  defaults  0  0
UUID=zzzz-zzzz  /mnt/sdc1  exfat  defaults  0  0
UUID=wwww-wwww  /mnt/sdd1  exfat  defaults  0  0
```

**With this, everytime the drive is mounted, it will automatically mount to the specific defined paths properly**
