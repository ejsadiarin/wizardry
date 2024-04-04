---
id: fscrypt
aliases: []
tags:
  - Encryption
  - Linux
date: 2024-02-10T14:43
title: fscrypt for encrypting directories
---
<!-- 2024-02-10-1443 (February 10, 2024 2:43 PM) -->

# fscrypt for encrypting directories
- Install `fscrypt` package
  ```bash
  sudo apt install fscrypt
  # or (depending on your package manager)
  sudo pacman -S fscrypt
  ```
- Make sure `CONFIG_FS_ENCRYPTION=y` is set (enable encryption on your fs):
  - do an `lsblk` to find your home partition
  - for ext4 filesystems (see references for more fs support):
  ```bash 
  tune2fs -O encrypt /dev/device
  ```
- Setup:
```bash
fscrypt setup
```
- to unlock passphrase-protected directories automatically on login (See [PAM module](https://wiki.archlinux.org/title/Fscrypt#PAM_module)):
- Encrypt a directory:
  ```bash
  fscrypt encrypt <directory>
  # like so:
  fscrypt encrypt private/
  ```
  - i recommend using 2:
  > Should we create a new protector? [y/N] y
    Your data can be protected with one of the following sources:
    1 - Your login passphrase (pam_passphrase)
    2 - A custom passphrase (custom_passphrase)
    3 - A raw 256-bit key (raw_key)
    Enter the source number for the new protector [2 - custom_passphrase]: 2
    Enter a name for the new protector: Super Secret
    Enter custom passphrase for protector "Super Secret":
    Confirm passphrase:
    "private/" is now encrypted, unlocked, and ready for use.

- Lock and Unlock:
```bash
fscrypt unlock <directory> # then fscrypt will prompt for password
fscrypt lock <directory>
```

- To encrypt full home directory see [here](https://wiki.archlinux.org/title/Fscrypt#Encrypt_a_home_directory)

# References
- [https://wiki.archlinux.org/title/Fscrypt](https://wiki.archlinux.org/title/Fscrypt)
