# Emacs Complete Guide

[![Emacs](https://img.shields.io/badge/Emacs-7F5AB6?style=for-the-badge&logo=gnuemacs&logoColor=white)](https://www.gnu.org/software/emacs/)
[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](../LICENSE)

Emacs is a highly extensible, customizable text editor and computing environment. It's more than just a text editor - it's a complete computing environment that can be tailored to any task. Emacs is particularly popular among programmers, researchers, and power users who value its extensibility and integrated approach to computing.

## Table of Contents

1. [Installation](#installation)
2. [Emacs Philosophy](#emacs-philosophy)
3. [Basic Concepts](#basic-concepts)
4. [Essential Commands](#essential-commands)
5. [Configuration](#configuration)
6. [Package Management](#package-management)
7. [Org-Mode](#org-mode)
8. [Data Science Setup](#data-science-setup)
9. [Advanced Features](#advanced-features)
10. [Troubleshooting](#troubleshooting)

## Installation

### macOS
```bash
# Using Homebrew
brew install emacs

# Using MacPorts
sudo port install emacs

# Check installation
emacs --version
```

### Linux
```bash
# Ubuntu/Debian
sudo apt update
sudo apt install emacs

# Fedora/RHEL
sudo dnf install emacs

# Arch Linux
sudo pacman -S emacs

# Check installation
emacs --version
```

### Windows
```bash
# Using Chocolatey
choco install emacs

# Using Scoop
scoop install emacs

# Download from https://www.gnu.org/software/emacs/
```

## Emacs Philosophy

### The Emacs Way
Emacs is built around several core principles:
- **Extensibility**: Everything can be customized and extended
- **Integration**: All tools work together seamlessly
- **Consistency**: Uniform interface across all features
- **Efficiency**: Powerful commands for complex operations
- **Self-documenting**: Built-in help and documentation

### Why Emacs?
- **Extensibility**: Can be customized for any task
- **Integration**: Email, news, file management, terminal all in one
- **Org-mode**: Powerful note-taking and project management
- **Lisp-based**: Programmable in Emacs Lisp
- **Longevity**: Decades of development and refinement

## Basic Concepts

### Buffers, Windows, and Frames
- **Buffer**: A container for text (file content, scratch space, etc.)
- **Window**: A viewport into a buffer
- **Frame**: A complete Emacs window (can contain multiple windows)

### Modes
- **Major Mode**: Determines behavior for a specific file type
- **Minor Mode**: Provides additional functionality
- **Fundamental Mode**: Basic editing mode

### Key Notation
- `C-` = Control key
- `M-` = Meta key (Alt or Option)
- `C-M-` = Control + Meta
- `RET` = Return/Enter key
- `SPC` = Space bar

## Essential Commands

### File Operations
| Command | Key | Action |
|---------|-----|--------|
| Find File | `C-x C-f` | Open a file |
| Save Buffer | `C-x C-s` | Save current buffer |
| Save As | `C-x C-w` | Save buffer with new name |
| Kill Buffer | `C-x k` | Close current buffer |
| Switch Buffer | `C-x b` | Switch to another buffer |
| List Buffers | `C-x C-b` | Show all buffers |

### Navigation
| Command | Key | Action |
|---------|-----|--------|
| Beginning of Line | `C-a` | Move to start of line |
| End of Line | `C-e` | Move to end of line |
| Beginning of Buffer | `M-<` | Move to start of buffer |
| End of Buffer | `M->` | Move to end of buffer |
| Forward Word | `M-f` | Move forward one word |
| Backward Word | `M-b` | Move backward one word |
| Forward Paragraph | `M-}` | Move forward one paragraph |
| Backward Paragraph | `M-{` | Move backward one paragraph |

### Editing
| Command | Key | Action |
|---------|-----|--------|
| Kill Line | `C-k` | Delete from cursor to end of line |
| Kill Word | `M-d` | Delete word forward |
| Kill Word Backward | `M-DEL` | Delete word backward |
| Yank (Paste) | `C-y` | Paste last killed text |
| Yank Pop | `M-y` | Cycle through kill ring |
| Transpose Characters | `C-t` | Swap characters |
| Transpose Words | `M-t` | Swap words |
| Transpose Lines | `C-x C-t` | Swap lines |

### Search and Replace
| Command | Key | Action |
|---------|-----|--------|
| Incremental Search | `C-s` | Search forward |
| Reverse Search | `C-r` | Search backward |
| Query Replace | `M-%` | Replace with confirmation |
| Replace String | `M-x replace-string` | Replace all occurrences |
| Regexp Replace | `M-x replace-regexp` | Replace using regex |

### Window Management
| Command | Key | Action |
|---------|-----|--------|
| Split Window | `C-x 2` | Split horizontally |
| Split Window Vertically | `C-x 3` | Split vertically |
| Delete Window | `C-x 0` | Close current window |
| Delete Other Windows | `C-x 1` | Close other windows |
| Switch Window | `C-x o` | Switch to other window |
| Balance Windows | `C-x +` | Equalize window sizes |

## Configuration

### Init File
Create `~/.emacs.d/init.el` (modern approach) or `~/.emacs`:

```elisp
;; Basic Settings
(setq inhibit-startup-message t)
(setq initial-scratch-message nil)
(setq make-backup-files nil)
(setq auto-save-default nil)
(setq create-lockfiles nil)

;; Display Settings
(setq line-number-mode t)
(setq column-number-mode t)
(setq size-indication-mode t)
(setq display-time-mode t)

;; Indentation
(setq-default indent-tabs-mode nil)
(setq-default tab-width 4)
(setq-default standard-indent 4)

;; Scrolling
(setq scroll-step 1)
(setq scroll-margin 5)
(setq scroll-conservatively 10000)

;; Search
(setq case-fold-search t)
(setq search-highlight t)
(setq query-replace-highlight t)

;; Font
(set-face-attribute 'default nil :font "Monaco-14")

;; Theme
(load-theme 'wombat t)

;; Package Management
(require 'package)
(setq package-archives
      '(("gnu" . "https://elpa.gnu.org/packages/")
        ("melpa" . "https://melpa.org/packages/")
        ("org" . "https://orgmode.org/elpa/")))
(package-initialize)
```

### Advanced Configuration
```elisp
;; Performance
(setq gc-cons-threshold 100000000)
(setq read-process-output-max (* 1024 1024))

;; Auto-completion
(setq tab-always-indent 'complete)

;; Line wrapping
(setq-default fill-column 80)
(setq-default auto-fill-function 'do-auto-fill)

;; Backup settings
(setq backup-directory-alist
      `((".*" . ,(concat user-emacs-directory "backups"))))
(setq auto-save-file-name-transforms
      `((".*" ,(concat user-emacs-directory "auto-save-list") t)))

;; Key bindings
(global-set-key (kbd "C-x C-b") 'ibuffer)
(global-set-key (kbd "C-x g") 'magit-status)
(global-set-key (kbd "C-c C-c") 'comment-or-uncomment-region)
```

## Package Management

### Built-in Package Manager
```elisp
;; Install packages
M-x package-install RET package-name RET

;; List installed packages
M-x package-list-packages

;; Update package list
M-x package-refresh-contents
```

### Use-Package (Recommended)
```elisp
;; Install use-package
(unless (package-installed-p 'use-package)
  (package-refresh-contents)
  (package-install 'use-package))

(require 'use-package)
(setq use-package-always-ensure t)

;; Example package configuration
(use-package magit
  :bind ("C-x g" . magit-status))

(use-package projectile
  :config
  (projectile-mode +1)
  :bind-keymap
  ("C-c p" . projectile-command-map))
```

### Essential Packages for Data Science

#### Magit (Git Interface)
```elisp
(use-package magit
  :bind ("C-x g" . magit-status)
  :config
  (setq magit-completing-read-function 'ivy-completing-read))
```

#### Projectile (Project Management)
```elisp
(use-package projectile
  :config
  (projectile-mode +1)
  :bind-keymap
  ("C-c p" . projectile-command-map))
```

#### Ivy (Completion Framework)
```elisp
(use-package ivy
  :config
  (ivy-mode 1)
  (setq ivy-use-virtual-buffers t)
  (setq enable-recursive-minibuffers t))
```

#### Company (Auto-completion)
```elisp
(use-package company
  :config
  (global-company-mode)
  (setq company-idle-delay 0.1)
  (setq company-minimum-prefix-length 2))
```

## Org-Mode

### Introduction
Org-mode is Emacs' most powerful feature - a system for notes, project planning, and document authoring.

### Basic Org Commands
| Command | Key | Action |
|---------|-----|--------|
| Insert Heading | `M-RET` | Insert new heading |
| Insert Subheading | `M-S-RET` | Insert subheading |
| Toggle Visibility | `TAB` | Cycle visibility |
| Toggle Visibility All | `S-TAB` | Cycle all headings |
| Move Subtree Up | `M-up` | Move heading up |
| Move Subtree Down | `M-down` | Move heading down |
| Promote Subtree | `M-left` | Increase heading level |
| Demote Subtree | `M-right` | Decrease heading level |

### Org Structure
```org
* Main Heading
** Subheading
*** Sub-subheading
- List item
- Another item
  - Nested item

#+BEGIN_SRC python
def hello():
    print("Hello, World!")
#+END_SRC
```

### Task Management
```org
* TODO Write documentation
  DEADLINE: <2024-01-15>
  SCHEDULED: <2024-01-10>
  :PROPERTIES:
  :Effort: 2h
  :END:
```

### Code Blocks
```org
#+NAME: example
#+BEGIN_SRC python :session :results output
import pandas as pd
df = pd.DataFrame({'A': [1, 2, 3], 'B': [4, 5, 6]})
print(df)
#+END_SRC

#+RESULTS: example
   A  B
0  1  4
1  2  5
2  3  6
```

### Export
```elisp
;; Export to various formats
C-c C-e h o    ; Export to HTML and open
C-c C-e l o    ; Export to LaTeX and open
C-c C-e p o    ; Export to PDF and open
C-c C-e m o    ; Export to Markdown and open
```

## Data Science Setup

### Python Development
```elisp
(use-package elpy
  :config
  (elpy-enable)
  (setq elpy-rpc-python-command "python3"))

(use-package python-mode
  :config
  (setq python-shell-interpreter "python3"))

;; Jupyter integration
(use-package ein
  :config
  (setq ein:jupyter-default-server-command "jupyter"))
```

### R Development
```elisp
(use-package ess
  :config
  (setq ess-default-style 'RStudio)
  (setq ess-use-auto-complete t))
```

### Julia Development
```elisp
(use-package julia-mode
  :config
  (setq julia-program "julia"))
```

### LaTeX Support
```elisp
(use-package auctex
  :config
  (setq TeX-auto-save t)
  (setq TeX-parse-self t)
  (setq TeX-save-query nil))

(use-package reftex
  :config
  (reftex-mode 1))
```

### Data Visualization
```elisp
;; CSV mode
(use-package csv-mode)

;; JSON mode
(use-package json-mode)

;; YAML mode
(use-package yaml-mode)
```

## Advanced Features

### Macros
```elisp
;; Start recording macro
F3

;; Stop recording macro
F4

;; Execute macro
F4

;; Execute macro multiple times
C-u 10 F4
```

### Registers
```elisp
;; Save position to register
C-x r SPC a

;; Jump to position in register
C-x r j a

;; Save text to register
C-x r s a

;; Insert text from register
C-x r i a
```

### Bookmarks
```elisp
;; Set bookmark
C-x r m

;; Jump to bookmark
C-x r b

;; List bookmarks
C-x r l
```

### Tramp (Remote Editing)
```elisp
;; Edit remote file
C-x C-f /ssh:user@host:/path/to/file

;; Edit file over SSH
C-x C-f /ssh:user@host:/home/user/file.txt

;; Edit file over Sudo
C-x C-f /sudo::/etc/hosts
```

### Eshell (Integrated Shell)
```elisp
;; Start eshell
M-x eshell

;; Split window and start eshell
C-x 2 M-x eshell
```

## Troubleshooting

### Common Issues

#### Emacs Not Starting
```bash
# Check if emacs is installed
which emacs

# Start emacs in debug mode
emacs --debug-init

# Start emacs without init file
emacs -Q
```

#### Package Issues
```elisp
;; Refresh package list
M-x package-refresh-contents

;; Clear package cache
M-x package-refresh-contents

;; Reinstall package
M-x package-delete RET package-name RET
M-x package-install RET package-name RET
```

#### Performance Issues
```elisp
;; Profile startup time
emacs --profile

;; Check memory usage
M-x memory-report

;; Garbage collect
M-x garbage-collect
```

### Useful Commands
```elisp
;; Help system
C-h ?          ; Help for help
C-h k          ; Describe key
C-h f          ; Describe function
C-h v          ; Describe variable
C-h m          ; Describe mode

;; Info system
C-h i          ; Enter info
C-h C-i        ; Enter info

;; Describe current mode
C-h m          ; Describe current major mode
```

### Recovery
```bash
# Recover auto-save file
M-x recover-file

# List auto-save files
M-x list-auto-save-files

# Recover session
M-x desktop-recover
```

## Resources

- [Emacs Manual](https://www.gnu.org/software/emacs/manual/)
- [Emacs Wiki](https://www.emacswiki.org/)
- [Org Mode Manual](https://orgmode.org/manual/)
- [Emacs Lisp Reference](https://www.gnu.org/software/emacs/manual/elisp.html)
- [Emacs Rocks](http://emacsrocks.com/)
- [Mastering Emacs](https://www.masteringemacs.org/)
- [Emacs Reddit](https://www.reddit.com/r/emacs/) 