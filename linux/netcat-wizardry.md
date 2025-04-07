---
date: 2025-04-07T15:41
title: Netcat (`nc`) Wizardry
tags: 
    - Linux
    - Wizardry
---
<!-- 2025-04-07-1541 (April 07, 2025 03:41:02 PM) -->

# Netcat (`nc`) Wizardry

- `nc` (netcat) simultaneously reads from `stdin` and writes to `stdout`, sending anything read from `stdin` out to the network and printing anything received from `stdout`.

## Sending and Receiving Files

- on Receiving side:
```bash
# normal use with zip (-q 1 means seconds to wait after EOF on stdin)
nc -l -p 12345 -q 1 > something.zip < /dev/null
nc -l -p 12345 -q 1 > something.zip

# use tar to preserve file permissions, ownership, and timestamps
nc -l -p 12345 -q 1 | tar xz -C </path/here>

# for multi-gig large files (-q -1 means to wait forever after EOF on stdin)
nc -l -p 12345 -q -1 > something.zip
```

- on Sending side:
```bash
cat something.zip | nc <server-ip-of-receiving> 12345

tar czf - <./directory/to/transfer> | nc <server-ip-of-receiving> 12345
```

---

## Other Tricks

- [SSH with netcat/nc](./ssh-nc.md)
