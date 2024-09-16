---
tags:
  - Linux
  - How-To
  - Wizardry
date: 2024-03-13T19:23
title: How to get the whole System Information in a Linux Machine
---

<!-- 2024-03-13-1923 (March 13, 2024 7:23 PM) -->

# How to get the whole System Information in a Linux Machine

- `inxi -F` - prints the detailed system information (complete)

- `uname` command:
  - `uname -a` - prints all the system information (Host-level)
  - `uname -s` - prints the kernel name
  - `uname -n` - prints the network node hostname
  - `uname -r` - prints the kernel release
  - `uname -v` - prints the kernel version
  - `uname -m` - prints the machine hardware name
  - `uname -p` - prints the processor type
  - `uname -i` - prints the hardware platform
  - `uname -o` - prints the operating system
- `df -hT` - prints the disk space usage (human-readable and with filesystem type)
- `free -h` - prints the memory (RAM) usage in a human-readable format
- `lscpu` - prints the CPU information
- `lsblk` - prints the block devices (disks) information
- `lspci` - prints the PCI devices information
- `lsusb` - prints the USB devices information
- `lsmod` - prints the loaded kernel modules
- `lsof` - lists open files
- `uptime` - prints the system uptime and load averageks
- `lsb_release -a` - prints all the distro-specific information (Linux Standard Base)
- `ifconfig` - see network interfaces (old way)
- `ip addr show` - see network interfaces (new way)
- `ip link | awk '/state UP/ {print $2}' | tr -d :` - see what interfaces are currently up

## `cat` the `/proc` directory

- `cat /proc/cpuinfo` - prints the CPU information
- `cat /proc/meminfo` - prints the memory information
- `cat /proc/version` - prints the kernel version
- `cat /proc/partitions` - prints the disk partitions
- `cat /proc/mounts` - prints the mounted filesystems
- `cat /proc/sys/kernel/hostname` - prints the hostname
- `cat /proc/sys/kernel/osrelease` - prints the kernel release
- `cat /proc/sys/kernel/version` - prints the kernel version
- `cat /proc/sys/kernel/ostype` - prints the operating system type
- `cat /proc/sys/kernel/threads-max` - prints the maximum number of threads
- `cat /proc/sys/kernel/shmmax` - prints the maximum shared memory segment size
- `cat /proc/sys/kernel/shmall` - prints the total amount of shared memory
- `cat /proc/sys/kernel/sem` - prints the semaphore values
- `cat /proc/sys/kernel/random/entropy_avail` - prints the available entropy
- `cat /proc/sys/kernel/random/poolsize` - prints the entropy pool size
- `cat /proc/sys/kernel/random/uuid` - prints a random UUID
- `cat /proc/sys/kernel/random/boot_id` - prints the boot ID

# see `/etc` for system configurations and informations

- `cd /etc && ls`
- `ls /etc`
- `cat /etc/os-release` - similar to `lsb_release`

## Other helpful CLI tools (requires installation)

- `neofetch` - prints the system information and an ASCII logo
- `btop` - an interactive CLI system monitor (best)
- `hwinfo` - prints the hardware information
- `dmidecode` - prints the DMI (Desktop Management Interface) information
- `lshw` - prints the hardware information
- `htop` - top-like interactive system-monitor
- `top` - OG system monitor

# How to see current IP Address on Linux?

- `ifconfig` - old way
- `ip addr show` or `ip a` - newer way
- `ip addr show <interface>` - print all information on a specific network interface

# How to check for free disk space?

- `df -h` - print free disk space in human-readable format
- `df -hT` - same as `df -h` but also prints the filesystem of the disks/partitions

# How to see if a Linux service is running?

- `service <service> status` - on older systems
- `systemctl status <service>` - checks the status of a service (newer systems systemd - most common)
- `sudo systemctl start <service>` - starts the service now (does not persist through reboot)
- `sudo systemctl enable <service>` - starts the service on boot automatically (on startup)
- `sudo systemctl stop <service>` - stops the service now (does not persist through reboot)
- `sudo systemctl disable <service>` - disables the service on boot (on startup)

