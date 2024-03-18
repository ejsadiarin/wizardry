---
title: How to get the whole System Information in a Linux Machine
date: 2024-03-13-1923 (March 13, 2024 7:23 PM)
tags: 
- System Information
- Linux
---

# How to get the whole System Information in a Linux Machine
- `inxi -F` - prints the detailed system information (complete)

- `uname` command:
  - `uname -a` - prints all the system information
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
- `uptime` - prints the system uptime and load averages

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

## Other helpful CLI tools (requires installation)
- `neofetch` - prints the system information and an ASCII logo
- `btop` - an interactive CLI system monitor (best)
- `hwinfo` - prints the hardware information
- `dmidecode` - prints the DMI (Desktop Management Interface) information
- `lshw` - prints the hardware information
- `htop` - top-like interactive system-monitor
- `top` - OG system monitor

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
