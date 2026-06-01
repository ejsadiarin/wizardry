---
date: 2025-04-06T16:30
title: How to Define Theme for Specific Applications in KDE Plasma (Fix theme for Packet Tracer)
tags: 
    - How-To
    - KDE
---
<!-- 2025-04-06-1630 (April 06, 2025 04:30:15 PM) -->

# How to Define Theme for Specific Applications in KDE Plasma (Fix theme for Packet Tracer)

- ref: [https://askubuntu.com/questions/1292633/how-to-define-a-theme-for-specific-application-in-kde-plasma](https://askubuntu.com/questions/1292633/how-to-define-a-theme-for-specific-application-in-kde-plasma)

## Problem

- Breeze-dark (when set as the global dark theme) for KDE Plasma causes some application themes to tweak out like Packet Tracer
- To mitigate this, we edit the environment variable `XDG_CURRENT_DESKTOP` (and 2 others if this doesn't work by itself) of the specific application on their startup

## Solution (example with packet tracer)

1. Locate the `*.desktop` file of the application (in this case packet tracer).
- In my case: `/usr/local/share/applications/cisco-pt821.desktop`

2. Choose one method you want below (for user changes or system changes)
- For user changes (changes only for the current user): Make a copy of it and paste it in `~/.local/share/applications`
- For system changes (needs to be root): edit `/usr/local/share/applications/cisco-pt821.desktop`

3. In the line where is `Exec=/opt/pt/packettracer %f`, edit line and add with `XDG_CURRENT_DESKTOP=GNOME` (see final changes below)

- final changes should look like this:
```desktop
[Desktop Entry]
Type=Application
Exec=XDG_CURRENT_DESKTOP=GNOME /opt/pt/packettracer %f
Name=Packet Tracer 8.2.2
Icon=/opt/pt/art/app.png
Terminal=false
StartupNotify=true
MimeType=application/x-pkt;application/x-pka;application/x-pkz;application/x-pks;application/x-pksz;
```

- this will set the environment variable before starting the application.

---

### If above solution doesn't work, try this

- add these 2 other `env` vars to the `Exec=...` line
```
QT_QPA_PLATFORMTHEME=lxqt
GTK_THEME=Default
```

- so final changes looks like this now:
```desktop
[Desktop Entry]
Type=Application
Exec=XDG_CURRENT_DESKTOP=GNOME QT_QPA_PLATFORMTHEME=lxqt GTK_THEME=Default /opt/pt/packettracer %f
Name=Packet Tracer 8.2.2
Icon=/opt/pt/art/app.png
Terminal=false
StartupNotify=true
MimeType=application/x-pkt;application/x-pka;application/x-pkz;application/x-pks;application/x-pksz;
```
