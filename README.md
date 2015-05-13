# vopen v0.0.5

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

### Use alternate vim/neovim

`vopen` supports two environment variables for specifying what command to run
for the editor and gui editors.

- `VOPEN_EDITOR` defaults to `vim`
- `VOPEN_VISUAL` defaults to gvim.

Here is an example using NeoVim as the terminal editor and `coolwanglu/neovim-e`
as the gui editor:

```
export VOPEN_EDITOR="nvim"
export VOPEN_VISUAL="/Applications/Electron.app/Contents/MacOS/Electron ~/src/neovim-e"
```

## Changelog

```
2015-05-09 - [fixed] use `$OSTYPE` instead of my shell var
2015-05-08 - [added] --servername flag will override servername correctly.
2015-05-05 - [fixed] Server is not used when --nofork arg is provided.
2015-05-03 - published

```

Copyright (c) 2015 David O'Trakoun <me@davidosomething.com>
