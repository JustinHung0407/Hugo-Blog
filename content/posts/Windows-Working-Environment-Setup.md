---
title: Windows Working Environment Setup
description: my windows pc setup for develope
date: 2021-03-13T06:17:47.000+08:00
toc: true

---
# Achievement

> WSL2 + Docker Desktop + zsh/zim + brew
> ![K8s](/images/wsl.png)

## Step 1: WSL2

- New Way
  - requirement: Windows 10 version 2004 and higher (Build 19041 and higher) or Windows 11
  - `wsl --install`

- Old Way
  - Link: <https://docs.microsoft.com/zh-tw/windows/wsl/install-win10>
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

- Download: 
  - [Microsoft Store](https://www.microsoft.com/zh-tw/p/windows-terminal/9n0dx20hk701?rtc=1&activetab=pivot:overviewtab)
  - chocolately: `choco install microsoft-windows-terminal`

- My setting

  - theme: [Dracula](https://draculatheme.com/)
  - font: [MesloLGS NF](https://github.com/romkatv/dotfiles-public/tree/master/.local/share/fonts/NerdFonts)

      ``` json
      {
          "$schema": "https://aka.ms/terminal-profiles-schema",
          "actions": [
              {
                  "command": {
                      "action": "copy",
                      "singleLine": false
                  },
                  "keys": "ctrl+c"
              },
              {
                  "command": "find",
                  "keys": "ctrl+shift+f"
              },
              {
                  "command": "paste",
                  "keys": "ctrl+v"
              },
              {
                  "command": {
                      "action": "splitPane",
                      "split": "auto",
                      "splitMode": "duplicate"
                  },
                  "keys": "alt+shift+d"
              },
              {
                  "command": "closePane",
                  "keys": "ctrl+w"
              }
          ],
          "confirmCloseAllTabs": false,
          "copyFormatting": "all",
          "copyOnSelect": true,
          "defaultProfile": "{2c4de342-38b7-51cf-b940-2309a097f518}",
          "experimental.rendering.forceFullRepaint": true,
          "experimental.rendering.software": true,
          "largePasteWarning": false,
          "multiLinePasteWarning": false,
          "profiles": {
              "defaults": {
                  "acrylicOpacity": 0.7,
                  "closeOnExit": "graceful",
                  "colorScheme": "Dracula",
                  "snapOnInput": true,
                  "startingDirectory": "%USERPROFILE%",
                  "useAcrylic": true
              },
              "list": [
                  {
                      "font": {
                          "face": "MesloLGS NF",
                          "size": 14
                      },
                      "guid": "{2c4de342-38b7-51cf-b940-2309a097f518}",
                      "hidden": false,
                      "name": "Ubuntu",
                      "source": "Windows.Terminal.Wsl"
                  },
                  {
                      "commandline": "gsudo.exe powershell.exe",
                      "font": {
                          "face": "MesloLGS NF",
                          "size": 12
                      },
                      "guid": "{41dd7a51-f0e1-4420-a2ec-1a7130b7e950}",
                      "hidden": false,
                      "icon": "C:\\Users\\hugh1\\Pictures\\Icons\\powershell-elevated.png",
                      "name": "Windows PowerShell Elevated"
                  },
                  {
                      "commandline": "powershell.exe",
                      "guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
                      "hidden": false,
                      "name": "Windows PowerShell"
                  },
                  {
                      "commandline": "cmd.exe",
                      "guid": "{0caa0dad-35be-5f56-a8ff-afceeeaa6101}",
                      "hidden": false,
                      "name": "CMD"
                  },
                  {
                      "guid": "{b453ae62-4e3d-5e58-b989-0a998ec441b8}",
                      "hidden": false,
                      "name": "Azure Cloud Shell",
                      "source": "Windows.Terminal.Azure"
                  }
              ]
          },
          "schemes": [
              {
                  "background": "#282A36",
                  "black": "#21222C",
                  "blue": "#BD93F9",
                  "brightBlack": "#6272A4",
                  "brightBlue": "#D6ACFF",
                  "brightCyan": "#A4FFFF",
                  "brightGreen": "#69FF94",
                  "brightPurple": "#FF92DF",
                  "brightRed": "#FF6E6E",
                  "brightWhite": "#FFFFFF",
                  "brightYellow": "#FFFFA5",
                  "cursorColor": "#F8F8F2",
                  "cyan": "#8BE9FD",
                  "foreground": "#F8F8F2",
                  "green": "#50FA7B",
                  "name": "Dracula",
                  "purple": "#FF79C6",
                  "red": "#FF5555",
                  "selectionBackground": "#44475A",
                  "white": "#F8F8F2",
                  "yellow": "#F1FA8C"
              }
          ]
      }
      ```

## Step 3: ZSH

- Installation
  - Ubuntu
    - `sudo apt-get install zsh -y`
    - `chsh -s $(which zsh)`
  - CentOS
    - `sudo yum install zsh -y`
    - `chsh -s $(which zsh)`

## Step 4: ZIM or Oh-my-zsh

--

### ZIM

- Link: <https://github.com/zimfw/zimfw>
- Prerequest
  - exa
    - Link: <https://the.exa.website>
    - `sudo wget http://archive.ubuntu.com/ubuntu/pool/universe/r/rust-exa/exa_0.9.0-4_amd64.deb && sudo apt-get install ./exa_0.9.0-4_amd64.deb`
    - Check `exa --version`
- Quick command:
  - Download: 
    `curl -fsSL https://raw.githubusercontent.com/zimfw/install/master/install.zsh | zsh`
  - Setup Path: `echo << "ZIM_HOME=/home/justin/.zim" >> ~/.zshrc && source ~/.zshrc`
  - Install powerlevel10k theme
    `echo "zmodule romkatv/powerlevel10k\nzmodule exa" >> ~/.zimrc`
  - Install and restart terminal
    `zimfw install`
  - Configure powerlevel10k
    `p10k configure`

- More ZIM Modules
  - <https://zimfw.sh/docs/modules/>

- Uninstall
  - `rm -rf ~/.zim ~/.zimrc ~/.zlogin`

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
    - `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
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
    - `brew install neofetch`
- Font
  - [nerd-fonts](https://github.com/ryanoasis/nerd-fonts)
  - [MesloLGS NF](https://github.com/romkatv/powerlevel10k)
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

- [WSL2 Official Website](https://docs.microsoft.com/en-us/windows/wsl/install)
- [WSL2-setup](https://yduman.github.io/blog/wsl2-setup/)