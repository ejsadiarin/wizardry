---
date: 2025-11-30T14:56
title: Reduce Go binary size
tags: 
    - Go
---
<!-- 2025-11-30-1456 (November 30, 2025 02:56:27 PM) -->

# Reduce Go binary size

TLDR; use `go build -ldflags "-s -w"`

- can use [profiler to identify culprit behind large binary size](https://gsa.zxilly.dev/) 

- ref: [Reduce Go binary size](https://www.reddit.com/r/golang/comments/1p9trgh/reduce_go_binary_size/) 

## Problem

Go binaries are bigger (~38Mb) than usual because of certain reasons:

1. Go links statically, meaning there's no runtime you're linking to at runtime
    - as opposed to dynamically like C, Java, etc.
    - This has massive benefits in portability, and ease of deployment.
1. Go builds a runtime (Go runtime) for each binary
    - since there are no dynamic linking (unless you use CGO, which dynamically links to glibc at runtime)


## Solution

1. Pass the `-ldflags` flag to the go build command. This will allow you to configure the linker when building the binary. To reduce the size you can pass `-s -w` to disable the symbol table and DWARF generation respectively. For example, 

```bash
go build -ldflags "-s -w"
```

- This reduces the binary size by 20% - 40%.

A full list of flags that the linker takes can be seen with `go tool link`. 

- can also use `-trimpath` for clearer panic logs (but doesn't reduce size in any significant way)

2. You can use UPX

> [!NOTE]
> Careful upx compressed binaries tend to be flagged as viruses.

- So use upx for compressing binaries only for specific environments that allows it.
ex. **Only run binaries through UPX for Linux binaries that get pulled down during automation pipeline runs**

3. You can use tinygo

> [!NOTE]
> This works but a lot of networking features are missing.
