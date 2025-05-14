---
date: 2025-03-01T15:43
tags:
    - Wizardry
    - How-To
---
<!-- 2025-03-01-1543 (March 1, 2025 03:43:57 PM) -->

# Syncthing Wizardry

- fast, quick, and easy guide for linux-based machines

## Install

- Fedora:
```bash
sudo dnf install syncthing
```

- start and enable:
```bash
sudo systemctl enable --now syncthing@$USER.service
```

- check status:
```bash
sudo systemctl status syncthing@$USER.service
```
- check `localhost:8384` for GUI

## CLI

- see [Syncthing CLI Guide](../linux/syncthing-cli-guide.md)
