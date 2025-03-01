---
tags:
  - Wizardry
date: 2024-06-10T16:36
---
<!-- 2024-06-10 (June 10, 2024 4:36 PM Monday) -->

#  `restic` Wizardry


# Backup

- for full backblaze b2 restic guide see [my docs](./restic-backblazeb2-backup.md) or [official backblazeb2 docs](https://www.backblaze.com/docs/cloud-storage-integrate-restic-with-backblaze-b2#create-a-bucket)
1. create `restic-env` with necessary values in some path (e.g. `/etc/restic-env`)
```env
export AWS_ACCESS_KEY_ID=<B2_KEY_ID>
export AWS_SECRET_ACCESS_KEY=<B2_ApplicationKey>
export RESTIC_REPOSITORY="s3:s3.us-east-005.backblazeb2.com/<unique-bucket-name>"
export RESTIC_PASSWORD_FILE=<path-to/restic-password>
```

2. create `restic-password` with one-liner password in the path specified above
- paths can be `/etc/restic-password` or `$HOME/restic/restic-password`


3. change ownership and permissions (optional) to root or any user desired
```bash
chown root:root /etc/restic-env
chown root:root /etc/restic-password
chmod 700 /etc/restic-env
chmod 700 /etc/restic-password
```

4. source `restic-env` by adding this to `~/.bashrc` (or any shell rc) file 
```bash
source /etc/restic-env
```
- then `source ~/.bashrc` to refresh bash configs

5. initialize restic repository
```bash
source /etc/restic-env
restic -r s3:s3.us-east-005.backblazeb2.com/<unique-bucket-name> init
```

6. now backup 
* if `RESTIC_REPOSITORY` is defined:
```bash
restic backup ~/backups
```
* if not:
```bash
restic -r s3:s3.us-east-005.backblazeb2.com/<unique-bucket-name> backup ~/backups
```


# Restore

* quick guide (if already has existing restic repo):

1. source `restic-env`
```bash
source <path/restic-env
```

2. now check for latest snapshot hash and copy that
```bash
restic snapshots
```

3. restore in this format: `restic restore <snapshot-hash> --target <destination-path>`
```bash
restic restore 20ee6d7b  --target /tmp/restore
```


## Some other things
- autorestic - CLI wrapper for restic (yaml files ez)

> Both are fine choices for what you're trying to do. I'd suggest trying both with some smaller backup sets and see which one makes more sense to you.
  I went back and forth between borg and restic, finally settled on [autorestic](https://github.com/cupcakearmy/autorestic) with 1 sftp and 2 s3-compatible backends.
  
- backrest - web ui with native rclone support

> I was in the same boat as you and decided to use [backrest](https://github.com/garethgeorge/backrest). They have an alpine docker image that comes with rclone, so it was really easy to setup for a use case like yours. 
  I’d recommend storing your rclone config with the backrest config, which you can then define in a docker compose file for easy reproducibility

From this https://www.reddit.com/r/selfhosted/s/fCFRbbQTbJ 
