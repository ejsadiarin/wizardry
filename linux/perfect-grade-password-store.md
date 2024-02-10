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

## Importing keys from another machine
```bash
```

IMPORTANT for Docker
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
