---
date: 2024-12-12T00:27
tags: 
- Homelab
---
<!-- 2024-12-12-0027 (December 12, 2024 12:27:52 AM) -->

# Restic + Backblaze B2 (or any S3-compatible storage) Full Guide

- ref: [https://www.backblaze.com/docs/cloud-storage-integrate-restic-with-backblaze-b2#create-a-bucket](https://www.backblaze.com/docs/cloud-storage-integrate-restic-with-backblaze-b2#create-a-bucket)

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

## Creating Backups and using Restic

- Backing Up (full command with `-r`)
```bash
restic -r s3:s3.us-east-005.backblazeb2.com/<unique-bucket-name> backup ~/backups
```

> [!IMPORTANT]
> NOTE: no need `-r <bucket-endpoint>` since `RESTIC_REPOSITORY` (as environment variable) is already defined by the `restic-env`:

- Backing Up (if `RESTIC_REPOSITORY` is defined)
```bash
restic backup ~/backups
```

- See restic snapshots
```bash
restic snapshots
```

## Restoring from Backups

> [!IMPORTANT]
> NOTE: Make sure that you have the configured `/etc/restic-env` and  `/etc/restic-password`
> - this ensures that you have the necessary secret keys, remote repos endpoints, etc.

- To restore a snapshot to a directory, supply the snapshot ID and specify the target directory. Restic restores all of the files from the backup, with their full paths, starting under that directory:
```bash
restic restore 20ee6d7b  --target /tmp/restore
```

- can also mount the restic snapshot database and browse snapshots from that:
    - see here:[https://www.backblaze.com/docs/cloud-storage-integrate-restic-with-backblaze-b2#restore-files](https://www.backblaze.com/docs/cloud-storage-integrate-restic-with-backblaze-b2#restore-files)