# How to check the size of a directory in Linux?

- `du -sh <directory>` - prints disk usage of a (s)ingle directory in a (h)uman-readable format
- `du -h <directory-path>` - prints the disk usage of directories recursively in a (h)uman-readable format
- `du -hx --max-depth=4 <directory-path> --exclude="<pattern>" | sort -h | tail -n 10` - prints the top ten largest directories in a given directory-path
  - the `-x` skips directories in a different filesystem type

# How to check for open ports in Linux?

- `nmap <ip-or-hostname>`
- `netstat -a` or `netstat --all` - lists all ports
- `netstat -tupln` - list (l)istening, (t)cp, (u)dp, - by displaying (p)rogram names and - (n)ot resolve IP to hostnames
- `sudo netstat -tupln` - using `sudo` will allow to see PID/(p)rogram name
  - allows to see what services/programs are running on each ports listed
- **INFO:** `0.0.0.0:<port>` are public, `127.0.0.1` or `127.0.1.1` are private (both are loopback addresses)
- `lsof -nP -iTCP -sTCP:LISTEN` - display a list of ports in use
- `lsof -nP -i:<port-number>` - check a specific port number

# How to see services running behind the ports?

- `netstat -tulpn | grep -n <port>` - to find the service running behind the specific port, -n is to show the line numbers
- `sudo netstat -tulpn | grep -n <port>` - use sudo if not all processes/services could be identified
- `sudo netstat -anputW` - another way to find (all, numeric, ports, udp, tcp, wide):

# How to check for Linux process information (CPU usage, RAM usage, etc.)?

- `ps aux | grep <process>`
- `top`, `htop`, or `btop` - `btop` is more modern, `top` is installed by default
- `free -h` - prints RAM usage
- more information --> see [above](wizardry/linux/get-system-info#How to get the whole System Information in a Linux Machine)

# How to deal with mounts in Linux?

- `sudo mount /dev/mapper/<sdX> /mnt/<path>` - mount a device (like USB, HDD, External Hard drives, etc.)
- `sudo umount /mnt/<path>` - unmount
- `mount` - check for existing mounts
  - see [How to Allow Non-root Users to Mount and Unmount Devices without Sudo via `udev` rules ❯ How to Allow Non-root Users to Mount and Unmount Devices without Sudo via `udev` rules](wizardry/linux/how-to-allow-mount-unmount-without-sudo-via-udev.md#how-to-allow-non-root-users-to-mount-and-unmount-devices-without-sudo-via-udev-rules)
  - see [How to Mount Device at Boot via fstab](wizardry/linux/how-to-mount-device-at-boot-via-fstab)
- `sudo fdisk -l`
  - see [How to deal with Partitions](wizardry/linux/encrypt-hard-drive-partition#Encrypting Hard Drive Partition with LUKS)

# How to look something up?

- `man <command>` - classic man pages (manual pages)
- `tldr <command>` - more modern version of man pages, focusing on practical examples rather than a full detailed
  guide/manual
- search engine|google|stack overflow|blogs|articles|reddit|github|even AI - other ways to look up on something you don't know (everything is on the internet or web)

# Linux Performance Observability and Security Tools

- `strace` - system call tracer
- `iotop` - I/O monitor
- `ruptime` - uptime of remote machines

## Security focused

- `selinux` - security-enhanced Linux
- `auditctl` - a utility to assist controlling the kernel's audit system
- `rsyslog` - system logging
- `nftables` - firewall, NAT, and packet filtering (successor of `iptables`)
- `fail2ban` - intrusion prevention software framework
- `apparmor` - Mandatory Access Control (MAC) system
- `osquery` - SQL-based operating system instrumentation, monitoring, and analytics

# See more

- [security-hacking-wizardry](./wizardry/linux)
