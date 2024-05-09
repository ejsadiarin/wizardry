---
tags:
  - Troubleshooting
date: 2024-05-04T16:19
---
<!-- 2024-05-04-1619 (May 04, 2024 04:19:16 PM) -->

# How to Troubleshoot the classic "No Internet" IT Problem

- [ ] *Check the scope of the issue first*: Is it "No internet at all", or limited only to specific devices/applications

- [ ] *Check Physical Connections*: Are all cables (Ethernet, modem, router) properly connected, and no damages or loose cables? 

- [ ] *Restart Network Devices*: Try to restart the devices (modem, router, and other networking devices), since issue might just be because of temporary software issues or glitches
- [ ] *Check the status light*: Check if the status light indicator is on "red" or "green"

- [ ] *Test Connectivity on Other Devices*: If scope is "No internet at all" then it affects other devices on the same network

- [ ] *Troubleshoot Network Settings*: On the affected device, check network settings such as IP configuration, DNS settings, and proxy settings. Check if the device is set to obtain an IP address automatically (DHCP) and that DNS servers are configured correctly.

- [ ] *Flush DNS Cache*: Sometimes, **DNS cache corruption** can cause internet connectivity issues. Flushing the DNS cache can resolve this problem. On Windows, this can be done using the command `ipconfig /flushdns` in Command Prompt. On macOS and Linux, you can use the `sudo killall -HUP mDNSResponder` command.

- [ ] *Disable and Re-enable Network Adapter*: Temporarily disable and then re-enable the network adapter on the affected device. This action can refresh network settings and establish a new connection with the network.

- [ ] *Run Network Troubleshooters*: Many operating systems offer built-in network troubleshooters that can diagnose and fix common connectivity issues automatically. Run the appropriate network troubleshooter for the user's operating system.

- [ ] *Check for Firmware Updates*: Check if the firmware (software) on the modem and router is up to date. Manufacturers often release updates to address known issues and improve compatibility with internet service providers.

- [ ] *Contact Internet Service Provider (ISP)*: If none of the above steps resolve the issue, it's possible that the problem lies with the internet service itself. Contact the ISP's support line to check for any known outages or issues in the area.
