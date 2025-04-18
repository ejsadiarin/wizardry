---
tags:
    - How-To
    - Laptop
date: 2025-01-27T18:34
---
<!-- 2025-01-27 (January 27, 2025 6:34 PM Monday) -->

# Ultimate Gaming Laptop Battery Guide

- for context: I have a Lenovo LOQ 15IRX9 i7-14700HX 4060 (w/ iGPU)

## Application-specific

* Firefox ([ref](https://www.youtube.com/watch?v=YE431SYO2Is))
    - Hardware Acceleration (use GPU instead of CPU for video decoding, etc.)
    - go to `about:support` -> find (ctrl + f): `Compositing`
        > [!NOTE]
        > if `Compositing` is `WebRender`, then GOOD (you use GPU).
        > if `Compositing` is `WebRender Software` or something, then BAD (you use CPU).
    - now find: `HARDWARE_VIDEO_DECODING` -> make sure it is `default: available`
        - if not, go to `about:config` -> search `media.hardware-video-decoding.enabled` -> set to `true`
        - may need to install `vaapi`
            > [!NOTE]
            > if you use NVIDIA proprietary drivers, it is already included/supported
            >
            > - but it won't hurt to install it:
            >
            >   on fedora:
            >     ```bash
            >     sudo dnf install nvidia-vaapi-driver libva-utils vdpauinfo
            >     ```
    - enable: `gfx.webrender.all` and `media.ffmpeg.vaapi.enabled` ([ref](https://www.reddit.com/r/linux/comments/1fbbkly/comment/lm4f0b0/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button))
        - to test if it is working:
            - install `intel-gpu-tools` then run `intel_gpu_top`
            - check if `Video` % is non-zero, if non-zero then it is working properly

    > What GPU do you have? If it's Intel or AMD, run vainfo in the terminal.
    > If it runs successfully, open Firefox and go to `about:config` and search for `vaapi`.
    > Enable the `ffmpeg` vaapi option. Now, search for `gfx.webrender.all` and enable it.
    > Restart Firefox and try watching a YouTube video.
    > Install `intel-gpu-tools` and run `intel_gpu_top` on Intel GPUs and check if the Video utilisation is non zero.
    > If it is non zero, then it is working properly 

* YouTube
    - turn off ambient mode (click a video -> video settings)

* TLP
    - using TLP-UI,

## Windows

ref: [https://www.reddit.com/r/GamingLaptops/comments/1ib2842/how_do_i_enhance_the_juice_of_my_laptop/?share_id=Qjb3XUewLJ-3ROiFUf4I8&utm_medium=android_app&utm_name=androidcss&utm_source=share&utm_term=5](https://www.reddit.com/r/GamingLaptops/comments/1ib2842/how_do_i_enhance_the_juice_of_my_laptop/?share_id=Qjb3XUewLJ-3ROiFUf4I8&utm_medium=android_app&utm_name=androidcss&utm_source=share&utm_term=5)

- uninstall Vantage and install Lenovo Toolkit
  - In Toolkit:
    - automate actions - so everything of these configs are switched on demand
  
- set minimum CPU power settings in Windows -> `advanced power options`:
  - minimum to 2%
  - maximum to 99% (this also disabled the core clock boost and lowered temps dramatically)

- select maximum battery life in Intel Graphics Settings
  - as well as most things in Windows -> `advanced power options`:

- 60 hz refresh on screen

- TRIPLE CHECK if dGPU (in my case, the `RTX 4060`) is off

- hybrid iGPU-only mode in Toolkit
  - Sometimes toggling between hybrid-igpu and hybrid auto a few times seems to reset it. Just setting it to Optimus doesn't do the deed

- set brightness to 25%

- battery saver mode

• keyboard backlight off

- port lights off

- Bluetooth off

- uninstalled bloatware

- adjusted settings so nothing auxiliary automatically boots on startup (things like steam that like to sneakily start, disabled through task manager)

- quiet mode

- closed all extra applications and tabs

- HDR off

- Voltage capped at 1.4v

> Doing all this I can get down to 9-12w usage surfing the web. If it’s anything above 16 the dgpu is still getting polled, even if everything says it isn’t. Reset it and kill apps using it through toolkit a few times and it’ll drop. Most the other things are fairly small gains, the dgpu will double power consumption. Anyone who says they can't get more than 5 hours with a new battery isn't trying hard enough

## OS-Agnostic

- undervolt CPU (intel HX)
  - BIOS -> Configuration -> CPU UnderVolt Protected to `Disabled` (this is `Enabled` by default) -> Save and Exit then Reboot
  - then use Throttlestop (Windows) / XTU (Windows) / cpupower cpufrequtils (Linux) for undervolting

#### `cpufrequtils` guide

ref: [https://askubuntu.com/a/1120099](https://askubuntu.com/a/1120099)
> I came across the same problem and I found a solution which works for me.  
> You'll have to download **cpufrequtils**.  
>
> Run every command in Terminal: Note: The '-c' argument is for core number. If your CPU has four cores, run the given command for 0 through 3 and if your CPU has eight cores, then run the command for 0 through 7.

```bash
sudo cpufreq-set -c 0 -g performance
sudo cpufreq-set -c 1 -g performance
sudo cpufreq-set -c 2 -g performance
sudo cpufreq-set -c 3 -g performance
sudo cpufreq-set -c XX -g performance
sudo modprobe msr
sudo rdmsr 0x1FC
```

The **XX** here is the number of your core. After this step you'll get an output which you need to **note down** and then use in the next command.

```bash
sudo wrmsr 0x1FC XXXXX
```

Here, **XXXXX** is the output from the previous command execution.  
Finally, to check if it has worked, run:

```bash
cpufreq-info
```

## Linux

- install `auto-cpufreq` and `thermald` (for intel CPU)
    - `auto-cpufreq` - replaces tuned and/or power-profiles-daemon
        > [!NOTE]
        > **TEST if `auto-cpufreq` actually helps with watt usage (at idle, medium load, full load)**

- NVIDIA:
    - install 
