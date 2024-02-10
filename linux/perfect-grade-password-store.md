---
title: The Perfect Grade Password Store
date: 2024-02-10-18:22 (February 10, 2024 6:22 PM)
tags:
- Security
- CLI (pass, gpg)
- Linux
- Password
- OTP, 2FA, MFA
---

# The Perfect Grade Password Store
- **pass** is a password manager that uses GPG and Git for Linux
  - CLI-based (although there are plugins, see below)
  - can be used for scripting
- **NOTE: all passwords live in `~/.password-store`**
  - all passwords are encrypted with GPG
- `gpg-agent` is used for caching the password for a certain duration
  - can be configured to stay authenticated for a few minutes
- entries can be organized in a tree-like structure
  - add `/` for directory grouping
  - e.g. `email/gmail`, `email/github`, etc.

## Setup
- Common uses:
```bash
# initialize password store:
pass init <any-name-or-email> # preferably your email

# add/insert an entry:
pass add <entry> # e.g. pass add email/gmail
pass insert <entry> # e.g. pass insert email/gmail

# to see all entries:
pass # or pass ls 

# to copy the password to clipboard (no output to stdout):
pass -c <entry> # or pass show -c <entry>

# to remove a password entry:
pass rm <entry> 

# to see the password for an entry (outputs to stdout, better to use -c especially in public):
pass <entry> # or pass show <entry>

# to edit an entry in a text editor you want:
pass edit <entry>

# to generate a random password using /dev/urandom (good if you suck at passwords):
pass generate <entry> 15
```

- Use Git for version control (highly recommended, since all are encrypted no need to worry about privacy):
```bash
pass git init
pass git remote add origin <git-remote-repo-url>
pass git push # to push to the remote
```
- **NOTE: `pass` creates a `git commit` (locally) each a password is manipulated (when adding, removing, editing, etc.)**
  - then `pass git push` it to update the remote
- more details on `pass git` [here](https://git.zx2c4.com/password-store/about/#EXTENDED%20GIT%20EXAMPLE)

## Importing keys from another machine
- 
```bash
```

```bash
pass
gpg --generate-key
# if you want to change expiry duration:
gpg --edit-key <generated-public-key>
expire
# you will be prompted for options (0 means no expiry, etc.)
0 # if you want
save
exit # if save did not exit you to the gpg-shell
pass init <generated-public-key>
```

## Plugins
- `pass-otp` - for TOTP (Time-based One-Time Password) support
  - see more details [here](./use-pass-for-2FA-MFA-otp-auth.md)
  - `pass otp <entry>` - get TOTP for an entry
  - `pass otp <entry> | xclip -sel clip` - copy TOTP to clipboard
  - `pass otp -c <entry>` - copy TOTP to clipboard
  - `pass otp -c <entry> | xclip -sel clip` - copy TOTP to clipboard

# References
- main docs/website: [passwordstore.org](https://www.passwordstore.org/)
