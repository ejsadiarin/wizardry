---
id: creating-users-or-guests
aliases: []
tags:
  - System Administration
  - How-To
date: 2024-03-28-1509 (March 28, 2024 3:09 PM)
title: Creating Users or Guests to a Linux Machine
---

# Creating Users or Guests to a Linux Machine
### login as root
```bash
su
# or just prefix every command below with sudo
```

### add new user
```bash
useradd -m -s /bin/bash guest
```
- `-m` creates a home directory for the user
- `-s /bin/bash` sets the default shell for the user

### set password for user
```bash
passwd guest
```

### use ACLs (access control lists)
- if not yet installed, install `acl`
```bash
# for arch (btw)
sudo pacman -S acl

# for debian/ubuntu
sudo apt install acl
```

- then set the permissions for the directory
- for example, to restrict access to the `/home/guest` directory:
```bash
setfacl -m u:guest:--- /home/guest
# this removes all permissions for the guest user on the /home/guest directory.
```

#### setting up a chroot environment
- For a more secure environment, you can set up a chroot jail. This involves creating a separate directory tree for the guest user and configuring the system to restrict the user's access to this tree.

- Create a directory for the chroot environment:
```bash
sudo mkdir /home/guest/chroot
```

- Copy necessary files and directories into the chroot environment. This typically includes /bin, /lib, /usr, and others, depending on your requirements.

- Configure the user's shell to start in the chroot environment. Edit the user's shell configuration file (e.g., .bashrc or .bash_profile in the user's home directory) and add:
```bash
cd /home/guest/chroot
```

- Change the user's shell to a script that starts the `chroot` environment. Create a script (e.g., `/usr/local/bin/chroot-shell`) with the following content:
```bash
#!/bin/bash
chroot /home/guest/chroot /bin/bash
```

- Make the script executable:
```bash
sudo chmod +x /usr/local/bin/chroot-shell
```

- Change the user's default shell to the script:
```bash
sudo chsh -s /usr/local/bin/chroot-shell guest
```

### test the new user account
- Finally, test the new user account by switching to the guest user:
```bash
su - guest
```
You should now be logged in as the guest user with the permissions and restrictions you've configured.

Remember, the exact steps and commands might vary depending on your Linux distribution and specific requirements. Always refer to your distribution's documentation for the most accurate information.
