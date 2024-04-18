---
tags:
  - Linux
date: 2024-04-18T07:57
---
<!-- 2024-04-18-0757 (April 18, 2024 07:57:06 AM) -->

# How to Allow Non-root Users to Mount and Unmount Devices without Sudo via `udev` rules
- If you want to allow a non-root user to mount and unmount devices without sudo, you can use the udev rules. Here's a basic example of how to create
 a udev rule that allows a user to mount and unmount a specific device: 

1. Create a new file in `/etc/udev/rules.d/`, for example, `9-usb.rules`
2. Add a rule to the file that grants the user permission to the device. 
  - Replace username with the actual username and `/dev/<sdX>` with the device you want to allow access to:
  ```udev
  KERNEL=="sdX", SUBSYSTEM=="block", OWNER="username", GROUP="username", MODE="
  060"
  ```
3. Reload udev rules:
```bash
sudo udevadm control --reload-rules
```
4. The user should now be able to mount and unmount the device without `sudo`.
