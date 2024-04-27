---
tags:
  - Linux
  - Security
  - SSH
  - How-To
date: 2024-02-06T19:14
title: Passwordless SSH Login via SSH Keypair
---
<!-- 2024-02-06-1914 (February 6, 2024 7:14 PM) -->

# Passwordless SSH Login via SSH Keypair
- perfect when configuring a Cloud Server, when self-hosting, etc.

## How it works?
- SSH uses public/private key pair to authenticate users
- The public and private key pair are cryptographically linked

## Detailed Theory (Handshake)
- Server has both public/private key
- Client has both public/private key
- 4 step process:
(1) Server sends a public key and an encrypted message (encrypted using the public key)
(2) Client will attempt to decrypt the message using its private key
    - if valid, the message can be decrypted (since it is cryptographically linked)
(3) Client will reencrypt the message with the server's public key (that is sent down on step 1)
(4) Server will attempt to decrypt the message using its private key
    - then verify:
        - if original message (server sent in step 1) === decrypted message (in step 4)

## SETUP

### 1: Make public/private key pair

- generate an ssh public key using ed25519 or RSA:
    
```bash
# -------> ED25519 <------- #
# use ed25519 (recommended)
ssh-keygen -t ed25519
# if already have an existing keypair do:
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_custom 
# --> The public key will be in ~/.ssh/id_ed25519_custom.pub

# when accessing a remote server, specify what ssh key to use:
ssh -i ~/.ssh/id_ed25519_custom <user>@<remote-server-ip>

# -------> RSA <------- #
# use RSA when specified (-b is bits, can be 3072 or higher)
ssh-keygen -t rsa -b 4096

# use RSA when specified (-C is comment, can be anything)
ssh-keygen -t rsa -b 4096 -C "descrription here"
```
    
- cd to `~/.ssh` and check if the keys exist:

PRIVATE KEY: `id_ed25519` or `id_rsa`
PUBLIC KEY: `id_ed25519.pub` or `id_rsa.pub`

- copy the public key:
```bash
cat ~/.ssh/id_ed25519.pub

# then copy the output...
```

### 2: Add public key to server

- login to your cloud server as root user or any proper user (ip and password)

```bash
# example
ssh root@<server-ip>
root@<server-ip>'s password: <password>
```

- search for `~/.ssh` directory in server
- if there is none, create one
- and set permissions to 700 (drwx------):
  
  ```bash
  chmod 700 ~/.ssh
  ```
  
- create a `authorized_keys` file in server inside `~/.ssh` directory and paste the public key inside
- if there is none, create one
- and set permissions to 600 (-rw-------):
  
  ```bash
  chmod 600 ~/.ssh/authorized_keys
  ```
  
- paste the public key inside the `authorized_keys` file using Vim (ofc use Vim) or Nano

### 3: Verify passwordless login works

- logout your server

```bash
logout
```
    
- ssh into server:

```bash
ssh root@<server-ip>
# if have multiple keypair, specify what ssh key to use:
ssh -i ~/.ssh/id_ed25519_custom <user>@<remote-server-ip>
```
    
- it should now login without asking for password

### 4: Disable password logins on server

- login to root user on server (if step 3 is successful, then you are already logged in)

```bash
ssh root@<server-ip>
```

- edit the sshd_config file (using Vim or Nano):

```bash
# remove sudo if you are already root
sudo vim /etc/ssh/sshd_config
```

- edit the following lines:

```bash
# change from yes to without-password
PermitRootLogin without-password
# change to no
PasswordAuthentication no
# change to no
ChallengeResponseAuthentication no
# change to no
UsePAM no
```

- restart sshd service:

```bash
# remove sudo if you are already root
sudo systemctl restart sshd
```

- Verify that password logins are disabled:
    
```bash
# logout to your server
logout
```

- then try to ssh into server with a random user:
    
```bash
ssh <random-user>@<server-ip>
# example: ssh asdf@123.122.121.77
```
    
- The output should be a permission denied

## SETUP (automatic way using ssh-copy-id)

- in your local machine (client), run the following command:
    
```bash
ssh-copy-id <user>@<server-ip>
```

## Why do this?

- when you expose your SSH server to the internet (like pointing to a DNS, etc.), there are malicious bots that will constantly scan and attempt to login to your server using brute force attacks.
- having a server with weak password (ex. root, admin, pass) or weak configurations will make your server vulnerable to these attacks.

# Next Steps - Optional
- see [[how-to-properly-setup-a-linux-machine-server|how to Properly Setup a Linux Machine ❯ How to Properly Setup a Linux Machine]]
