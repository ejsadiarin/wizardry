---
tags:
  - Linux
  - Server
  - Cloud
  - How-To
date: 2024-02-06T19:19
title: How to Properly Setup a Linux Machine
---
<!-- 2024-02-06-1919 (February 6, 2024 7:19 PM) -->

# How to Properly Setup a Linux Machine

**This guide is for Debian-based servers**
- install iso image here: [https://www.debian.org/](https://www.debian.org/)
- also see [\[HowTo\] Your first Debian server](https://forums.debian.net/viewtopic.php?t=153625)

*NOTE: applicable to all new servers, cloud VMs, and linux machines.*
- you can get a Linux server up and running on the cloud via Linode, DigitalOcean, AWS EC2, or any VPS or VM in the cloud (look for one with free credits)

## Prerequisites

1. install and configure Debian successfully and login as `root`
    - Connect your future server to a keyboard, screen and mouse and proceed with the install.
    - Select Graphical Install
    - Select language
    - Select country, location, territory and locale
    - Enter hostname. A fitting hostname would be something like server.
    - Select domain name
    - Give the root user a password: This howto assumes that you will set a root password for your server. If you prefer sudo, that’s fine, but all the root-commands below will be given as root user. For security reasons, please set strong passwords/passphrases. A server with a root password like "root" toor", "1234" or similar, will likely be hacked the same day if the SSH-server is reachable from the Internet.
    - Enter the name for the user account.
    - In partitioning, select Guided – use entire disk and All files in one partition.
    - Select Finish partitioning and write changes to disk.
    - Select «Yes» when asked «Write changes to disk?»
    - When the partitions are finished formatting, the base system is being installed.
    - Select network mirror.
    - Leave HTTP-proxy blank unless you have one.
    - Skip the package survey by selecting «No».
- The software selection screen is very important: Only install these: \<SSH-server\> and \<Standard system utilities\>. If you want to run a web server, you can also enable that.
    - When asked if you want to install Grub to your primary drive, select «Yes».
    - The system installation is finished.
    - After installation, you will be greeted by a black screen.
    - Just log in using root as login and then enter your root password.

2. Get IP address (static or via dhcp)

**NOTE: If you are behind a router, some of them will assign the devices behind them the same address every time based on their MAC addresses. If this is the case for you, this step can be skipped.**

In case you need to set a static IP, you can follow a few simple steps below.

- It is a good idea to make a backup of your old config-file
```bash
cp /etc/network/interfaces /etc/network/interfaces.bak
```

- install `net-tools` package and NetworkManager
```bash
apt install net-tools network-manager
```

- get current network interface (via `ifconfig`) and current route/gateway and netmask information (via `netstat`)
    - `ifconfig`: look for ethernet interface like `eth0` or `enp1s0` or `wlan0` (for wifi) or any interface excluding `lo`
    - `netstat -nr`: look for gateway and genmask
```bash
ifconfig
netstat -nr
```

- edit `/etc/network/interfaces` using `vi` or `nano`
```conf 
# The loopback interface
auto lo
iface lo inet loopback

# The primary network interface
auto eth0
iface eth0 inet static
       address 192.168.1.10 #this is the static IP address I want to set
       netmask 255.255.255.0 #use the info from the last line above
       network 192.168.1.0  #use the info from the last line above
       gateway 192.168.1.1 #your router's address/gateway
```

*If something went wrong, you can restore the old version with this command:*

```bash
cp /etc/network/interfaces /etc/network/interfaces.bak
```

4. install `sudo` to have user sudo commands
```bash
apt install sudo
```

## Proper Configuration

### 1. Limited User Account

- switch to `root`:
```bash
su -
```

- if not yet created a user, create a user with:
```bash
adduser <username>
```

- add to `sudo` user groups with: 
```bash
usermod -aG sudo <username>
```

- switch to user:
```bash
su <username>
```

### Add Debian sources list

- please read [https://wiki.debian.org/SourcesList](https://wiki.debian.org/SourcesList) and see what sources you need
- then edit `/etc/apt/source.list` (below is a basic configuration):

```conf
deb http://deb.debian.org/debian/ bookworm main non-free-firmware
deb-src http://deb.debian.org/debian/ bookworm main non-free-firmware

deb http://security.debian.org/debian-security bookworm-security main non-free-firmware
deb-src http://security.debian.org/debian-security bookworm-security main non-free-firmware

# bookworm-updates, to get updates before a point release is made;
# see https://www.debian.org/doc/manuals/debian-reference/ch02.en.html#_updates_and_backports
deb http://deb.debian.org/debian/ bookworm-updates main non-free-firmware
deb-src http://deb.debian.org/debian/ bookworm-updates main non-free-firmware


deb http://deb.debian.org/debian bookworm-backports main contrib non-free non-free-firmware
deb-src http://deb.debian.org/debian bookworm-backports main contrib non-free non-free-firmware
```

- then update repositories and download upgrades:
```bash
sudo apt update && sudo apt upgrade -y
```


### 2. Configure SSH

- for remote access, add your main computer's public key to the server's `~/.ssh/authorized_keys`
    - remember to run the command as the `user` (not `root`) when copying public key
```bash
# on your main computer
ssh-copy-id <user>@<ip>
```


#### Key-based SSH login

- if you wish to just ssh login via password then skip this part
- enhance ssh security by key-based login only [see `sshd` Disable Password for login](passwordless-ssh-login.md)
    - NOTE: can change ssh port of remote server in `/etc/ssh/sshd_config`


#### Start and Enable `sshd` on startup

- sshd will make your server an ssh server where clients can login 
    - *Key-based SSH login: allowed clients are those who have their public keys on your server's `~/.ssh/authorized_keys`)*
    - *Password SSH login: allowed clients are those knows your computer password*

```bash
sudo systemctl start sshd
sudo systemctl enable sshd
```


#### 3. Prevent brute forcing of passwords

- no need to set this up if you use Key-based SSH login (though you can still do this for more security)
- to detect the IPs of failed logins
- recommended: `fail2ban`

##### fail2ban

```bash
sudo apt install fail2ban
```


##### crowdsec

- install bouncer for `crowdsec`:
  - needed since `crowdsec` just watches the logs for these things, but doesn't do anything to it 

### 4. Enable Automatic Updates (useful for servers)

*Services with known vulnerabilities are a huge attack vector. You can get automatic security updates with unattended-upgrade*

#### using `unattended-upgrades`

- install `unattended-upgrades`: 
```bash
sudo apt install unattended-upgrades
```

- enable automatic upgrades
```bash
dpkg-reconfigure unattended-upgrades # dpkg-reconfigure --priority=low unattended-upgrades
```

- then edit these files:

	- `/etc/apt/apt.conf.d/50unattended-upgrades`:
    - to `true`
  ```bash
  ...

  // Automatically reboot *WITHOUT CONFIRMATION* if
  //  the file /var/run/reboot-required is found after the upgrade
  Unattended-Upgrade::Automatic-Reboot "true";

  // Automatically reboot even if there are users currently logged in
  // when Unattended-Upgrade::Automatic-Reboot is set to true
  Unattended-Upgrade::Automatic-Reboot-WithUsers "true";

  // If automatic reboot is enabled and needed, reboot at the specific
  // time instead of immediately
  //  Default: "now"
  Unattended-Upgrade::Automatic-Reboot-Time "02:00";

  ...
  ```

  - `/etc/apt/apt.conf.d/20auto-upgrades`:
    - make sure these are set to "1"
  ```bash
  APT::Periodic::Update-Package-Lists "1";
  APT::Periodic::Unattended-Upgrade "1";
  ```

#### Some sentiments on automatic updates

> or perhaps create a cronjob to update once a month
> - note that cronjobs don’t test the updates
> - good for private/individual use only
	
> schedule updates every month if the Linux Server is a production system (essentially, don’t enable automatic updates) → for corporate/business setting
> - also always test updates in a test/dev/QA environment before making changes to prod
	
> The ideal situation is using a repository host, whether that's something like Red Hat Satellite, Oracle Spacewalker, or using simple webserver which synchronizes repositories.
> - You then control your packages on the upstream, so that when they are downloaded by the host - only the packages that have been tested are applied.

> This is how we automate patching:
> 1. determine what updates we want
> 2. synchronise packages to repo host  
> 3. create test environment to mimic prod 
> 4. schedule ansible jobs via Tower to auto patch test hosts with smoke tests 
> 5. when smoke tests pass, execute job on prod 
> 6. run smoke tests, and if it fails, execute a job to undo the patch.

#### Using a script (cronjob) for automatic updates

- ref: [section 10.1 in HowTo your first Debian server](https://forums.debian.net/viewtopic.php?t=153625)

*A different option is to create a custom script to keep the server up tp date. This script will not only download and install all upgrades. It will also reboot the server if the kernel has been upgraded and clean up downloaded and no longer needed package files.*

- create script in server
```bash
vi /usr/local/bin/upgrade.sh 
```

- copy paste the contents
```bash
#!/bin/bash

# Read the current kernel version
current_kernel=$(uname -r|cut -d '-' -f 1)

# Update package list
su -c "apt update"

# Upgrade installed packages
su -c "apt upgrade -y"

# Check if kernel has been upgraded
new_kernel=$(dpkg -l linux-image-*.*|awk '/^ii/{print $2}'|grep -v -e $(uname -r|cut -f1,2 -d"-")|grep -e [0-9]|cut -d'-' -f3)

if [[ $current_kernel != $new_kernel ]]; then
    echo "Kernel has been upgraded, rebooting..."
    su -c "reboot"
else
    echo "Kernel has not been upgraded"
fi

# Clean up downloaded package files
su -c "apt autoclean -y"
```

- make it executable
```bash
chmod 111 /usr/local/bin/upgrade.sh
# or chmod -x /usr/local/bin/upgrade.sh
```

- make it a cronjob
```bash
crontab -e
```

- paste contents into the file and save:
```bash
0 3 * * * /usr/local/bin/upgrade.sh > /var/log/upgrade.log 2>&1
```

**Your server should now be maintenance free.**



### 4. Enable Firewall and Configure

- two ways via `ufw` or `nftables`

`ufw` is a frontend/wrapper for legacy iptables
`nftables` is a frontend/wrapper to Debian's firewall netfilter (i prefer/recommend using this)

#### nftables

-  check for listening services
```
netstat -tupln
# or lsof -nP -iTCP -sTCP:LISTEN
```

- install nftables

- edit the `/etc/nftables.conf` to only allow ports 22, 80, 443 (which is the standard best practice)
    - remove ports 80 and 443 if you're not running a web server
    - **NOTE: do not forget to open port 22 for SSH access, failure to do so will result to locking you out of the server**
```conf
#!/usr/sbin/nft -f
flush ruleset

table inet firewall {
    chain inbound {
        type filter hook input priority 0; policy drop;
        ct state { established, related } accept
            ct state invalid drop
            iifname lo accept
            tcp dport { 22, 80, 443 } accept
    }
    chain forward {
        type filter hook forward priority 0; policy drop;
    }
    chain output {
        type filter hook output priority 0; policy accept;
    }
}
```


#### ufw
- check open ports/sockets and services running behind ports: `sudo ss -tupln` or `sudo netstat -tupln`
- install `ufw` : `sudo apt install ufw` (if on debian-based distro)
    - check status of ufw: `sudo ufw status`
- allow SSH port:
    
    ```bash
    sudo ufw allow <SSH-port-here> # by default SSH port is 22
    ```
    
- enable ufw: `sudo ufw enable` && see status: `sudo ufw status`

allowing more ports to firewall (ex. port 80/tcp for http websites)

- example, you install apache2: `sudo apt install apache2`
- allow port in firewall to be accessed

```bash
sudo ufw allow 80/tcp
```

- go to browser to check then do URL search: `<ip-addr-public>`
    - if not working, restart firewall daemon: `sudo ufw reload`

### 5. Make server to be unpingable (disable ping from external sources)
- go to `/etc/ufw/before.rules` with vim or nano
- locate to comment `# ok icmp code for INPUT` and add this line:

```bash
# - existing code - #

# ok icmp code for INPUT
-A ufw-before-input -p icmp --icmp-type echo-request -j DROP # add this
...exiting similar lines of code

# - existing code - #
```

- restart firewall: `sudo ufw reload`
  - ping again the public IP of your server, if still pinging reboot the server: `sudo reboot`
  - ssh to your server `ssh <user>@<server-ip>` or `ssh <user>@<server-ip> -p <custom-ssh-port>` (if you changed the default ssh port)

### 6. Start, enable, stop, disable, and check services with systemd

```bash
sudo systemctl status <service> # check
sudo systemctl enable <service>
sudo systemctl start <service>
sudo systemctl stop <service>
sudo systemctl disable <service>
```

- example (to check if sshd (computer as ssh server) is running):
```bash
sudo systemctl status sshd
```



## Verify the server's integrity
```
dpkg --verify
```

##  Further reading

The Securing Debian Manual by Javier Fernández-Sanguino Peña is a highly recommended and comprehensive guide on how to harden and secure a Debian server and services: 
[https://www.debian.org/doc/manuals/secu ... ex.en.html](https://www.debian.org/doc/manuals/securing-debian-manual/index.en.html)


## Dist-upgrading your server

In you want to upgrade your server to the next stable version, it's unproblematic to do this over SSH. Please read the release notes for good instructions: [https://www.debian.org/releases/stable/releasenotes](https://www.debian.org/releases/stable/releasenotes)


That's it. Feel free to share improvements or offer tips on how to install and configure services.
