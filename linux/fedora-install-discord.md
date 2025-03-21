---
date: 2025-03-21T18:50
title: How to install Discord in Fedora
tags: 
    - Linux
---
<!-- 2025-03-21-1850 (March 21, 2025 06:50:06 PM) -->

# How to install Discord in Fedora

## Non-Flatpak Way (via dnf non-free repo)

1. Add RPM-fusion non-free repository
```bash
sudo dnf install https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

2. update system (optional)
```bash
sudo dnf update
```

3. install Discord
```bash
sudo dnf install discord
```
