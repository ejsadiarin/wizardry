---
date: 2026-02-16T18:26
title: awk and xargs
tags:
    - Linux
---

<!-- 2026-02-16-1826 (February 16, 2026 06:26:40 PM) -->

# awk and xargs

finally learned how to do awk and xargs properly (2026-02-16 18:27)

## awk

```bash
awk '[pattern/condition] {action}'
```

- example for printing fields based on pattern/condition

```bash
# print the ppid of the zombie processes in the system
ps -eo pid,ppid,stat,cmd | awk '$3 == "Z" {print $2}'
```

## xargs

```bash
xargs [optional flags] [command]

# example
xargs -I {} [command]
```

- example for executing based on list of items as input

```bash
# hard kill ppid of zombie processes
ps -eo pid,ppid,stat,cmd | awk '$3 == "Z" {print $2}' | xargs -I {} kill -9 {}
```

The `-I {}` is "argument" (`{}` or any placeholder) that represents each item in the list of input
