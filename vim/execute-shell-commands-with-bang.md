---
date: 2025-03-10T23:47
title: Execute Shell Commands with ! inside buffers
tags: 
    - Wizardry
---
<!-- 2025-03-10-2347 (March 10, 2025 11:47:39 PM) -->

# Execute Shell Commands with ! inside buffers

> [!NOTE]
> if the external command has an output, it replaces the buffer/visual selected lines with it

- `git status` on a whim
```bash
:!git status
```

- stage and commit the current file with `git`
```bash
:!git add % && git commit -m "chore: minor update"
```

- have a `curl` inside? execute it and replace it with the output
```bash
# visual select the curl line
:'<,'>!bash
```

- want to format json? use `jq`
```bash
# visual select the unformatted code
:'<,'>!jq
```

- want to `grep` and replace buffer with its output?
```bash
%!grep word
```

- perform `strace` inside `nvim` and output its contents in the buffer
```bash
:%!strace program
```

# Other Wizardry

- pipe `strace` to nvim and grep contents
```bash
strace program | nvim -
# then inside neovim
:%!grep
```
