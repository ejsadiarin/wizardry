---
date: 2025-03-11T03:31
title: How to See and Kill Process Running on a Port in Linux
tags: 
    - How-To
---
<!-- 2025-03-11-0331 (March 11, 2025 03:31:12 AM) -->

# How to See and Kill Process Running on a Port in Linux

- kill process running on `:port`

    - `kill -9` - `-9` is the strongest kill signal, which forces to kill the process
    - `lsof` - lists open files
        > [!NOTE]
        > In Linux, *everything* is a file.
    - `-t` - only outputs PIDs
    - `-i :port` - finds process that opened the local `:port`

```bash
kill -9 $(lsof -ti :port)

# example, kill syncthing running on port 8384
kill -9 $(lsof -ti :8384)
```

- see process running on a `:port`
```bash
lsof -i :port
```

- see PIDs of the process running on a `:port`
```bash
lsof -ti :port
```
