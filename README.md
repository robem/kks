# kks

Handy Kakoune companion.

## Installation

### From release binaries

Download the compiled binary for your system from
[Releases](https://github.com/kkga/kks/releases) page and put it somewhere in
your `$PATH`.

### From source

Requires [Go](https://golang.org/) installed on your system.

Clone the repository and run `go build`, then copy the compiled binary somewhere
in your `$PATH`.

If Go is [configured](https://golang.org/ref/mod#go-install) to install packages
in `$PATH`, it's also possible to install without cloning the repository: run
`go install github.com/kkga/kks@latest`.

## Kakoune and shell integration

### Kakoune configuration

Source `kks init` to add `kks-connect` command to Kakoune...

```kak
eval %sh{ kks init }
```

... and use your terminal integration to connect
[provided scripts](#provided-scripts), for example:
`kks-connect terminal kks-files`.

### Kakoune mappings example

```kak
map global normal -docstring 'terminal'         <c-t> ': kks-connect terminal<ret>'
map global normal -docstring 'files'            <c-f> ': kks-connect popup kks-files<ret>'
map global normal -docstring 'buffers'          <c-b> ': kks-connect popup kks-buffers<ret>'
map global normal -docstring 'files by content' <c-g> ': kks-connect popup kks-grep<ret>'
map global normal -docstring 'lines in buffer'  <c-l> ': kks-connect popup kks-lines<ret>'
map global normal -docstring 'recent files'     <c-r> ': kks-connect popup kks-mru<ret>'
map global normal -docstring 'lf'               <c-h> ': kks-connect panel kks-lf<ret>'
map global normal -docstring 'lazygit'          <c-v> ': kks-connect popup lazygit<ret>'
```

For more terminal integrations and for the (quite handy) `popup` command, see:

- [alacritty.kak](https://github.com/alexherbo2/alacritty.kak)
- [foot.kak](https://github.com/kkga/foot.kak)

### Shell configuration example

```sh
export EDITOR=`kks edit`

alias k='kks edit'
alias ks='eval (kks-select)'
alias kcd='cd (kks get %sh{pwd})'
alias ka='kks attach'
alias kl='kks list'
```

## Commands

This is the output of `kks -h`. Certain commands take additional flags, see
`kks <command> -h` to learn more.

```
USAGE
  kks <command> [-s <session>] [-c <client>] [<args>]

COMMANDS
  new, n         create new session
  edit, e        edit file
  send, s        send command
  attach, a      attach to session
  kill           kill session
  ls             list sessions and clients
  get            get %{val}, %{opt} and friends
  cat            print buffer content
  env            print env
  init           print Kakoune definitions

ENVIRONMENT VARIABLES
  KKS_SESSION    Kakoune session
  KKS_CLIENT     Kakoune client

Use "kks <command> -h" for command usage.
```

## Configuration

`kks` can be configured through environment variables.

#### Automatic sessions based on git directory

```
export KKS_USE_GITDIR_SESSIONS=1
```

When `KKS_USE_GITDIR_SESSIONS` is set to any value and `KKS_SESSION` is empty,
running `kks edit` will do the following:

- if file is inside a git directory, `kks` will search for an existing session
  based on top-level git directory name and connect to it;
- if a session for the directory doesn't exist, `kks` will start a new session
  and connect to it.

## Provided scripts

| script                                 | function                                                |
| -------------------------------------- | ------------------------------------------------------- |
| [`kks-buffers`](./scripts/kks-buffers) | pick buffers                                            |
| [`kks-files`](./scripts/kks-files)     | pick files                                              |
| [`kks-grep`](./scripts/kks-grep)       | search for pattern in working directory                 |
| [`kks-lf`](./scripts/kks-lf)           | open [lf] with current buffer selected                  |
| [`kks-lines`](./scripts/kks-lines)     | jump to line in buffer                                  |
| [`kks-mru`](./scripts/kks-mru)         | pick recently opened file                               |
| [`kks-select`](./scripts/kks-select)   | select Kakoune session and client to set up environment |

[lf]: https://github.com/gokcehan/lf

---

## Similar projects

- [kakoune.cr](https://github.com/alexherbo2/kakoune.cr)
- [kakoune-remote-control](https://github.com/danr/kakoune-remote-control)
