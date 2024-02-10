---
title: GPG
date: 2024-02-10-16:00 (February 10, 2024 4:00 PM)
tags:
- Encryption
---

# GPG with GnuPG

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

## Revoking keys with revoke certificates
`gpg --import` revoke-cert.asc
- this will automatically revoke the key

## Exporting keys (Backing up)
`gpg --export-secret-keys --armor --output <filename>.gpg <email>` - private key?
`gpg --export --armor <email>` - public key
`gpg --export --armor --output <filename>.gpg.pub <email>` - public key in a file

## gpg-agent
- `gpg-agent` is a daemon to manage secret keys for GPG
- `ps -A | grep gpg-agent` - check if gpg-agent is running
- if want, create gpg-agent.conf file in ~/.gnupg/ and add `use-standard-socket` to it
```bash
default-cache-ttl 604800 # 7 days
max-cache-ttl 604800 # 7 days
# use-standard-socket
```
