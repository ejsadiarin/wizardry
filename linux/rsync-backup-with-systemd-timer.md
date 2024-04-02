---
id: rsync-backup-with-systemd-timer
aliases: []
tags:
  - Backup
  - Systemd
  - Linux
  - How-To
date: 2024-02-24-20:37 (February 24, 2024 8:37 PM)
title: Rsync Backup with Systemd Timer
---

# Rsync Backup with Systemd Timer
> I have a systemd timer that runs every 3 days that backs up to a server with rsync, here's the setup:

- `/usr/local/bin/backup.sh`

```bash
#!/bin/bash

notify-send "Backup Starting" "Your computer is starting the backup process."

SOURCE="/path/to/local/system"
DESTINATION="user@server:/path/to/backup/directory"

# Create a timestamped backup directory
BACKUP_DIR=$(date +%Y-%m-%d_%H-%M-%S)
ssh user@server "mkdir -p ${DESTINATION}/${BACKUP_DIR}"

# Perform the rsync operation
rsync -azP --delete ${SOURCE} ${DESTINATION}/${BACKUP_DIR}

# Cleanup old backups - keep only the last 5 backups
ssh user@server "ls -dt ${DESTINATION}/* | tail -n +6 | xargs --no-run-if-empty rm -rf"

# Optionally, send a notification when the backup is complete
notify-send "Backup Complete" "Your computer has finished the backup process."
```

- `/etc/systemd/system/backup.service`
```toml
[Unit]
Description=Backup Service

[Service]
Type=oneshot
ExecStart=/usr/local/bin/backup.sh
```

`/etc/systemd/system/backup.timer`
```toml
[Unit]
Description=Runs backup every 3 days

[Timer]
OnCalendar=*-*-* 19:00:00
Persistent=true
Unit=backup.service

[Install]
WantedBy=timers.target
```

- `/usr/local/bin/backup-notify.sh`
```bash
#!/bin/bash

TODAY=$(date +%Y-%m-%d)

# Next scheduled backup date from the timer
NEXT_BACKUP=$(systemctl list-timers --all | grep backup.timer | awk '{print $2}')

if [ "$TODAY" == "$NEXT_BACKUP" ]; then
    notify-send "Backup Scheduled" "Your computer will backup at midnight today."
fi
```

`/etc/systemd/system/backup-notify.service`
```toml
[Unit]
Description=Backup Notification Service
After=network.target

[Service]
Type=oneshot
ExecStart=/usr/local/bin/backup-notify.sh

[Install]
WantedBy=default.target
```

> Comment (about only sending rsync diffs):
  > Rsync is one of my favorite tools. In your case, while it works, you give up one of the best features; only sending diffs. If your backups are small or have a lot of changes it probably won't help much but otherwise, it might be worth looking at keeping one directory in sync remotely and rotating the tree backwards in time on the remote side. That way you're not transferring the full tree over the network - every time.
  > incremental backup (for entire system): `rsync -aAXv --progress --delete --exclude=".*" --exclude="lost+found" /src /dst`

# References
- [reddit](https://www.reddit.com/r/archlinux/comments/1ayd68v/comment/kru7cm6/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button)
