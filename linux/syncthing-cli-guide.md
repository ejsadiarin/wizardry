---
date: 2025-01-23T03:14
title: Syncthing CLI Guide
tags:
    - Wizardry
    - How-To
---
<!-- 2025-01-23-0314 (January 23, 2025 03:14:57 AM) -->

# Syncthing CLI Guide

ref: [https://gist.github.com/Jonny-exe/9bad76c3adc6e916434005755ea70389](https://gist.github.com/Jonny-exe/9bad76c3adc6e916434005755ea70389)

## Step-by-step (copied from [ref](https://gist.github.com/Jonny-exe/9bad76c3adc6e916434005755ea70389))
This is useful for example on a headless Raspberry Pi without proxying web-traffic through SSH or with port-forwarding limitations.
In this example we will want to share the default folder from Machine A with Machine B
<table>
<tr>
<th> Machine A </th>
<th> Machine B </th>
</tr>
<tr>
<td>
  
  **Install Syncthing**
  
  On debian: `sudo apt install syncthing`

  On fedora: `sudo dnf install syncthing`
  
  **Generate device ID and config files and default folder**

  `syncthing generate`
  
  **Start Syncthing**

  `syncthing --no-browser`
  
  **Get the device id**

  `syncthing --device-id`
</td>
<td>
  
  **Install Syncthing**
  
  On debian: `sudo apt install syncthing`

  On fedora: `sudo dnf install syncthing`
  
  **Generate device ID and config files and default folder**

  `syncthing generate`
  
  **Start Syncthing**

  `syncthing --no-browser`
  
  **Get the device id**

  `syncthing --device-id`
</td>
</tr>
<tr>
<td>

  **Add Machine B**

  `syncthing cli config devices add --device-id $DEVICE_ID_B`
</td>
<td>
  
  **Add Machine A**
  
  `syncthing cli config devices add --device-id $DEVICE_ID_A`

</td>
</tr>
<tr>
<td>
  
  **Share default folder with Machine B**

  `syncthing cli config folders $FOLDER_ID devices add --device-id $DEVICE_ID_B`

  _Insert the id from the folder you want to share into $FOLDER_ID_
</td>
<td></td>
</tr>
<td>



</td>
<td>

 **Accept default folder from Machine A**
  `syncthing cli config devices $DEVICE_ID_A auto-accept-folders set true`

  _If you don't want Machine B automatically accepting all of Machine A's folder requests, just run this command again with `false`_

</td>
</tr>

</table>

Additional useful info can be found at: https://superuser.com/questions/1397683/how-can-i-configure-syncthing-from-command-line-to-share-a-folder-with-another-c/1731999#1731999?s=0744928a5f9d4717b7445d039785ba53.

Don't waste your time with this repo, as it hasn't been updated since 2014, it is not working: https://github.com/classicsc/syncthingmanager

This repo https://github.com/tenox7/stc is useful but it doesn't not offer adding devices/folders.
