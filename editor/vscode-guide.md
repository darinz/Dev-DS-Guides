# Visual Studio Code Complete Guide

[![VS Code](https://img.shields.io/badge/VS%20Code-007ACC?style=for-the-badge&logo=visualstudiocode&logoColor=white)](https://code.visualstudio.com/)
[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)](https://jupyter.org/)
[![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)](https://git-scm.com/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](../LICENSE)

Visual Studio Code (VS Code) is a powerful, free, and open-source code editor developed by Microsoft. It's particularly popular in data science due to its excellent Python support, Jupyter integration, and extensive extension ecosystem.

## Table of Contents

1. [Installation](#installation)
2. [Basic Setup](#basic-setup)
3. [Essential Extensions](#essential-extensions)
4. [Data Science Extensions](#data-science-extensions)
5. [Keyboard Shortcuts](#keyboard-shortcuts)
6. [Configuration](#configuration)
7. [Git Integration](#git-integration)
8. [Debugging](#debugging)
9. [Terminal Integration](#terminal-integration)
10. [Workspace Management](#workspace-management)
11. [Troubleshooting](#troubleshooting)

## Installation

### macOS
```bash
# Using Homebrew
brew install --cask visual-studio-code

# Manual installation
# Download from https://code.visualstudio.com/
```

### Linux
```bash
# Ubuntu/Debian
sudo apt update
sudo apt install software-properties-common apt-transport-https wget
wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"
sudo apt update
sudo apt install code

# Fedora/RHEL
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/code\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
sudo dnf install code
```

### Windows
Download the installer from [https://code.visualstudio.com/](https://code.visualstudio.com/)

## Basic Setup

### First Launch
1. Open VS Code
2. Install the Command Palette (`Cmd/Ctrl + Shift + P`)
3. Install recommended extensions for your file types

### Command Palette
- **macOS:** `Cmd + Shift + P`
- **Windows/Linux:** `Ctrl + Shift + P`

The Command Palette is your gateway to all VS Code functionality.

## Essential Extensions

### Core Extensions
- **Python** (Microsoft) - Python language support
- **Pylance** (Microsoft) - Fast Python language server
- **GitLens** - Enhanced Git capabilities
- **Auto Rename Tag** - Auto rename paired HTML/XML tags
- **Bracket Pair Colorizer** - Colorize matching brackets
- **Indent Rainbow** - Colorize indentation
- **Path Intellisense** - Autocomplete filenames
- **Prettier** - Code formatter
- **ESLint** - JavaScript linting
- **Thunder Client** - REST API client

### Installation
```bash
# Install extensions via command line
code --install-extension ms-python.python
code --install-extension ms-python.pylance
code --install-extension eamodio.gitlens
```

## Data Science Extensions

### Python & Jupyter
- **Python** (Microsoft) - Core Python support
- **Jupyter** (Microsoft) - Jupyter notebook support
- **Jupyter Keymap** - Jupyter keyboard shortcuts
- **Jupyter Slide Show** - Create presentations from notebooks
- **Python Docstring Generator** - Generate docstrings
- **Python Indent** - Smart Python indentation

### Data Visualization
- **Plotly** - Interactive plots
- **Rainbow CSV** - CSV file syntax highlighting
- **Excel Viewer** - View Excel files
- **Data Preview** - Preview CSV/JSON data

### Scientific Computing
- **R** - R language support
- **R LSP Client** - R language server
- **Julia** - Julia language support
- **LaTeX Workshop** - LaTeX support
- **Markdown All in One** - Enhanced Markdown support

## Keyboard Shortcuts

### Navigation
| Action | macOS | Windows/Linux |
|--------|-------|---------------|
| Command Palette | `Cmd + Shift + P` | `Ctrl + Shift + P` |
| Quick Open | `Cmd + P` | `Ctrl + P` |
| Go to Line | `Cmd + G` | `Ctrl + G` |
| Go to Symbol | `Cmd + Shift + O` | `Ctrl + Shift + O` |
| Go to Definition | `F12` | `