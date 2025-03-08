---
tags:
  - How-To
  - Neovim
date: 2025-03-08T15:20
title: Man Page in Neovim with gO Hack
---
<!-- 2025-03-08 (2025-03-08 3:20:51 PM) -->

# Man Page in Neovim with gO Hack

- in `~/.bashrc` or `~/.zshrc`:
```bash
export MANPAGER="nvim +Man!"
```

- then try to man any command:
```bash
man rsync
```

- use `gO` in nvim to navigate to the "outline" (table of contents) of the page for easy navigation

## For testing this out

- in cli do:
```bash
MANPAGER='nvim +Man!' man man
```
