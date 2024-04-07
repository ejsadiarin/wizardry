---
tags:
  - Linux
  - USB
date: 2024-02-02T22:01
title: USB on Linux
---
<!-- 2024-02-02-2201 (February 02, 2024 10:01 PM) -->

# 3 ways:
- the classic way
- the modern way
- the easy way (using a file manager)

# The Classic Way

## Mounting USB drives on Linux
- `lsblk` to see block devices
- Plug USB
- run `lsblk` to see USB (see new entry like `sdb` or `sdb1`)
  - `sdb` is the USB device
  - `sdb1` is the partition
  - NOTE: it can be different (`sdxN`, where x is letter, N is number)
- Mount USB drive:
  ```bash
  # TLDR: create a mount point and mount the drive
  sudo mkdir /mnt/usbdrive            # create a mount point (if needed)
  sudo mount /dev/sdxN /mnt/usbdrive  # mount the drive's data partition
  ```
- the `/mnt/usbdrive` is just the access point (it doesn't automatically copy the data in USB)

## Unmounting and safely eject USB
- Unmount:
  ```bash
  # Unmount the drive's partitions---choose one option.

  # Option 1 (yup, "umount" and not "unmount")
  umount /mnt/usbdrive  # by specifying mount point (preferred)

  # Option 2
  umount /dev/sdxN      # by specifying the device partition (not preferred)
  ```

- Safely Eject (remove):
  ```bash
  # Option 1: works from a normal user shell using sudo
  echo 1 | sudo tee /sys/block/sdx/device/delete

  # Option 2: works only from a root shell
  echo 1 > /sys/block/sdx/device/delete
  ```

# The Modern Way
- using `udisksctl` (or `udisks2`)
- install `udisks2` if not installed
  ```bash
  sudo pacman -S udisks2
  ```
- run `udisksctl` to see USB drives
- mount the USB drive:
  ```bash
  udisksctl mount -b /dev/sdxN
  ```
- eject the USB drive:
  ```bash
  # Unmount the drive's partition
  udisksctl unmount -b /dev/sdxN

  # Power off a drive
  udisksctl power-off -b /dev/sdx
  ```

# Easy way
- just install a file manager like `thunar`
- then drag and drop files to and from USB drives

### References
- [https://www.ejmastnak.com/tutorials/arch/usb](https://www.ejmastnak.com/tutorials/arch/usb)
