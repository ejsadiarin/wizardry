---
date: 2025-01-22T15:52
tags:
  - How-To
  - Linux
---

<!-- 2025-01-22-1552 (January 22, 2025 15:52:59) -->

# Ultimate Nvidia Guide to Install Drivers and more

> [!IMPORTANT]
> **THIS GUIDE IS ONLY FOR FEDORA 41+ users**

- refs:
    - [Howto/NVIDIA](https://rpmfusion.org/Howto/NVIDIA)
    - [Howto/SecureBoot](https://rpmfusion.org/Howto/Secure%20Boot)

## Drivers Installation

1. Enable free and non-free repositories ([see rpm Configuration](https://rpmfusion.org/Configuration))
    - > [!NOTE]
    > no need to do this step if you already enabled non-free repositories in the fedora installation step (GUI)

- via dnf
```bash
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```
* On Fedora, we default to use the openh264 library, so you need the repository to be explicitly enabled.
    - On Fedora 41 and later:
    ```bash
    sudo dnf config-manager setopt fedora-cisco-openh264.enabled=1
    ```

    - On Fedora up to 40, the command is as follow:
    ```bash
    sudo dnf config-manager --enable fedora-cisco-openh264
    ```

2. Enable and Configure Secure Boot

- this step is copied directly from [Howto/SecureBoot](https://rpmfusion.org/Howto/Secure%20Boot)
    - see first the documentation above to see if it is updated/new ways to configure secure boot

* Install the following tools:

    ```bash
    sudo dnf install kmodtool akmods mokutil openssl 
    ```

* The steps are described below. Refer to `/usr/share/doc/akmods/README.secureboot` for more information.

* To generate a key with the default values:

    ```bash
    sudo kmodgenca -a 
    ```

* Now you need to enroll the public key in MOK, enroll the new keypair with certificate with the command

    ```bash
    sudo mokutil --import /etc/pki/akmods/certs/public_key.der 
    ```

* Mokutil asks to generate a password to enroll the public key. You will need this soon.

* Rebooting the system is needed for MOK to enroll the new public key.

    ```bash
    systemctl reboot 
    ```

* On the next boot MOK Management is launched and you have to choose "Enroll MOK"
 
* Choose "Continue" to enroll the key or "View key 0" to show the keys already enrolled.
 
* Confirm enrollment by selecting "Yes".

* You will be invited to enter the password generated above.

    WARNING: keyboard is mapped to QWERTY!

* The new key is enrolled, and the system asks you to reboot. 

3. Determine your Card Model

```bash
lspci | grep -e VGA

# if previous command has no output (Optimus is probably enabled):
lspci | grep -e 3D
```
- You can also check the [supported chips](https://download.nvidia.com/XFree86/Linux-x86_64/560.35.03/README/supportedchips.html) section and see which series is recommended for you card, then install the appropriate driver series. Please remember that you need additional steps for optimus. 

4. Installing the drivers

> [!IMPORTANT]
> make sure that secure boot is already enabled and configured (see step 2) 

- update and reboot (if not on latest kernel)
```bash
# update repositories
sudo dnf update -y 

# reboot (optional, only reboot if you are not on the latest kernel)
systemctl reboot
```

- install necessary packages
```bash
sudo dnf install akmod-nvidia # rhel/centos users can use kmod-nvidia instead
sudo dnf install xorg-x11-drv-nvidia-cuda #optional for cuda/nvdec/nvenc support  
```

- after installation, reboot
```bash
systemctl reboot
```

5. Once the module is built, check the version
```bash
modinfo -F version nvidia
```
- this should output a version (e.g. `565.77`) and not `modinfo: ERROR: Module nvidia not found.` 

6. READ the [Special Notes section in Howto/NVIDIA](https://rpmfusion.org/Howto/NVIDIA)


## Using the dGPU for graphical applications

- ref [PRIME Render Offload section in Howto/Optimus](https://rpmfusion.org/Howto/Optimus) 

> [!IMPORTANT]
> If the graphical application uses Vulkan, `__NV_PRIME_RENDER_OFFLOAD=1` is enough
> If it uses GLX, add __GLX_VENDOR_LIBRARY_NAME=nvidia

> as per documentation: Configure Graphics Applications to Render Using the GPU Screen
> 
> To configure a graphics application to be offloaded to the NVIDIA GPU screen, set the environment variable NV_PRIME_RENDER_OFFLOAD to 1.
> If the graphics application uses Vulkan, that should be all that is needed.
> If the graphics application uses GLX, then also set the environment variable GLX_VENDOR_LIBRARY_NAME to nvidia, so that GLVND loads the NVIDIA GLX driver.
> NVIDIA's EGL implementation does not yet support PRIME render offload. 

Examples:
```bash
__NV_PRIME_RENDER_OFFLOAD=1 vkcube
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia glxinfo | grep vendor
```

- for finer-grained control see documentation [PRIME Render Offload section in Howto/Optimus](https://rpmfusion.org/Howto/Optimus) 