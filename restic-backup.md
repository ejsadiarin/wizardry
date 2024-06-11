---
tags:
  - Wizardry
date: 2024-06-10T16:36
---
<!-- 2024-06-10 (June 10, 2024 4:36 PM Monday) -->

#  restic
- autorestic - CLI wrapper for restic (yaml files ez)

> Both are fine choices for what you're trying to do. I'd suggest trying both with some smaller backup sets and see which one makes more sense to you.
  I went back and forth between borg and restic, finally settled on [autorestic](https://github.com/cupcakearmy/autorestic) with 1 sftp and 2 s3-compatible backends.
  
- backrest - web ui with native rclone support

> I was in the same boat as you and decided to use [backrest](https://github.com/garethgeorge/backrest). They have an alpine docker image that comes with rclone, so it was really easy to setup for a use case like yours. 
  I’d recommend storing your rclone config with the backrest config, which you can then define in a docker compose file for easy reproducibility

From this https://www.reddit.com/r/selfhosted/s/fCFRbbQTbJ 