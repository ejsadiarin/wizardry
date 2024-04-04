---
tags:
  - SSH
  - Linux
date: 2024-02-07T21:14
title: Why does SSH freeze when idle?
---
<!-- 2024-02-07-2114 (February 7, 2024 at 9:14 PM) -->

# Why does SSH freeze when idle?
- It disconnects after a while of inactivity.
- The "freeze" is the part where the server waits for the client to send something (some input, event, etc.), that will
never come :(

# Solutions

## 1. SSH configuration
### From the server:
- edit file: `sudo vim /etc/ssh/sshd_config`
- add/set: 
```bash
ClientAliveInterval 60
ClientAliveCountMax 10000
TCPKeepAlive yes # no need if ClientAliveInterval and ServerAliveInterval are set low enough
```
- save and restart the sshd service: `sudo service sshd restart` or `sudo service ssh restart`
### From the client:
- edit file: `sudo vim /etc/ssh/ssh_config` (notice ssh_config not sshd_config)
- add/set: 
```bash
Host *
ServerAliveInterval 60 # or ServerAliveInterval 100
```
- save and restart the sshd service: `sudo service sshd restart` or `sudo service ssh restart`

## 2. use `mosh`
- see [[setup-mosh#setup]]

### NOTE
> Whenever any modification is performed on a Production instance, please ensure to create a backup.
- **`ClientAliveInterval`:** number of seconds that the server will wait before sending a null packet to the client (to keep the connection alive).
- **`ClientAliveCountMax`:** Server will send alive messages to the client even though it has not received any message back from the client.
- **`ServerAliveInterval`:** number of seconds that the client will wait before sending a null packet to the server (to keep the connection alive).
- **NULL packet:** is sent by the server to the client. The same packet is sent by the client to the server. A TCP NULL packet does not contain any controlling flag like SYN, ACK, FIN etc. because the server does not require a reply from the client. The NULL packet is described here: https://www.rfc-editor.org/rfc/rfc6592

# References
- [https://unix.stackexchange.com/questions/200239/how-can-i-keep-my-ssh-sessions-from-freezing](https://unix.stackexchange.com/questions/200239/how-can-i-keep-my-ssh-sessions-from-freezing)
