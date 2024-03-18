---
title: Perfect Backup Strategy
date: 2024-03-12-1039 (March 12, 2024 10:39 AM)
tags: 
- Backups
---

# Perfect Backup Strategy
opts:
- `rsync` and `rclone`
- github for config files and notes
- `timeshift` for system snapshots
- nightly `borgmatic` backup to synology NAS
- weekly backup: `restic` or `rclone` to backlaze b2 bucket
- USB drive for personal
- `btrfs` with snapshots + `btrbk` to sync snapshots to NAS or USB drive

## Need to backup
1. config files (dotfiles)
   - distro-specfic (`bspwm`)
   - `tmux`, `nvim`, `zsh`
2. personal files
   - including `gpg` keys, `ssh` keys, etc.
   - journal
3. system configs
   - firewall rules, etc.
4. notes and documentation
  - wizardry
  - obsidian vault
