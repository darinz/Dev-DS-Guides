# Complete Guide to Windows Subsystem for Linux (WSL)

[![WSL](https://img.shields.io/badge/WSL-008080?style=for-the-badge&logo=windows&logoColor=white)](https://docs.microsoft.com/en-us/windows/wsl/)
[![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)](https://www.linux.org/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](../LICENSE)

## Table of Contents

1. [What is WSL?](#what-is-wsl)
2. [Why Use WSL?](#why-use-wsl)
3. [Installing WSL](#installing-wsl)
4. [Setting Up Your Linux Distribution](#setting-up-your-linux-distribution)
5. [Essential Configuration](#essential-configuration)
6. [Using WSL for Development](#using-wsl-for-development)
7. [Integrating WSL with Windows Tools](#integrating-wsl-with-windows-tools)
8. [Tips for Data Science Workflows](#tips-for-data-science-workflows)
9. [Troubleshooting](#troubleshooting)
10. [Resources](#resources)

---

## What is WSL?

Windows Subsystem for Linux (WSL) is a compatibility layer for running Linux binary executables natively on Windows 10/11. It enables you to use Linux command-line tools, utilities, and applications directly on Windows, without the overhead of a virtual machine.

## Why Use WSL?
- Access to Linux tools and workflows on Windows
- Seamless integration with Windows filesystem and applications
- Ideal for development, scripting, and data science
- Lightweight and fast compared to VMs

## Installing WSL

### Quick Install (WSL 2 Recommended)
Open PowerShell as Administrator and run:

```powershell
wsl --install
```

This command installs WSL 2 and the default Ubuntu distribution. Restart your computer if prompted.

### Manual Installation Steps
1. Enable the Windows Subsystem for Linux feature:
   ```powershell
   dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
   ```
2. Enable Virtual Machine Platform:
   ```powershell
   dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
   ```
3. Download and install a Linux distribution from the Microsoft Store (e.g., Ubuntu, Debian, Kali Linux).
4. Set WSL 2 as your default version:
   ```powershell
   wsl --set-default-version 2
   ```

## Setting Up Your Linux Distribution

- Launch your installed Linux distribution from the Start menu.
- Create a UNIX username and password when prompted.
- Update your package lists:
  ```bash
  sudo apt update && sudo apt upgrade
  ```
- Install common tools:
  ```bash
  sudo apt install build-essential git curl wget python3 python3-pip
  ```

## Essential Configuration

- **Access Windows files from Linux:**
  - Windows drives are mounted under `/mnt/c` (e.g., your Desktop is `/mnt/c/Users/<YourUsername>/Desktop`).
- **Set up SSH keys:**
  ```bash
  ssh-keygen -t ed25519 -C "your_email@example.com"
  eval "$(ssh-agent -s)"
  ssh-add ~/.ssh/id_ed25519
  ```
- **Configure Git:**
  ```bash
  git config --global user.name "Your Name"
  git config --global user.email "your_email@example.com"
  ```
- **Change default user (optional):**
  ```powershell
  ubuntu config --default-user <username>
  ```

## Using WSL for Development

- **Install programming languages:**
  - Python, Node.js, R, etc. via `apt` or language-specific installers.
- **Use VS Code with WSL:**
  - Install the [Remote - WSL extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) in VS Code.
  - Open a WSL folder in VS Code: `code .`
- **Run Jupyter Notebooks:**
  ```bash
  pip3 install notebook
  jupyter notebook --no-browser --ip=0.0.0.0
  ```
- **Access localhost servers:**
  - Services started in WSL (e.g., Flask, Jupyter) are accessible from Windows at `localhost:<port>`.

## Integrating WSL with Windows Tools

- **Access Windows executables from Linux:**
  ```bash
  /mnt/c/Windows/System32/notepad.exe
  explorer.exe .
  ```
- **Copy files between Windows and Linux:**
  - Use standard `cp`, `mv`, or drag-and-drop in Windows Explorer.
- **Clipboard integration:**
  ```bash
  echo "Hello from WSL" | clip.exe
  powershell.exe Get-Clipboard
  ```

## Tips for Data Science Workflows

- Use WSL for Python, R, and Julia development with native Linux tools.
- Install Conda or virtualenv for Python environment management.
- Use VS Code Remote - WSL for seamless editing and debugging.
- Store large datasets on the Windows filesystem for easy access from both environments.

## Troubleshooting

- **Check WSL version:**
  ```powershell
  wsl --list --verbose
  ```
- **Upgrade WSL 1 to WSL 2:**
  ```powershell
  wsl --set-version <distro> 2
  ```
- **Reset or unregister a distribution:**
  ```powershell
  wsl --unregister <distro>
  ```
- **Networking issues:**
  - Restart WSL: `wsl --shutdown`
  - Restart LxssManager service from Windows Services
- **File permission issues:**
  - Use `chmod` and `chown` as needed

## Resources

- [Microsoft WSL Documentation](https://docs.microsoft.com/en-us/windows/wsl/)
- [WSL Github Repo](https://github.com/microsoft/WSL)
- [Awesome WSL](https://github.com/sirredbeard/Awesome-WSL)
- [VS Code Remote - WSL](https://code.visualstudio.com/docs/remote/wsl) 