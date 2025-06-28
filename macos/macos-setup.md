# macOS Setup Guide for Developers & Power Users

A step-by-step guide to setting up a new Mac for software development, research, and productivity.

---

## Table of Contents
1. [Initial Setup & System Preferences](#initial-setup--system-preferences)
2. [Homebrew & Package Management](#homebrew--package-management)
3. [Essential Applications](#essential-applications)
4. [Developer Tools](#developer-tools)
5. [Shell & Terminal Customization](#shell--terminal-customization)
6. [Security & Privacy](#security--privacy)
7. [Productivity Enhancements](#productivity-enhancements)
8. [Backup & Maintenance](#backup--maintenance)

---

## Initial Setup & System Preferences
- Create your user account and set a strong password
- Enable FileVault disk encryption
- Set up Touch ID and Apple Watch unlock
- Configure trackpad, keyboard, and display settings
- Set up Night Shift and True Tone
- Configure Dock, Mission Control, and Hot Corners
- Set up iCloud, Handoff, and Continuity

## Homebrew & Package Management
- Install [Homebrew](https://brew.sh/):
  ```bash
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
  ```
- Add Homebrew to your PATH (if needed)
- Install essential packages:
  ```bash
  brew install git python node wget htop tmux zsh
  brew install --cask iterm2 visual-studio-code google-chrome slack rectangle
  ```
- Use `brew search` and `brew info` to discover more tools

## Essential Applications
- **Browsers**: Google Chrome, Firefox, Safari
- **Editors/IDEs**: Visual Studio Code, Sublime Text, PyCharm, Xcode
- **Terminal**: iTerm2, Hyper
- **Productivity**: Rectangle (window manager), Alfred, Magnet, Notion, Obsidian
- **Communication**: Slack, Zoom, Microsoft Teams
- **Cloud Storage**: Dropbox, Google Drive, OneDrive
- **Password Manager**: 1Password, Bitwarden

## Developer Tools
- **Xcode Command Line Tools**:
  ```bash
  xcode-select --install
  ```
- **Git**: Version control
- **Python**: Use `pyenv`, `pipenv`, or `poetry` for environments
- **Node.js**: Use `nvm` for version management
- **Docker**: For containerized development
- **Database Clients**: TablePlus, DBeaver, Postico
- **API Tools**: Postman, Insomnia
- **Virtualization**: Parallels, VMware Fusion, VirtualBox

## Shell & Terminal Customization
- Use [iTerm2](https://iterm2.com/) for advanced terminal features
- Change your default shell to Zsh (default on modern macOS)
- Customize with [Oh My Zsh](https://ohmyz.sh/):
  ```bash
  sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
  ```
- Install Powerlevel10k theme and useful plugins (zsh-autosuggestions, zsh-syntax-highlighting)
- Configure aliases and functions in `~/.zshrc`

## Security & Privacy
- Enable FileVault and Firewall
- Set up automatic software updates
- Use a password manager
- Configure privacy settings (System Preferences > Security & Privacy)
- Enable Find My Mac
- Use strong, unique passwords
- Consider Little Snitch or LuLu for network monitoring

## Productivity Enhancements
- Set up Spotlight and Alfred for quick search
- Use window managers (Rectangle, Magnet)
- Set up hotkeys and custom shortcuts
- Use clipboard managers (Paste, CopyClip)
- Automate tasks with Automator and Shortcuts

## Backup & Maintenance
- Set up Time Machine for automatic backups
- Use cloud storage for important files
- Regularly check for macOS and app updates
- Clean up disk space with built-in tools or apps like CleanMyMac (optional)

---

**Tip:** Keep your system and tools up to date for security and performance. Regularly review your setup to remove unused apps and optimize your workflow. 