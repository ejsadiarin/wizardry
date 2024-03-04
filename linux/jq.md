---
title: jq - Command-line JSON magic
date: 2024-02-09-1029 (February 9, 2024 10:29 AM)
tags:
- CLI
---
# jq 
- text manipulation tool for JSON data
- perfect for filtering and transforming 
- great with pipes

```bash
# transform data to json format (can be used with minified json)
jq < file # or jq < file.json

# compress data (minify)
jq -c < file # or jq -c < file.json

# keys (.data) notation to filter data
jq '.id' < file
# if the json starts with [], all data at index 0
jq '.[].id' < todos  # select all ids from todos
jq -r '.[].id' < todos  # -r removes quotes

# using select to filter data
jq '.[] | select(.age > 30)' < file
jq '.[] | select(.id < 10 and .completed == false)' < todos
jq 'select(.values.a + values.b + values.c > 100)' < file
jq 'select(.foo | length > 5)' < file
jq 'select(.logs | length > 0)' < file # output entries that do not have null or empty logs

# using map to transform data

# using with cat
jq < todos.json | cat >> todos # appends output to new file 'todos'
cat file | jq .id # prints id from cat stdout
```

# Using with Vim/Neovim
- visual-select line and colon(`:`): `:'<,'>!jq` 
- `:%!jq` 
- `:%!jq 'select(.values.a + values.b + values.c > 100)'`

# References
- [jq](https://stedolan.github.io/jq/) is a lightweight and flexible command-line JSON processor.
- [jq Manual](https://stedolan.github.io/jq/manual/)
