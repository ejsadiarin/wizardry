---
title: How to `sudo` without password (recovery guide)
date: 2024-03-28-1452 (March 28, 2024 2:52 PM)
tags: 
- Linux
- Recovery
---

# How to `sudo` without password (recovery guide)

## if VM:
- boot VM
- select recovery mode from grub
  - if cannot see grub on start up: 
    - see what timeouts are set in `/etc/default/grub` (decent is 10 seconds)
    - then update using:
    ```bash
    sudo update-grub
    ```
<br><br>
from there you have a root shell from which you can add user to sudoers file, and change password of user (`passwd`)

## if Physical Host Machine:
- get a USB live image running, from which you can use:
```bash
mount /dev/yourdiskpartition /mnt
chroot /mnt
passwd root # to change the password of root
```
- then reboot and login as root/sudo
- see next section for adding a user to a group

### Adding user to a group
- add to sudo group for root access
```bash
# to add to sudo group (one group):
usermod -aG sudo yourusername

# to multiple groups:
usermod -G -a yourusername sudo admn dip lpadmin lxd plugdev sambashare
```
- you can also edit `/etc/sudoers` and `/etc/group` to give regular user the necessary permissions
