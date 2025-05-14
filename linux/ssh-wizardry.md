---
tags:
  - SSH
  - Wizardry
  - How-To
  - Security
date: 2024-03-12T15:07
title: SSH Wizardry
---
<!-- 2024-03-12-1507 (March 12, 2024 3:07 PM) -->

# SSH Wizardry (The Only Guide You'll Ever Need)
- install `openssh`

- to make a machine "`ssh`-able", enable `sshd`:
```bash
# for systemd
sudo systemctl start sshd
sudo systemctl enable sshd # to start on boot
```

- with `tailscale`, you have a VPN that can be used to `ssh` into your machine from anywhere in the world.
  - even behind a NAT or firewall, and without `authorized_keys`
```bash
tailscale status

# on any terminal (even on android termux)
ssh <username>@<tailscale-ip>

# from machine to termux
ssh -p 8022 <username>@<tailscale-ip>
```
# Generate SSH Keys

```bash
ssh-keygen -t ed25519 -b 4096 -C "email@domain.com"
```

# SSH Port Forwarding (SSH Tunneling)

## Practical Examples

```bash
# from local (forward port 8385) --> to access remote_server Syncthing instance (running on port 8384) 
ssh -L 8385:localhost:8384 user@remote_server
```

## Local Forwarding
- forward traffic (or setup tunnel) on a *LOCAL* port access a service from *REMOTE* 
- allows *LOCAL* to use a *REMOTE* service
```bash
# forward traffic on localhost:8080 to service.example.com:80 (different host) using gw.example.com as a jump_server
ssh -L 8080:service.example.com:80 gw.example.com
# ^ is the same as: ssh -L 8080:service.example.com:80 user@jump_server

# using a bind address
ssh -L 127.0.0.1:8080:service.example.com:80 gw.example.com
ssh -L localhost:8080:service.example.com:80 gw.example.com

# forwarding localhost:8080 to remote_server's localhost:80
ssh -L 8080:localhost:80 user@remote_server
```
- see the `LocalForward` option in the `ssh_config` file
- [see SSH official docs for more details (recommended)](https://www.ssh.com/academy/ssh/tunneling-example)

### Use cases

- Tunneling sessions and file transfers through jump servers
- Connecting to a service on an internal network from the outside
- Connecting to a remote file share over the Internet

## Remote Forwarding
- forward traffic (or setup tunnel) on a *REMOTE* port to access a service from *LOCAL*
- allows *LOCAL* to use a *REMOTE* service
```bash
ssh -R 8080:localhost:80 user@remote_server
ssh -R 8080:local_host:80 user@jump_server

# if GatewayPorts clientspecified is configured:
ssh -R <client-ip-address>:8080:localhost:80 host69.aws.example.com
```
- see the `GatewayPorts` option in the `ssh_config` file
- [see SSH official docs for more details (recommended)](https://www.ssh.com/academy/ssh/tunneling-example)

### Use cases
- Share a service like a web server (running locally) to the Internet
- Used in Minecraft servers -> allowing other people to connect to your local Minecraft server
- Setup reverse shell

more refs: [yt vid BugsWriter](https://www.youtube.com/watch?v=lKIRHDY7OhU)

# SSH with `nc`
- see [ssh-nc](ssh-nc.md)

## Using SSH -R for Reverse Shell
- The -R option is used to set up a reverse tunnel from the remote machine to the local machine. This is particularly useful for creating a reverse shell, where the remote machine can execute commands on the local machine.

### 1. **On the remote machine (the one you want to control):**
``` bash
ssh -R 90:localhost:22 user@your_local_machine_ip
```
- This command forwards port 90 on the remote machine to port 22 (SSH) on the local machine. Replace `user@your_local_machine_ip` with your actual username and the IP address of your local machine.

### 2. **On the local machine (the one you want to control from):**
- Listen on the forwarded port:
``` bash
nc -l -p 90
```

- Then, connect to the remote machine using the forwarded port:
``` bash
# Replace user with your actual username on the local machine.
ssh -p 90 user@localhost
```

## Using SSH -L for Local Port Forwarding
- The -L option is used to forward a port from the local machine to the remote machine. This is useful for accessing services on the remote machine as if they were running locally.

- this "reverse" shell `ssh -L` is used only to access LOCAL from a REMOTE machine
  - useful when you already have a reverse shell with `ssh -R` on a Victim PC and running this to access other PCs in the same internal network (LAN)

### 1. **On the local machine:**
``` bash
ssh -L 90:localhost:22 user@remote_machine_ip
```
- This command forwards port 90 on the local machine to port 22 (SSH) on the remote machine. Replace `user@remote_machine_ip` with your actual username and the IP address of the remote machine.

### 2. **On the remote machine:**
- You can now access the local machine's SSH service through the forwarded port:
``` bash
# Replace user with your actual username on the local machine.
ssh user@localhost -p 90
```

### NOTE before port forwarding anything:
Ensure the following:
- You have proper authentication and authorization in place.
- You're only forwarding necessary ports.
- You're aware of the potential for unauthorized access if the forwarded ports are exposed to the internet.

## See more different approach on Port Forwarding with SSH:
### Forwarding a service on a remote machine to be accessible on your local machine, on Linux:

`~/.ssh/config`

Under a Host entry:
```
LocalForward localIP:localPort remoteIP:remotePort
```

- Establish the ssh session, connect to localIP:localPort on your machine and you have access.

- Add as many of these directives as you want. 
- Bind them to the default port on your local machine, use any 127.x.x.x IP and set up a etc/hosts entry, make it easy on yourself. 
- Webserver for docker? Local IP 127.1.2.3, port 80, add a hosts entry to make it docker.remote or something.

## using ProxyJump directive in ~/.ssh/config
ProxyJump is a directive you might be interested in. It's a streamlined version of ProxyCommand which is another useful directive.

For the use case of connecting to an SSH server (linuxpc) behind a firewall or unroutable network, I can just run:
```bash
ssh linuxpc
```

With a properly configured `~/.ssh/config` this works from any network.

- The `~/.ssh/config` file on the clients would look something like this:
```config
Host relay
    HostName relay.public.ip
    HostKeyAlias relay
    Port 2222

# Don't jump through the relay when on home network
Match Host linuxpc localnetwork 192.168.1.0/24
    ProxyJump none

Host linuxpc
    HostKeyAlias linuxpc
    Port 22
    User linuxuser
    ProxyJump relay
```

- I used to do this with SSH port forwarding, then migrated to the ProxyCommand directive, and when the ProxyJump directive was introduced in OpenSSH 7.3 I started to use that because it is so simple.

### How to access RDP over a reverse SSH Tunnel
- see guide [how-to-access-rdp-over-reverse-ssh-tunnel](how-to-access-rdp-over-reverse-ssh-tunnel.md)

# Securing SSH
> None of my Arch machines are accessible to the public, but I do use fail2ban on my servers to protect *more* than just SSH. 
> But TBH, for just SSH, fail2ban would be quite far down the list for a smart administrator.

### tldr on securing SSH:

1. Turn off password auth, enable key auth.  That eliminates 99.99% of risk and you could stop here.
2. Change SSHd to a different port. That eliminates 99.99% of *log spam* from random attacks. Feel free to stop here.
3. [Set SSHd to use more modern ciphers](https://infosec.mozilla.org/guidelines/openssh.html) to eliminate many dumb SSH bots
4. [Drop SSH user agents you'll never use](https://news.ycombinator.com/item?id=39852865) to eliminate really dumb SSH  bots
5. Use fail2ban/sshguard to block IPs that have multiple failed attempts. If you've done #1, this isn't  necessary. If you've done #2, this won't prevent many log messages, because there won't be many.

ref: https://www.reddit.com/r/archlinux/s/smyPx2Bp5F

# Access Desktop Environment / DE though an SSH connection
### 4 options:
1. ssh -X
2. x2go
  - recommended?
3. NoMachine
4. ssh port forwarding and VNC
- if using 4: make sure to resolve conflicting ports like so:
> You are running into port conflict if you have VNC running and its already listening on 5901, you cant setup another listener using 5901.
>
> from: `ssh -L 6000:localhost:5901`
>
> Change it to something like: `ssh -L 6000:localhost:5901`
> 
> So port 6000 will forward to 5901
> 
> If you already have VNC listening on the box, you can just use tailscale IP to hit VNC. Your connection will be already encrypted with tailscale so you dont really need to use ssh tunnels if you dont want too 
- see [ssh-port-forwarding](#ssh-port-forwarding) for more info

ref: https://www.reddit.com/r/Tailscale/s/6SzLSbzrSs
