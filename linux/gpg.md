---
title: GPG
date: 2024-02-10-16:00 (February 10, 2024 4:00 PM)
tags:
- Encryption
---

# GPG with GnuPG

## Daily Usage
- with `tar`:
```bash
# compress and encrypt with symmetric password only (flexible)
tar czf - <file-or-dir> | pv | gpg --symmetric -o <file-or-dir>.tar.gz.gpg
# decrypt and extract (will be prompted for password)
gpg --decrypt <file-or-dir>.tar.gz.gpg | pv | tar xz

# encrypt using keypair (need to import the ultimate keypair first, see below)
gpg --encrypt <file-or-dir>.tar.gz # encrypting a tar.gz file
gpg --encrypt <file-or-dir> # encrypting any file or directory
```
- gpg common commands:
```bash
gpg --list-keys # list existing keys

gpg --full-gen-key # full process to generate a new key
gpg --gen-key # generate a new key (defaults)

gpg --delete-secret-keys <uid-or-email> # delete secret key (private key)
gpg --delete-keys <uid-or-email> # delete public key

# you can import this to other machines to revoke the key (see below how)
gpg --gen-revoke <uid-or-email> # or: gpg --generate-revocation <uid-or-email> - generate revoke certificate

gpg --edit-key <email> # if you want to change expiration date
# it will open a gpg-shell:
gpg>expire
# you will be prompted for options (0 means no expiry, etc.)
gpg>0 # if you want
gpg>save
gpg>exit # if save did not exit you to the gpg-shell

gpg --passwd <email> # change password
```

## Backing up
- Exporting:
```bash
# export private key:
gpg --export-secret-keys --armor --output private.gpg <email> # or: gpg --export-secret-keys <email> > private.gpg
# export public key:
gpg --export --armor --output private.gpg <email> # or: gpg --export <email> > public.gpg
``` 

- Importing to another machine:
```bash
# get keys
scp -r user@local:/path/to/exported-keys.gpg ~/imported-keys
gpg --import public.gpg
gpg --import private.gpg # will prompt for password
gpg --edit-key <email> # will prompt for password
# then in the gpg-shell:
trust
5 # ultimate
y # confirm
save
exit # if save did not exit you to the gpg-shell
```

## Revoking keys with revoke certificates
```bash
gpg --import revoke-cert.asc # this will automatically revoke the key
```

## gpg-agent
- `gpg-agent` is a daemon to manage secret keys for GPG
- `ps -A | grep gpg-agent` - check if gpg-agent is running
- if want, create gpg-agent.conf file in ~/.gnupg/ and add `use-standard-socket` to it for better performance
  - then restart gpg-agent
```bash
default-cache-ttl 604800 # 7 days
max-cache-ttl 604800 # 7 days
# use-standard-socket
```
