# macOS Terminal & Shell Guide

A practical guide to using the Terminal, customizing your shell, and leveraging command-line tools on macOS.

---

## Table of Contents
1. [Terminal Basics](#terminal-basics)
2. [Shells: Bash, Zsh, and Fish](#shells-bash-zsh-and-fish)
3. [Customization & Themes](#customization--themes)
4. [Essential Commands](#essential-commands)
5. [File & Directory Management](#file--directory-management)
6. [Process & System Management](#process--system-management)
7. [Networking & Security](#networking--security)
8. [Developer Workflows](#developer-workflows)
9. [Productivity Tips](#productivity-tips)
10. [Resources](#resources)

---

## Terminal Basics
- Open Terminal: Applications > Utilities > Terminal
- Use [iTerm2](https://iterm2.com/) for advanced features
- Tabs and split panes: ⌘ + T (new tab), ⌘ + D (split pane)
- Copy/paste: ⌘ + C / ⌘ + V (or use mouse)
- Drag files/folders into Terminal to auto-fill path

## Shells: Bash, Zsh, and Fish
- **Zsh** is the default shell on modern macOS (Catalina+)
- **Bash** is still available (`/bin/bash`)
- **Fish** is a user-friendly alternative (`brew install fish`)
- Change default shell:
  ```bash
  chsh -s /bin/zsh
  chsh -s /bin/bash
  chsh -s /usr/local/bin/fish
  ```
- Shell config files:
  - Zsh: `~/.zshrc`
  - Bash: `~/.bash_profile`, `~/.bashrc`
  - Fish: `~/.config/fish/config.fish`

## Customization & Themes
- Use [Oh My Zsh](https://ohmyz.sh/) for Zsh customization
- Install Powerlevel10k theme for a modern prompt
- Add plugins: zsh-autosuggestions, zsh-syntax-highlighting
- Customize aliases and functions in your shell config
- Set environment variables (e.g., `export PATH=...`)

## Essential Commands
- `ls`, `cd`, `pwd`, `cp`, `mv`, `rm`, `mkdir`, `touch`
- `cat`, `less`, `head`, `tail`, `grep`, `find`, `du`, `df`
- `open .` (open current directory in Finder)
- `pbcopy`, `pbpaste` (clipboard integration)
- `say "Hello"` (text-to-speech)
- `history`, `clear`, `exit`

## File & Directory Management
- List files: `ls -alh`
- Change directory: `cd ~/Documents`
- Copy/move files: `cp file.txt ~/Desktop/`, `mv file.txt ~/Desktop/`
- Remove files/directories: `rm file.txt`, `rm -rf folder/`
- Find files: `find . -name '*.md'`
- Search content: `grep 'pattern' file.txt`
- Disk usage: `du -sh *`, `df -h`

## Process & System Management
- List processes: `ps aux`, `top`, `htop` (install with Homebrew)
- Kill process: `kill PID`, `killall AppName`
- System info: `uname -a`, `sw_vers`, `system_profiler`
- Monitor resources: `Activity Monitor` app

## Networking & Security
- Check IP: `ifconfig`, `ipconfig getifaddr en0`
- Test connectivity: `ping google.com`, `traceroute example.com`
- Download files: `curl -O URL`, `wget URL` (install with Homebrew)
- SSH: `ssh user@host`
- Manage firewall: `sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setglobalstate on`

## Developer Workflows
- Use `git` for version control
- Use `python`, `node`, `ruby`, etc. for scripting
- Manage Python environments: `pyenv`, `virtualenv`, `conda`
- Manage Node.js versions: `nvm`
- Use `docker` for containers
- Use `brew` to install and manage CLI tools
- Automate tasks with shell scripts

## Productivity Tips
- Use tab completion and command history (up/down arrows)
- Use `alias` for frequent commands (e.g., `alias gs='git status'`)
- Use `tmux` for terminal multiplexing
- Use `fzf` for fuzzy file finding
- Use `ag` or `rg` for fast searching
- Use `man` for command manuals (e.g., `man ls`)

## Resources
- [macOS Terminal User Guide](https://support.apple.com/guide/terminal/welcome/mac)
- [Oh My Zsh](https://ohmyz.sh/)
- [iTerm2](https://iterm2.com/)
- [Homebrew](https://brew.sh/)
- [Zsh Documentation](https://zsh.sourceforge.io/Doc/)
- [Fish Shell](https://fishshell.com/)
- [tmux](https://github.com/tmux/tmux)

---

**Tip:** Mastering the Terminal unlocks the full power of macOS for development, automation, and productivity. 