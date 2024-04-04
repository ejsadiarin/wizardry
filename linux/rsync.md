---
id: rsync
aliases: []
tags:
  - Linux
  - CLI
  - File Transfer
  - Synchronization
date: 2024-02-10T22:54
title: Rsync
---
<!-- 2024-02-10-2254 (February 10, 2024 10:54 PM) -->

# Rsync
- `rsync` is a file transfer and synchronization tool for Linux
  - it skips files that are already in "sync"
  - by default, network transfer is not encrypted (use `ssh` for that)
    - works with `ssh` for network transfers
  - perfect for physical file syncing/transfer (backups)
    - it does not need to be encrypted, since it does not use the network
    - e.g. USB, external HDD
  - **NOTE:** `rsync` will not preserve user and group ownership (UNIX-style ownerships & permissions) IF the filesystem format of the USB flash drive is not POSIX-compliant (e.g. vfat or ntfs) [ref](https://www.reddit.com/r/linuxquestions/comments/slnv4q/comment/hvscc6u/?utm_source=share&utm_medium=web2x&context=3)
    - it will only preserve timestamps
    > The error says, that the target filesystem on your USB drive doesn't support UNIX-style ownerships & permissions. So I guess your external drive is vfat or ntfs formatted. Even when you use a GUI filemanager or rsync, the modes & ownership will not be preserved, only the timestamps will. If you really need the whole permissions-stuff on the USB drive, you have to format it with a ext2 or ext4 filesystem, then it will work.
    - check the filesystem format using `lsblk -f` or `df -h -T`

  - **SOLUTIONS: - proper mount so rsync preserves ownership & permissions**
    1. use GUI file manager that mounts it properly (e.g. Thunar, Nautilus, etc.) then `rsync`
    2. mount it using `udisks2` (cli) then `rsync`
    3. format USB flash drive to ext2 or ext4 filesystem

## Usage
- simple usage using archive mode:
```bash
# most basic usage (sufficient simple transfers)
rsync -avh <source> <destination>
rsync -avh <source>/ <destination> # the / means to just copy the contents of the directory

# with ssh (for network transfers)
rsync -avhz -e ssh <source> user@remote_host:/path/to/destination

# for updating (sending only diffs)
rsync -auzh <source> <destination>
```

- [from `tldr`] Use archive mode, resolve symlinks and skip files that are newer on the destination:
```bash
rsync --archive --update --copy-links path/to/source path/to/destination
```

- [from `tldr`] Transfer a directory to a remote host running `rsyncd` and delete files on the destination that do not exist on the source:
```bash
rsync --recursive --delete rsync://host:path/to/source path/to/destination
```

- [from `tldr`] Transfer a file over SSH using a different port than the default (22) and show global progress:
```bash
rsync --rsh 'ssh -p port' --info=progress2 host:path/to/source path/to/destination
```


### most common options
- `-a` - archive mode (preserves permissions, ownership, is recursive, etc.)
  - or `-rlptgoD` (long version of `-a`)
- `-r` or `--recursive` - recursive
- `-u` or `--update` - update (send only diffs)
- `-v` or `--verbose` - verbose output
- `-t` or `--times` - preserve modification times
- `-h` or `--human-readable` - output in human-readable format
- `-z` or `--compress` - compress file data during the transfer
- `-P` or `--partial --progress` - show progress and keep partially transferred files
- `--delete` - delete files in the destination that are not in the source
- `--exclude` - exclude files from the transfer
  - e.g. `--exclude='*.log'`
- `--dry-run` - perform a trial run with no changes made
- `--stats` - give some file-transfer stats

# References
- more details on man pages: `man rsync`
- use `tldr` to see practical uses: `tldr rsync`
- [rsync needs proper mounting](https://www.reddit.com/r/linuxquestions/comments/slnv4q/comment/hvscc6u/?utm_source=share&utm_medium=web2x&context=3)
- [rsync sync diff only](https://www.tecmint.com/sync-new-changed-modified-files-rsync-linux/)
