---
date: 2025-07-20T14:28
title: Regex to do in zhistory (zsh history)
tags: 
    - Vim
---
<!-- 2025-07-20-1428 (July 20, 2025 02:28:57 PM) -->

# Regex to do in zhistory (zsh history)

- quickie:

```vim
%s/^: [0-9]\+:[0-9]\+;/
```

or `:%s/^: [0-9]\+:[0-9]\+;.*\n\?//`

- `^: ` — line starts with `: `
- `[0-9]\+` — one or more digits (timestamp)
- `:` — literal colon
- `[0-9]\+` — one or more digits (duration)
- `;.*` — semicolon and the rest of the command
- `\n\?` — (optional) newline, to remove the whole line

## Problem Statement (Reasoning)

- to remove lines like:

```txt
: 1747240263:0;ls
: 1747240267:0;cd .ssh
: 1747240267:0;ls
```


Those lines in your zsh history are **extended history** entries.

```txt
: <epoch_time>:<duration>;command
```

- `1747240263` is the Unix timestamp when the command started.
- `0` is the duration in seconds.
- `ls` is the command.

This is normal if you have `setopt EXTENDED_HISTORY` in your `.zshrc`.

