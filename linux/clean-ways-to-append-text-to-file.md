---
title: Clean ways to append text to a file
date: 2024-02-06-1210 (February 6, 2024 12:10 PM)
tags:
- Linux
description:
reference: https://stackoverflow.com/questions/17701989/how-do-i-append-text-to-a-file
---

# Appending text to a file (no text editor, perfect for scripts)
- `>>` - for appending text to a file
- `>` - for overwriting a file

- for one-liners (useful for remapping Esc in .vimrc)
```bash
echo "inoremap kj <Esc>" >> .vimrc # or ~/.vimrc to ensure consistency
```

- using cat with CTRL+D
> Essentially, you can dump any text you want into the file. 
CTRL-D sends an end-of-file signal, which terminates input and returns you to the shell.
```bash
cat >> filename
This is text, perhaps pasted in from some other source.
Or else entered at the keyboard, doesn't matter. 
^D
```

- using `cat` with `EOF` (heredoc)
```bash
cat << EOF >> filename
This is text entered via the keyboard or via a script.
EOF
```
- if needing sudo
```bash
sudo sh -c 'cat << EOF >> filename
This is text entered via the keyboard.
EOF'
```
- or using `tee` to avoid sudo issue when using heredoc with `cat`
```bash
tee -a filename << EOF
This is text entered via the keyboard or via a script.
EOF
```

- using `echo` with `tee`
  - `>/dev/null` - to suppress the output of the command in terminal
```bash
echo "text" | tee -a filename >/dev/null
# or if needing sudo
echo "text" | sudo tee -a filename >/dev/null
```

### See more from the reference link
- [https://stackoverflow.com/questions/17701989/how-do-i-append-text-to-a-file](https://stackoverflow.com/questions/17701989/how-do-i-append-text-to-a-file)
