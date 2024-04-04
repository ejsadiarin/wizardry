---
id: generate-numbered-list
aliases: []
tags:
  - Vim / Neovim
date: 2024-02-17-1021 (February 17, 2024, 10:21 AM)
title: Generate Numbered List in Vim/Neovim
---
# Generate Numbered List in Vim/Neovim
1. Enter `VISUAL BLOCK` mode by pressing `Ctrl + V` 
  - or use `vim-multi-line`: mine: `ctrl + alt` {k,j}
2. Select lines and add "1." (use `Shift + i` when in visual block mode)
  - if `vim-multi-line`: just `i` if vim-multi-line
3. Exit insert mode and do `gv` to select last highlight
  - if `vim-multi-line`: idk manually visual select them till last line
4. `o` and `j` to exclude the first line
5. `g + <Ctrl+A>` to increment
