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

`gpg --list-keys` - list existing keys

`gpg --full-gen-key` - full process to generate a new key
`gpg --gen-key` - generate a new key (defaults)

`gpg --delete-secret-keys <uid>` - delete secret key (private key)
`gpg --delete-keys <uid>` - delete public key

`gpg --gen-revoke <uid>` or `gpg --generate-revocation <uid>` - generate revoke certificate
  - you can import this to other machines to revoke the key

`gpg --edit-key <public-key>` - if you want to change expiration date
  - it will open a gpg-shell:
  ```bash
  expire
  # you will be prompted for options (0 means no expiry, etc.)
  0 # if you want
  save
  exit # if save did not exit you to the gpg-shell
  ```

`gpg --passwd <email>` - change password

## Backing up
### Exporting keys
- `gpg --export-secret-keys --armor --output ~/exported-keys/private.gpg <email>` - export private key in a file
  ```bash
  # examples:
  gpg --export-secret-keys --armor --output ~/exported-keys/private.gpg <email>
  gpg --armor --export-secret-keys <email> > ~/exported-keys/private.gpg
  ``` 
- `gpg --export --armor --output <filename>.gpg <email>` - export public key in a file
  ```bash
  # examples:
  gpg --export --armor --output ~/exported-keys/public.gpg <email>
  gpg --armor --export <email> > ~/exported-keys/public.gpg
  ``` 
### Importing keys
- see [Importing Keys from Another Machine](https://github.com/ejsadiarin/wizardry/blob/main/linux/perfect-grade-password-store.md#importing-keys-from-another-machine)

## Revoking keys with revoke certificates
`gpg --import` revoke-cert.asc
- this will automatically revoke the key

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
