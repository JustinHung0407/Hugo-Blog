---
title: Windows Working Environment Setup
description: my windows pc setup for develope
date: 2021-03-13T06:17:47.000+08:00
toc: true
images: 
author:
  '0': J
  '1': u
  '2': s
  '3': t
  '4': i
  '5': n
  '6': " "
  '7': H
  '8': u
  '9': n
  '10': g
  name: Justin Hung
type:
- posts
- post
tags:
- WSL2
- Docker Desktop
- zsh
- zim
- brew
categories:
- setup
- Learning
series:
- Home Kubernetes public website setup

---
# Achievement

> WSL2 + Docker Desktop + zsh/zim + brew
> ![K8s](/images/wsl.png)

## Step 1: WSL2

- Link: [https://docs.microsoft.com/zh-tw/windows/wsl/install-win10]()
- Quick command:
  - Enable WSL2
    `dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart`
  - Check system version
    - X64：1903 with 18362 or up
    - ARM64 : 2004 with 19041 or up
  - Enable VM Platform
    - `dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart`
    - **Reboot**
  - Download kernal update
    - [wsl_update_x64.msi](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)
  - Use WSL2
    - `wsl --set-default-version 2`
  - Download Distribution
    - [https://aka.ms/wslstore]()
      - [Ubuntu 20.04](https://www.microsoft.com/store/apps/9n6svws3rx71)

## Step 2: Windows Terminal

- Download: [Windows Terminal](https://www.microsoft.com/zh-tw/p/windows-terminal/9n0dx20hk701?rtc=1&activetab=pivot:overviewtab)

- My setting

  - theme: [Dracula](https://draculatheme.com/)
  - font: [MesloLGS NF](https://github.com/romkatv/dotfiles-public/tree/master/.local/share/fonts/NerdFonts)

      ``` json
      {
        "$schema":"https://aka.ms/terminal-profiles-schema",
        "defaultProfile":"{07b52e3e-de2c-5db4-bd2d-ba144ed6c273}",
        "copyOnSelect":true,
        "copyFormatting":true,
        "multiLinePasteWarning":false,
        "largePasteWarning":false,
        "confirmCloseAllTabs":false,
        "profiles":{
            "defaults":{
              "colorScheme":"Dracula",
              "acrylicOpacity":0.5,
              "snapOnInput":true,
              "startingDirectory":"%USERPROFILE%",
              "useAcrylic":true,
              "closeOnExit":true
            },
            "list":[
              {
                  "guid":"{07b52e3e-de2c-5db4-bd2d-ba144ed6c273}",
                  "suppressApplicationTitle":true,
                  "tabTitle":"Ubuntu 20.04",
                  "name":"Ubuntu-20.04",
                  "fontFace":"MesloLGS NF",
                  "source":"Windows.Terminal.Wsl",
                  "hidden":false
              }{
                  "guid":"{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
                  "name":"Windows PowerShell",
                  "commandline":"powershell.exe",
                  "hidden":false
              },
              {
                  "guid":"{0caa0dad-35be-5f56-a8ff-afceeeaa6101}",
                  "name":"Command Prompt",
                  "commandline":"cmd.exe",
                  "hidden":false
              }
            ]
        },
        "schemes":[
            {
              "name":"Dracula",
              "cursorColor":"#F8F8F2",
              "selectionBackground":"#44475A",
              "background":"#282A36",
              "foreground":"#F8F8F2",
              "black":"#21222C",
              "blue":"#BD93F9",
              "cyan":"#8BE9FD",
              "green":"#50FA7B",
              "purple":"#FF79C6",
              "red":"#FF5555",
              "white":"#F8F8F2",
              "yellow":"#F1FA8C",
              "brightBlack":"#6272A4",
              "brightBlue":"#D6ACFF",
              "brightCyan":"#A4FFFF",
              "brightGreen":"#69FF94",
              "brightPurple":"#FF92DF",
              "brightRed":"#FF6E6E",
              "brightWhite":"#FFFFFF",
              "brightYellow":"#FFFFA5"
            }
        ],
        "actions":[
            {
              "command":{
                  "action":"copy",
                  "singleLine":false
              },
              "keys":"ctrl+c"
            },
            {
              "command":"paste",
              "keys":"ctrl+v"
            },
            {
              "command":"find",
              "keys":"ctrl+shift+f"
            },
            {
              "command":{
                  "action":"splitPane",
                  "split":"auto",
                  "splitMode":"duplicate"
              },
              "keys":"alt+shift+d"
            },
            {
              "command":"closePane",
              "keys":"ctrl+w"
            }
        ]
      }
      ```

## Step 3: ZSH

- Installation
  - Ubuntu
    - `sudo apt-get install zsh -y`
    - `sudo chsh -s $(which zsh)`
  - CentOS
    - `sudo yum install -y zsh`
    - `sudo chsh -s $(which zsh)`

## Step 4: ZIM or Oh-my-zsh

--

### ZIM

- Link: [https://github.com/zimfw/zimfw]()
- Prerequest
  - exa
    - Link: [https://the.exa.website/]()
    - `sudo wget http://archive.ubuntu.com/ubuntu/pool/universe/r/rust-exa/exa_0.9.0-4_amd64.deb sudo apt-get install ./exa_0.9.0-4_amd64.deb`
    - Check `exa --version`
- Quick command:
  - Download
    `curl -fsSL https://raw.githubusercontent.com/zimfw/install/master/install.zsh | zsh`
  - Install powerlevel10k theme
    `echo "zmodule romkatv/powerlevel10k\nzmodule exa" >> ~/.zimrc`
  - Install and restart terminal
    `zimfw install`
  - Configure powerlevel10k
    `p10k configure`

### Oh-my-zsh

- Installation
  - Ubuntu
    - `sudo apt install zsh`
    - `sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`

* zsh-autosuggestions + zsh-syntax-highlighting + powerlevel10k
  ```
  sudo git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
  sudo git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
  sudo git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/themes/powerlevel10k
  ```
* sudo vi ~/.zshrc

  ```javascript
  # Use Powerlevel10k theme
  ZSH_THEME="powerlevel10k/powerlevel10k"

  # Use plugins
  plugins=(
    git
    docker
    zsh-autosuggestions
    zsh-syntax-highlighting
  )
  ```

* Configure powerlevel10k
  - `p10k configure`

## Step 5: Homebrew

- Link: [https://brew.sh/]()
- Quick command:
  - Download and Install
    `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
  - Add to path

    ```bash
    test -d ~/.linuxbrew && eval $(~/.linuxbrew/bin/brew shellenv)
    test -d /home/linuxbrew/.linuxbrew && eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv)
    test -r ~/.bash_profile && echo "eval \$($(brew --prefix)/bin/brew shellenv)" >>~/.bash_profile
    echo "eval \$($(brew --prefix)/bin/brew shellenv)" >>~/.profile
    ```

## Others

- Software
  - [Neofetch](https://github.com/dylanaraps/neofetch)
    - `sudo apt install neofetch`
- Font
  - [nerd-fonts](https://github.com/ryanoasis/nerd-fonts)
  - MesloLGS NF
- Theme
  - [Nord](https://www.nordtheme.com/)
  - [Dracula](https://draculatheme.com/)
- ZSH plugin
  - [awesome-zsh-plugins](https://github.com/unixorn/awesome-zsh-plugins)
- Backup WSL2

  - Official way
    - Check version and name`wsl -l -v`
    - Export
      - `wsl --export Ubuntu-20.04 ubuntubackup.tar`
    - Import
      - `wsl --import Ubuntu-20.04 C:\Users\MyPC\AppData\Local\Packages\Ubuntu C:\Users\MyPC\Documents\ubuntubackup.tar`
  - Using Windows Task Scheduler
    - [https://stephenreescarter.net/automatic-backups-for-wsl2/]()
  - Using LXRunOffline

    > Pending...

- WSL2 Location
  - `%userprofile%\AppData\Local\Packages`
  - match prefix `CanonicalGroupLimited`
  - the **VHDX**
- Alias
  - `alias canhas="sudo apt-get install -y"`

    ```
    alias dc="docker-compose"
    alias dcr="docker-compose run --rm"
    alias dcb="docker-compose run --rm --build"
    ```

## Reference

- [WSL2-setup](https://yduman.github.io/blog/wsl2-setup/)