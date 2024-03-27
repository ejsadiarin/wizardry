---
title: Multi Cursors in Vim/Neovim
date: 2024-03-24-2150 (March 24, 2024, 9:50 PM)
tags: 
- Multi Cursors
---

# Multi Cursors in Vim/Neovim
- search and replace, whole file given a string (not strict word):
```vim
:%s/old/new/g
```

- if strict word, use regex:
```vim
:%s/\<word\>/new/g
" or
:%s/\<word\>/new
```

- search and replace, for few words (ideal for < 10 words to replace):
```vim
" select word under cursor
*
cgn
" use `.` to repeat
.
" use n and N to navigate
```

# Using Plugins
- vim-visual-multi
- nvim-spectre (search and replace)
