---
title: Setting Up a Cloud VM for Individual Use
date: 2024-02-06-1919 (February 6, 2024 7:19 PM)
tags:
- Linux
- Server
- Cloud
---

# Setting Up a Cloud VM for Individual Use
- get a Linux server up and running on the cloud via Linode, DigitalOcean, AWS EC2, or any VPS or VM in the cloud (look for one with free credits)
1. Enable Automatic Updates (only if the Linux system is for private/individual use only)
    
    ```bash
    apt update
    apt upgrade
    apt install unattended-upgrades
    dpkg-reconfigure --priority=low unattended-upgrades
    ```
    
    - or perhaps create a cronjob to update once a month
        - note that cronjobs don’t test the updates
        - good for private/individual use only
    - schedule updates every month if the Linux Server is a production system (essentially, don’t enable automatic updates) → for corporate/business setting
        - also always test updates in a test/dev/QA environment before making changes to prod
    - The ideal situation is using a repository host, whether that's something like Red Hat Satellite, Oracle Spacewalker, or using simple webserver which synchronizes repositories.
        - You then control your packages on the upstream, so that when they are downloaded by the host - only the packages that have been tested are applied.
        - This is how we automate patching - determine what updates we want > synchronise packages to repo host > create test environment to mimic prod > schedule ansible jobs via Tower to auto patch test hosts with smoke tests > when smoke tests pass, execute job on prod > run smoke tests, and if it fails, execute a job to undo the patch.
2. Limited User Account
    - create a user with `adduser <username>`
    - add to sudo user groups with: `usermod -aG  sudo <username>`
    - remember to add the public key to this new `<user>`
3. Add a SSH keypair and disable password for server login using SSH (see above)
    - can change ssh port of remote server in `/etc/ssh/sshd_config`
4. Enable Firewall
    - check open ports and sockets: `sudo ss -tupln`
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
5. Make server to be unpingable (disable ping from external sources)
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
