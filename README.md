# vopen

> Edit a file using a single instance of vim/gvim/mvim


| Name | Link |
| ---- | ---- |
| Project Home: | [https://github.com/davidosomething/vopen](https://github.com/davidosomething/vopen)

## About

This uses VIM's server capability to use only a single instance of gVim.
It also intelligently determines when to use that capability:

- If on Linux with an X Server, will use `gvim`
- If on Mac, it will use `gvim` (which is the same as MacVim.app if you use
  that)
- If in an SSH session or linux with no X11 it will use terminal VIM with no
  server.

It takes all of vim's normal args if you provide any extra ones.

## Installation

- Add the `vopen` file somewhere in your path, e.g. `/usr/local/bin/`

## Usage

```
vopen (args) (filenames/directories)
```

Typically I alias "vopen" to "e".

```
alias e="vopen"
```

### Default command

You can start vim with a default command if no files/directories are provided
using the `VOPEN_DEFAULT_COMMAND` global variable in your system. E.g. to
start vimfiler:

```
export VOPEN_DEFAULT_COMMAND="+VimFilerCurrentDir"
```

The format is any vim-compatible args (the plus sign means run this command).

## Changelog

```
2015-05-03 - published

```

Copyright (c) 2015 David O'Trakoun <me@davidosomething.com>