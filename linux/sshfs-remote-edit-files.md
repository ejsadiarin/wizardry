---
date: 2025-03-29T18:29
title: Remote Edit Files via `sshfs`
tags: 
    - Linux
    - How-To
---
<!-- 2025-03-29-1829 (March 29, 2025 06:29:38 PM) -->

# Remote Edit Files via `sshfs`

- install `sshfs` via package manager

```bash
# fedora
sudo dnf install sshfs

# ubuntu/debian
sudo apt install sshfs

# arch
sudo pacman -S sshfs
```

- mount `remote-directory` to `local-directory` via `sshfs`

```bash
sshfs {user}@{remote-host}:{remote-directory} {local-directory}

# example
sshfs ejs@server2:/home/ejs/homelab ~/homelab
```

## from `tldr`

```md
  `sshfs`

  Filesystem client based on SSH.
  More information: [https://github.com/libfuse/sshfs.](https://github.com/libfuse/sshfs.)

  - Mount remote directory:
    `sshfs username@remote_host:remote_directory mountpoint`

  - Unmount remote directory:
    `umount mountpoint`

  - Mount remote directory from server with specific port:
    `sshfs username@remote_host:remote_directory -p 2222`

  - Use compression:
    `sshfs username@remote_host:remote_directory -C`

  - Follow symbolic links:
    `sshfs -o follow_symlinks username@remote_host:remote_directory mountpoint`
```
