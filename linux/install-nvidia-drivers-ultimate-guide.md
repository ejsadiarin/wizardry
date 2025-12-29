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

> [!NOTE]
> Secureboot must be enabled to use the NVIDIA drivers from RPMFusion (see below)

- refs:
    - [Howto/NVIDIA](https://rpmfusion.org/Howto/NVIDIA)
    - [Howto/SecureBoot](https://rpmfusion.org/Howto/Secure%20Boot)

---

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
# ^ should print/output a version (e.g. 565.77)
```
- this should output a version (e.g. `565.77`) and not `modinfo: ERROR: Module nvidia not found.` 

- also check if nouveau is disabled (not in use)
```bash
lsmod | grep nouveau
# should print/output nothing
```
### Install other drivers for Hardware Feature Support

- install NVENC/NVDEC (most likely already installed by the commands above, but won't hurt to check!)
```bash
sudo dnf install xorg-x11-drv-nvidia-cuda-libs
```

- VDPAU/VAAPI (video acceleration, video decoding support)
```bash
sudo dnf install nvidia-vaapi-driver libva-utils vdpauinfo
```
- to check if correctly installed (here is sample output):
    - do `vdpauinfo`
    ```bash
    $ vdpauinfo
    display: :0   screen: 0
    API version: 1
    Information string: NVIDIA VDPAU Driver Shared Library  565.77  Wed Nov 27 22:50:58 UTC 2024
    ...
    ```
    - then `vainfo`
    ```bash
    $ vainfo
    Trying display: wayland
    libva info: VA-API version 1.22.0
    libva info: Trying to open /usr/lib64/dri-nonfree/iHD_drv_video.so
    libva info: Found init function __vaDriverInit_1_22
    libva info: va_openDriver() returns 0
    vainfo: VA-API version: 1.22 (libva 2.22.0)
    vainfo: Driver version: Intel iHD driver for Intel(R) Gen Graphics - 24.4.4 ()
    vainfo: Supported profile and entrypoints
    ...

- install Vulkan (again, most likely installed but still)
```bash
sudo dnf install vulkan
```


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

## Switch between NVIDIA (proprietary drivers) and Nouveau

ref: [https://rpmfusion.org/Howto/NVIDIA](https://rpmfusion.org/Howto/NVIDIA)

### NVIDIA to Nouveau

* On boot (grub), press `e` on the OS you want to boot into to edit grub kernel parameters
    - remove: `"rd.driver.blacklist=nouveau modprobe.blacklist=nouveau nvidia-drm.modeset=1"`
    - then finally `ctrl + x` to boot

### Nouveau to NVIDIA

- if you have done the NVIDIA proprietary drivers installation above, then you will boot with the proprietary driver by default
- so just boot to the OS you want to boot into

- for checking:
    - choose OS then press `e` to edit grub kernel parameters
    - there should be: `"rd.driver.blacklist=nouveau modprobe.blacklist=nouveau nvidia-drm.modeset=1"`

> With recent drivers as packaged with RPM Fusion, it is possible to switch easily between nouveau and nvidia while keeping the nvidia driver installed.
> When you are about to select the kernel at the grub menu step.
> You can edit the kernel entry, find the linux boot command line and manually remove the following options "rd.driver.blacklist=nouveau modprobe.blacklist=nouveau nvidia-drm.modeset=1". This will allow you to boot using the nouveau driver instead of the nvidia binary driver. At this time, there is no way to make the switch at runtime. 

---

## Quickie

- After a BIOS update (let BIOS know about the Secureboot key)
```bash
sudo mokutil --import /etc/pki/akmods/certs/public_key.der 
```

- Reinstall all (if issues arise from kernel being too new to be supported
```bash
# 1. Remove everything NVIDIA
sudo dnf remove *nvidia*

# 2. Reinstall the core driver packages
sudo dnf install akmod-nvidia xorg-x11-drv-nvidia-cuda

# 3. CRITICAL WAIT
# Do not reboot yet. Wait 2-3 minutes.
# Watch the build process:
watch -n 1 "ps aux | grep akmods"
# Wait until the 'akmods' process disappears from the list.

# 4. Reboot
sudo reboot
```

