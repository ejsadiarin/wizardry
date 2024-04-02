---
id: use-pass-for-2FA-MFA-otp-auth
aliases: []
tags:
  - Security
  - CLI (pass, gpg)
  - Linux
  - Password
  - OTP, 2FA, MFA
  - How-To
date: 2024-02-10-19:15 (February 10, 2024 7:15 PM)
title: Using pass-otp for 2FA, MFA, OTP authentication
---

# Using pass-otp for 2FA, MFA, OTP authentication
- install `pass` (password manager) and `pass-otp` (for TOTP support)
- install `zbarimg` for QR code scanning (most likely it is installed on most systems)
- this is a better alternative to Authenticator Apps (Google Authenticator, etc.) that requires a phone

# Process
1. Download the QR code from the service (e.g. Google, GitHub, etc.)
2. use `zbarimg` to scan the QR code:
```bash
zbarimg -q qr.png
# Output will look similar to this:
QR-Code:otpauth://totp/Google:example?secret=EXAMPLESECRET&issuer=Google
```
3. use `pass otp` plugin to add the secret:
```bash
pass otp add serviceotp
# or if you want to specify the issuer:
pass otp add Google/example
# You will be prompted to enter URI (only paste from otpauth://...end):
otpauth://totp/Google:example?secret=EXAMPLESECRET&issuer=Google # example URI to paste
```
4. now to get the 6-digit code for 2FA/MFA:
```bash
pass otp serviceotp
# or if you specified the issuer:
pass otp Google/example
# you will then see the 6-digit code
# NOTE: this changes every x seconds (like in Authenticator Apps)
```

# References
- [this yt vid](https://www.youtube.com/watch?v=sVkURNfxPd4&t=67s&pp=ygUIcGFzcyBvdHA%3D)
