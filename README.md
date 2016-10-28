# vopen

> Edit a file using a single instance of vim/gvim/mvim

| Name          | Link           |
| ------------- | -------------- |
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
- Optionally add the `vopen-nofork` file somewhere in your global path (make
  sure the root user/sudo has this in its path), e.g. `/usr/local/bin/`. This is
  used for `$EDITOR` / `$SUDO_EDITOR`, and is compatible with `git commit`,
  `visudo`, `vipw`, etc.

## Usage

```bash
vopen (args) (filenames/directories)
```

Typically I alias "vopen" to "e".

```bash
alias e="vopen"
```

Use the nofork version `vopen-nofork` for `$EDITOR` and `$VISUAL` if you plan
on modifying your env (`.bashrc`/`.zshrc`/etc.).

```bash
export EDITOR="vopen-nofork"
export SUDO_EDITOR="vopen-nofork"
export VISUAL="vopen-nofork"
```

### Default command

You can start vim with a default command if no files/directories are provided
using the `VOPEN_DEFAULT_COMMAND` global variable in your system. E.g. to
start vimfiler:

```bash
export VOPEN_DEFAULT_COMMAND="+VimFilerCurrentDir"
```

The format is any vim-compatible args (the plus sign means run this command).

### Use alternate vim/neovim

`vopen` supports two environment variables for specifying what command to run
for the editor and gui editors.

- `VOPEN_EDITOR` defaults to `vim`. I.e., for `neovim-remote` set to `nvr`.
- `VOPEN_VISUAL` defaults to `gvim`. I.e., for `neovim-remote` set to `nvr`.
- `VOPEN_USE_GUI` defaults to true if you have a gui-capable display
  (x11/osx).
- `VOPEN_USE_SERVER` defaults to true, use to disable server altogether
- `VOPEN_SERVERNAME` defaults to `VOPEN` to use a single instance called
  `VOPEN`. I.e., for `neovim-remote` set to `$NVIM_LISTEN_ADDRESS`.

Here is an example using NeoVim as the terminal editor and `coolwanglu/neovim-e`
as the gui editor:

```bash
export VOPEN_EDITOR="nvim"
export VOPEN_VISUAL="/Applications/Electron.app/Contents/MacOS/Electron ~/src/neovim-e"
export VOPEN_USE_GUI=false
export VOPEN_USE_SERVER=false
```

Here is an example using [neovim-remote](https://github.com/mhinz/neovim-remote):

```bash
export NVIM_LISTEN_ADDRESS=/tmp/nvimsocket
export VOPEN_SERVERNAME="$NVIM_LISTEN_ADDRESS"
export VOPEN_EDITOR="nvr"
export VOPEN_VISUAL="nvr"
```

### Never use server

If you're using a version of vim (e.g. neovim-e) that does not support servers
but you still want to use `vopen` for some reason, you can disable servers:

```bash
export VOPEN_USE_SERVER=false
```

There is also a commandline flag:

```bash
vopen --noserver myfile.txt
```

## Changelog

### 2016-10-28

- ADDED - neovim-remote support, cut 1.0.2
- ADDED - learned VOPEN_SERVERNAME env option

### 2016-01-29

- ADDED - VOPEN_USE_GUI if you want to never use the gui
- FIXED - better logic to determine what editor mode to use

### 2015-12-03

- FIXED - quoting

### 2015-10-15

- CHANGED - remove bash error modes (v0.1.0)
- CHANGED - no --nofork mode for nvim either (v0.0.10)
- CHANGED - detect when nvim is used and default to serverless since there is
  no server mode (v0.0.9)

### 2015-10-02

- CHANGED - default servername is now suffixed with -VOPEN to distinguish from
  `--nofork` runs

### 2015-10-01

- ADDED - `--noserver` flag, cut v0.0.7

### 2015-09-22

- FIXED - quote file paths, files w/escaped spaces work now

### 2015-05-13

- ADDED - env vars for VOPEN_VISUAL, VOPEN_EDITOR, VOPEN_USE_SERVER

### 2015-05-09

- FIXED - use `$OSTYPE` instead of my shell var

### 2015-05-08

- ADDED - `--servername` flag will override servername correctly.

### 2015-05-05

- FIXED - Server is not used when `--nofork` arg is provided.

### 2015-05-03

- published

Copyright (c) 2015 David O'Trakoun <me@davidosomething.com>
