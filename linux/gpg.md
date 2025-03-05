---
tags:
  - Encryption
  - How-To
date: 2024-02-10T16:00
title: GPG
---

<!-- 2024-02-10-1600 (February 10, 2024 4:00 PM) -->

# GPG with GnuPG

- before anything, make sure that GPG_TTY is added to .bashrc or .zshrc or any shell initialization file:

```
export GPG_TTY=$(tty)
```

## Daily Usage

- with `tar`:

```bash
# compress and encrypt with symmetric password only (flexible)
tar czf - <file-or-dir> | gpg --symmetric -o <file-or-dir>.tar.gz.gpg
# decrypt and extract (will be prompted for password)
gpg --decrypt <file-or-dir>.tar.gz.gpg | tar xz

# encrypt using keypair (need to import the ultimate keypair first, see below)
gpg --encrypt <file-or-dir>.tar.gz # encrypting a tar.gz file
gpg --encrypt <file-or-dir> # encrypting any file or directory
gpg -e -r <email> <file-or-dir>
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

## Adding email or UID to an existing key

```bash
gpg --edit-key <email>
gpg>adduid
# enter details...
gpg>O
# make it never expire
gpg>expire
gpg>0
gpg>y
# make ultimate
gpg>trust
gpg>5
# then save
gpg>save
```

## Backing up

- Exporting:

```bash
# export private key:
gpg --export-secret-keys --armor --output private.gpg <email> # or: gpg --export-secret-keys <email> > private.gpg
# export public key:
gpg --export --armor --output public.gpg <email> # or: gpg --export <email> > public.gpg

# OPTIONAL: move keys to a directory and compress-encrypt it
mkdir -p ultimate-gpg-keys
# compress and encrypt with symmetric password only (flexible)
tar czf - <file-or-dir> | gpg --symmetric -o <file-or-dir>.tar.gz.gpg

# for more info: see above sections to encrypt/decrypt wil tar and gpg --symmetric
```

## Importing to another machine:

```bash
# get keys
scp -r user@local:/path/to/exported-keys.gpg ~/imported-keys
# OR decrypt and extract (will be prompted for password)
gpg --decrypt <file-or-dir>.tar.gz.gpg | tar xz

# import
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

## edit gpg key email

```bash
gpg --list-secret-keys --keyid-format=long
gpg --edit-key <email-or-uid>
gpg> adduid
Real name: <Name>
Email: <New-Email>
Comments: <can-be-empty>
Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? <O-for-Okay>
gpg> list
gpg> uid 2
gpg> trust
gpg> 5 # for ultimate
gpg> y
gpg> expire
gpg> 0 # does not expire at all
gpg> y
gpg> uid 1 # old email uid 
gpg> deluid
gpg> y
gpg> save
```

## edit gpg key password

```bash
gpg --list-secret-keys --keyid-format=long
gpg --edit-key <email-or-uid>
gpg> passwd
# then just follow instructions
gpg> save # for safety
```

## text-based gpg PIN entry  (console mode gpg pinentry)

- ref: [https://superuser.com/questions/520980/how-to-force-gpg-to-use-console-mode-pinentry-to-prompt-for-passwords](https://superuser.com/questions/520980/how-to-force-gpg-to-use-console-mode-pinentry-to-prompt-for-passwords)

> [!IMPORTANT]
> Usecases:
> - for servers without window GUI
> - when encountering error: `gpg: problem with the agent: Screen or window too small`

### fast (temporary) solution

1. first, make sure that GPG_TTY is added on .bashrc or .zshrc:
```
export GPG_TTY=$(tty)
```

2. then update
```bash
gpg-connect-agent updatestartuptty /bye >/dev/null
```


### permanent solution

- To change the pinentry permanently, append the following to your `~/.gnupg/gpg-agent.conf`:

```bash
pinentry-program /usr/bin/pinentry-tty
```

(In older versions which lack pinentry-tty, use pinentry-curses for a 'full-terminal' dialog window.)

- Tell the GPG agent to reload configuration:

```bash
gpg-connect-agent reloadagent /bye
```


### on Debian box

1. install pinentry-tty
```bash
sudo apt install pinentry-tty
sudo update-alternatives --config pinentry
```

2. select pinentry-tty
