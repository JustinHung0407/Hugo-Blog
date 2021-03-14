---
title: "Bash Alias"
description: "Alias for bash, zsh, zim"
date: 2021-03-14T19:05:48+08:00
draft: false
toc: false
images:
author: "Justin Hung"
type: ["posts", "post"]
tags:
  - WSL2
  - bash
  - zsh
  - zim
  - brew
categories:
  - setup
  - alias
series:
  - Shortcut
---

# Alias
- CentOS

  ``` bash
  echo "alias wget='wget -c '
  alias egrep='egrep --color=auto'
  alias fgrep='fgrep --color=auto'
  alias grep='grep --color=auto'
  alias l.='ls -d .* --color=auto'
  alias ll='ls -al --color=auto'
  alias ls='ls --color=auto'
  alias ping='ping -c 5'
  alias c='clear'

  alias s='sudo systemctl'
  alias sreload='sudo systemctl daemon-reload'
  alias st='sudo systemctl start'
  alias ss='sudo systemctl status'
  alias sr='sudo systemctl restart'
  alias sstop='sudo systemctl stop'
  alias update='sudo yum update -y && sudo yum upgrade -y'

  alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'">> ~/.bashrc
  ```

- Ubuntu (WSL)


  ``` bash
  echo "alias wget='wget -c '
  alias egrep='egrep --color=auto'
  alias fgrep='fgrep --color=auto'
  alias grep='grep --color=auto'
  alias ls='exa'
  alias l='exa -lbF --git'
  alias ll='exa -lbGF --git'
  alias llm='exa -lbGd --git --sort=modified'
  alias la='exa -lbhHigUmuSa --time-style=long-iso --git --color-scale'
  alias lx='exa -lbhHigUmuSa@ --time-style=long-iso --git --color-scale'
  alias lS='exa -1'
  alias lt='exa --tree --level=2'
  alias ping='ping -c 5'
  alias c='clear'
  alias ports='netstat -tulanp '
  source <(kubectl completion bash)
  alias kubens='kubectl config set-context --current --namespace '
  alias k='kubectl'
  alias update='sudo apt update -y && sudo apt upgrade -y'">> ~/.bashrc
  source ~/.bashrc
  ```


## Develope in Chrome without CORS
- "C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" --disable-web-security --disable-gpu --user-data-dir=~/chromeTemp


## Export
- `export HISTTIMEFORMAT='%F %T '`