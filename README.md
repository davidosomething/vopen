# vopen

> Edit a file using a single instance of Vim/gVim/Neovim

| Name          | Link           |
| ------------- | -------------- |
| Project Home: | [https://github.com/davidosomething/vopen](https://github.com/davidosomething/vopen)

## About

This uses Vim's server capability to use a single instance of Vim.
It also intelligently determines when to use that capability:

- If on Linux with an X Server, will use `gvim`
- If on Mac, it will use `gvim` (which is the same as MacVim.app if you use
  that)
- If in an SSH session or linux with no X11 it will use terminal Vim with no
  server.

It takes of Vim's normal args if you provide any extra ones.

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

You can start Vim with a default command using the `VOPEN_DEFAULT_COMMAND`
global variable in your system. E.g. to start vimfiler:

```bash
export VOPEN_DEFAULT_COMMAND="+VimFilerCurrentDir"
```

The format is any Vim-compatible args (the plus sign means run this command).

For [neovim-remote] I recommend the following:

```bash
export VOPEN_SERVERNAME="$NVIM_LISTEN_ADDRESS"
export VOPEN_DEFAULT_COMMAND="--remote-silent +enew"
export VOPEN_REUSE_COMMAND="--remote-silent"
export VOPEN_EDITOR="nvr"
export VOPEN_VISUAL="nvr"
```

### Reuse command

If there's an existing editor server as detected by `$VOPEN_EDITOR --serverlist`,
and you run `vopen` without any arguments, it will try to use the
`VOPEN_REUSE_COMMAND` by default.

For Vim with `+clientserver`, the default reuse command is
`--remote-send \":call foreground()<CR>\"`, which will bring the instance to
the foreground. See `:h foreground()` in Vim for details.

For Neovim with [neovim-remote] I recommend the following:

```bash
export VOPEN_REUSE_COMMAND="--remote-silent +sleep"
```

which does a silent no-op and will suppress [neovim-remote] messages.

### Use alternate vim/Neovim

`vopen` supports environment variables for specifying what command to run for
the editor and gui editors.

- `VOPEN_EDITOR` defaults to `vim`. I.e., for [neovim-remote] set to `nvr`.
- `VOPEN_VISUAL` defaults to `gvim`. I.e., for [neovim-remote] set to `nvr`.
- `VOPEN_USE_GUI` defaults to true if you have a gui-capable display
  (x11/osx).
- `VOPEN_USE_SERVER` defaults to true, use to disable server altogether
- `VOPEN_SERVERNAME` defaults to `VOPEN` to use a single instance called
  `VOPEN`. I.e., for [neovim-remote] set to `$NVIM_LISTEN_ADDRESS`.

Here is an example using Neovim as the terminal editor and `coolwanglu/neovim-e`
as the gui editor:

```bash
export VOPEN_EDITOR="nvim"
export VOPEN_VISUAL="/Applications/Electron.app/Contents/MacOS/Electron ~/src/neovim-e"
export VOPEN_USE_GUI=false
export VOPEN_USE_SERVER=false
```

Here is an example using [neovim-remote]:

```bash
export NVIM_LISTEN_ADDRESS=/tmp/nvimsocket
export VOPEN_SERVERNAME="$NVIM_LISTEN_ADDRESS"
export VOPEN_EDITOR="nvr"
export VOPEN_VISUAL="nvr"
export VOPEN_DEFAULT_COMMAND="+enew"
export VOPEN_REUSE_COMMAND="--remote-silent +sleep"
```

### Never use server

If you're using a version of Vim (e.g. neovim-e) that does not support servers
but you still want to use `vopen` for some reason, you can disable servers:

```bash
export VOPEN_USE_SERVER=false
```

There is also a commandline flag:

```bash
vopen --noserver myfile.txt
```

### Debugging

Run `vopen` with the environment variable `VOPEN_DEBUG` set to see what it will
execute. I.e.

```bash
VOPEN_DEBUG=1 vopen
```

## Changelog

### 2017-05-01

- FIXED - neovim-remote 1.6.0 fixes for silent opening; cut 1.3.0

### 2016-11-11

- CHANGED - check for `--version` flag and pass through directly to editor
  (directly to nvim if using nvr)

### 2016-10-31

- ADDED - learned VOPEN_REUSE_COMMAND env option
- ADDED - learned VOPEN_DEBUG env option

### 2016-10-28

- ADDED - [neovim-remote] support, cut 1.0.2
- ADDED - learned VOPEN_SERVERNAME env option

### 2016-01-29

- ADDED - VOPEN_USE_GUI if you want to never use the gui
- FIXED - better logic to determine what editor mode to use

### 2015-12-03

- FIXED - quoting

### 2015-10-15

- CHANGED - remove bash error modes (v0.1.0)
- CHANGED - no --nofork mode for nvim either (v0.0.10)
- CHANGED - detect nvim and default to serverless since there is
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

- ADDED - `--servername` flag will properly override servername.

### 2015-05-05

- FIXED - Server is not used when `--nofork` arg is present.

### 2015-05-03

- published

[neovim-remote]: https://github.com/mhinz/neovim-remote
