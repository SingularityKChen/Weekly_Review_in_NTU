---
layout: post
title: "[Tutorial] Linux aliases"
description: "Some useful Linux aliases."
categories: [Tutorial]
tags: [Linux, alias]
last_updated: 2021-05-15 14:58:00 GMT+8
excerpt: "Some useful Linux aliases."
redirect_from:
  - /2021/01/30/
---

* Kramdown table of contents
{:toc .toc}

# Linux Aliases

## Some useful aliases

### `cd & ls`

I use this command quite frequently and even replace the command `cd`.

```bash
function cl() { builtin cd "$@"; ls; }
```

This is an combination of `cd` and `ls`.

### `readlink -f`

I use this command to read the absolute directory of one file in Linux.

```bash
alias rdlk="readlink -f"
```

However, in MacOS, things are different.

```bash
brew install coreutils

# then
alias rdlk="greadlink -f"
```

## My aliases

Put these at `~/.bash_aliases`:

```bash
alias editalias="v ~/.bash_aliases"
#alias ls="ls -color=always"
alias cddd="cd ../../"
alias cddn="cd .." 
alias cdd="cd ../; ls"
alias CDD="cdd"
alias CDDD="cddd"
alias CD="cd"
function cl() { builtin cd "$@"; ls; }
alias cdp="cd -; ls" 
alias cdpn="cd -"
alias rm="rm -vi" 
alias del="rm -vr" 
alias mv="mv -vi" 
alias cp="cp -vi" 
alias la="ls -a" 
alias ll="ls -h -l" 
alias lr="ls -R" 
alias dh="df -h -a -T"
alias ds="du -sh"
alias h="history"
alias v="/usr/bin/vim"
alias g="/usr/bin/gvim"
alias rl="source ~/.bashrc"
alias rdlk="readlink -f"
alias grep="grep --color -n"
alias jpnb="jupyter notebook"
alias idea="intellij-idea-ultimate"
# for mill
alias genIdea="rm -rf out .idea && mill mill.scalalib.GenIdea/idea"
```

