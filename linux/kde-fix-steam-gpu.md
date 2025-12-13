---
date: 2025-12-13T15:11
title: KDE - Fix Steam GPU
tags: 
    - How-to
---
<!-- 2025-12-13-1511 (December 13, 2025 03:11:34 PM) -->

# KDE - Fix Steam GPU

Problem: 
- For Laptops with integrated & discrete GPU, Steam prefers to look for the default "secondary" GPU 
- Happens when opening steam - UI doesn't open but steam is running in the background 

Solution:

1. Kill `steam` and `steamwebhelper`
```bash
pkill -9 steam
pkill -9 steamwebhelper
```

2. Open steam desktop entry in `/usr/share/applications/steam.desktop`

3. Change setting `PrefersNonDefaultGPU` from `true` to `false`:
    ```desktop
    # from this
    PrefersNonDefaultGPU=true

    # to this
    PrefersNonDefaultGPU=false
    ```

4. Open steam again 

### Diagnosis if above solution didn't work

- Kill `steam` and `steamwebhelper`
- Check for steam logs by opening `steam` via terminal (and checking its output logs)
    1. If there is "Mesa" or "libGL" error, then downgrade mesa 
    ```bash
    sudo dnf downgrade 'mesa*'
    ```
    2. If there is a steamwebhelper error, then there are hardware acceleration issues
    ```bash
    steam -no-browser +open steam://open/minigameslist
    ```
    - Then if this opens a small list window successfully, go to Settings > Interface and disable "Enable GPU accelerated rendering in web views", then restart normally. 

### If none works

Just use the flatpak version of Steam

```bash
# uninstall RPM version of steam first
sudo dnf remove steam

# install steam via flatpak
flatpak install flathub com.valvesoftware.Steam
```
