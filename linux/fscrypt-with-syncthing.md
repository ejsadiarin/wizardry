---
id: fscrypt-with-syncthing
aliases: []
tags:
  - Encryption
  - Syncthing
  - Linux
  - How-To
date: 2024-02-10T15:18
title: fscrypt with Syncthing
---
<!-- 2024-02-10-1518 (February 10, 2024 3:18 PM) -->

# Using fscrypt with Syncthing
1. since fscrypt encrypts directories when locked (obsfucates the file names and contents), you want to sync when it is "unlocked"
```bash
fscrypt unlock <directory-you-want-to-sync>
```
2. then sync with Syncthing normally via web GUI
3. after syncing, lock the directory again:
  ```bash
  fscrypt lock <directory>
  # if error do (NOTE: read the error message on how to kill the process using the directory):
  find "<directory_name>" -print0 | xargs -0 fuser -k
  ```
  - this will cause Syncthing to warn you and encrypted directory will be "Stopped"
  - no syncing will happen when the directory is locked
  - **TIP: you could also PAUSE the syncing of the directory in the web GUI and just resume when it is time to sync again**
4. when it is time to sync again:
  - unlock the directory
  ```bash
  fscrypt unlock <directory-you-want-to-sync>
  ```
  - then just open the web GUI and rescan (or resume if you paused it) 
  - everything will synced again
