---
title: Rsync
date: 2024-02-10-22:54 (February 10, 2024 10:54 PM)
tags:
- Linux
- CLI
- File Transfer
- Synchronization
---

# Rsync
- `rsync` is a file transfer and synchronization tool for Linux
  - it skips files that are already in "sync"
  - by default, network transfer is not encrypted (use `ssh` for that)
    - works with `ssh` for network transfers
  - perfect for physical file syncing/transfer
    - it does not need to be encrypted, since it does not use the network
    - e.g. USB, external HDD

## Usage
- simple usage using archive mode:
```bash
# most basic usage (sufficient simple transfers)
rsync -avt <source> <destination>
rsync -avt <source>/ <destination> # the / means to just copy the contents of the directory

# with ssh (for network transfers)
rsync -avtz -e ssh <source> user@remote_host:/path/to/destination
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
