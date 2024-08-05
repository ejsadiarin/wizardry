---
date: 2024-08-05T04:15
tags:
  - How-To
  - Wizardry
---

<!-- 2024-08-05-0415 (August 05, 2024 04:15:48 AM) -->

# How to Debug LSP Servers in Linux

## Quick Checklist

- [ ] check if LSP package is installed (either via `mason`, package manager like `pacman`, downloaded, or built from source)
- [ ] locate binary path or launcher path (read the docs to see what you need)
- [ ] proper config setup via `mason-lspconfig` or something else like `ftplugin/java.lua` (for `jdtls`)
  - [ ] make sure `capabilities` property is configured somewhere
        --> something like `capability = require('cmp_nvim_lsp').default_capabilities()`
- [ ] proper `cmd` config _(IMPORTANT!)_ to start the LSP server
      --> this is the command to be executed to the command-line
      --> something like `gopls serve` or `java -jar ... -jvm-arg:... -data ...`

  ```lua
    cmd = {
        'gopls', 'serve'
    }
  ```

- [ ] READ THE DOCS `RTFM!`

## Explanations

- if LSP is attached but no diagnostics, code completions, docs, etc.
- if LSP is NOT attached even though it is already installed

**if using `mason` with `mason-lspconfig` (especially when using `neovim (nvim)`)**

- _IMPORTANT:_ `~/.local/share/nvim/mason/packages/` - where the configs of _mason-installed tools_ are located

- note that the binaries in `bin (or any)` is the one that is run to start the LSP (of course)
  - this is specified in `cmd` property in `mason-lspconfig`

`cmd` (table) --> Override the default command used to **start the server**
`filetypes` (table) --> Override the default list of associated filetypes for the server
`capabilities` (table) --> Override fields in capabilities. Can be used to disable certain LSP features.
`settings` (table) --> Override the default settings passed when initializing the server.

_example:_

```lua
lua_ls = {
  cmd = {...},
  filetypes = { ...},
  capabilities = {},
  settings = {...}
}
```

## Practical Example

- these [docs (quickstart)](https://github.com/mfussenegger/nvim-jdtls?tab=readme-ov-file#configuration-quickstart) and [video (verbose)](https://www.youtube.com/watch?v=94IU4cBdhfM) on installing jdtls (nvim-jdtls)
- goes through:

1. Installing jdtls package
2. Locating `jdtls` binary and configure on `cmd` for proper starting of LSP
3. Setup dirs, settings, and attach to lspconfig

```java
local config = {
  cmd = { vim.fn.expand '~/.local/share/nvim/mason/packages/jdtls/bin/jdtls' },
  root_dir = vim.fs.dirname(vim.fs.find({ 'gradlew', '.git', 'mvnw' }, { upward = true })[1]),
}
require('jdtls').start_or_attach(config)
```
