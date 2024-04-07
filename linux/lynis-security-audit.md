---
tags:
  - System and Security Auditing
date: 2024-03-10T10:45
title: Lynix for system and security auditing
---
<!-- 2024-03-10-1045 (March 10, 2024 10:45 AM) -->

# Lynis
- System and security auditing tool.

## Basic Usage 
```bash
# Check that Lynis is up-to-date:
sudo lynis update info

# Run a security audit of the system:
sudo lynis audit system

# Run a security audit of a Dockerfile:
sudo lynis audit dockerfile path/to/dockerfile
```

# References:
- [Lynis](https://cisofy.com/documentation/lynis/)
