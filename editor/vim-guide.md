# Vim Complete Guide

[![Vim](https://img.shields.io/badge/Vim-019733?style=for-the-badge&logo=vim&logoColor=white)](https://www.vim.org/)
[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](../LICENSE)

Vim is a highly configurable text editor built to enable efficient text editing. It's an improved version of the vi editor distributed with most UNIX systems. Vim is particularly popular among developers and system administrators for its speed, efficiency, and availability on virtually all Unix-like systems.

## Table of Contents

1. [Installation](#installation)
2. [Vim Philosophy](#vim-philosophy)
3. [Modal Editing](#modal-editing)
4. [Basic Navigation](#basic-navigation)
5. [Editing Commands](#editing-commands)
6. [Search and Replace](#search-and-replace)
7. [Configuration](#configuration)
8. [Plugin Management](#plugin-management)
9. [Data Science Setup](#data-science-setup)
10. [Advanced Features](#advanced-features)
11. [Troubleshooting](#troubleshooting)

## Installation

### macOS
```bash
# Using Homebrew
brew install vim

# Using MacPorts
sudo port install vim

# Check if vim is already installed
vim --version
```

### Linux
```bash
# Ubuntu/Debian
sudo apt update
sudo apt install vim

# Fedora/RHEL
sudo dnf install vim

# Arch Linux
sudo pacman -S vim

# Check installation
vim --version
```

### Windows
```bash
# Using Chocolatey
choco install vim

# Using Scoop
scoop install vim

# Download from https://www.vim.org/download.php
```

## Vim Philosophy

### The Vim Way
Vim is built around the philosophy of **modal editing**:
- **Normal mode**: Navigation and commands
- **Insert mode**: Text input
- **Visual mode**: Text selection
- **Command mode**: Ex commands

### Why Vim?
- **Speed**: Minimal keystrokes for complex operations
- **Ubiquity**: Available on all Unix systems
- **Efficiency**: Hands stay on home row
- **Customizability**: Highly configurable
- **Terminal-friendly**: Works in any terminal

## Modal Editing

### Modes Overview

#### Normal Mode (Default)
- Used for navigation and commands
- Press `Esc` to return to normal mode
- Most commands are executed here

#### Insert Mode
- Enter with `i`, `a`, `o`, `O`
- Type text normally
- Exit with `Esc`

#### Visual Mode
- Enter with `v`, `V`, `Ctrl+v`
- Select text for operations
- Exit with `Esc`

#### Command Mode
- Enter with `:`
- Execute ex commands
- Exit with `Enter` or `Esc`

### Mode Transitions
```
Normal Mode ←→ Insert Mode
     ↕
Visual Mode
     ↕
Command Mode
```

## Basic Navigation

### Cursor Movement
| Command | Action |
|---------|--------|
| `h`, `j`, `k`, `l` | Left, Down, Up, Right |
| `w` | Next word |
| `b` | Previous word |
| `e` | End of word |
| `0` | Beginning of line |
| `$` | End of line |
| `^` | First non-blank character |
| `gg` | Beginning of file |
| `G` | End of file |
| `:number` | Go to line number |

### Screen Navigation
| Command | Action |
|---------|--------|
| `Ctrl+f` | Page down |
| `Ctrl+b` | Page up |
| `Ctrl+d` | Half page down |
| `Ctrl+u` | Half page up |
| `H` | Top of screen |
| `M` | Middle of screen |
| `L` | Bottom of screen |
| `zz` | Center current line |

### Jump Commands
| Command | Action |
|---------|--------|
| `Ctrl+o` | Previous location |
| `Ctrl+i` | Next location |
| `%` | Matching bracket |
| `*` | Next occurrence of word under cursor |
| `#` | Previous occurrence of word under cursor |

## Editing Commands

### Insert Commands
| Command | Action |
|---------|--------|
| `i` | Insert before cursor |
| `a` | Insert after cursor |
| `I` | Insert at beginning of line |
| `A` | Insert at end of line |
| `o` | Insert new line below |
| `O` | Insert new line above |
| `s` | Substitute character |
| `S` | Substitute entire line |

### Delete Commands
| Command | Action |
|---------|--------|
| `x` | Delete character under cursor |
| `X` | Delete character before cursor |
| `dw` | Delete word |
| `dd` | Delete line |
| `d$` | Delete to end of line |
| `d0` | Delete to beginning of line |
| `dG` | Delete to end of file |
| `dgg` | Delete to beginning of file |

### Change Commands
| Command | Action |
|---------|--------|
| `cw` | Change word |
| `cc` | Change line |
| `c$` | Change to end of line |
| `C` | Change to end of line |
| `r` | Replace character |
| `R` | Replace mode |

### Copy and Paste
| Command | Action |
|---------|--------|
| `yy` | Yank (copy) line |
| `yw` | Yank word |
| `y$` | Yank to end of line |
| `p` | Paste after cursor |
| `P` | Paste before cursor |
| `"ayy` | Yank to register 'a' |
| `"ap` | Paste from register 'a' |

## Search and Replace

### Search
| Command | Action |
|---------|--------|
| `/pattern` | Search forward |
| `?pattern` | Search backward |
| `n` | Next match |
| `N` | Previous match |
| `*` | Search word under cursor |
| `#` | Search word under cursor backward |

### Replace
```vim
:s/old/new/          " Replace first occurrence in line
:s/old/new/g         " Replace all occurrences in line
:%s/old/new/g        " Replace all occurrences in file
:%s/old/new/gc       " Replace with confirmation
:%s/old/new/gI       " Case-insensitive replace
```

### Global Commands
```vim
:g/pattern/command    " Execute command on lines matching pattern
:g/pattern/d          " Delete lines matching pattern
:g!/pattern/d         " Delete lines not matching pattern
:v/pattern/d          " Same as above
```

## Configuration

### Vimrc File
Create `~/.vimrc` (Unix) or `_vimrc` (Windows):

```vim
" Basic Settings
set nocompatible
set number
set relativenumber
set ruler
set showcmd
set showmode
set incsearch
set hlsearch
set ignorecase
set smartcase
set autoindent
set expandtab
set tabstop=4
set shiftwidth=4
set softtabstop=4
set backspace=indent,eol,start
set history=1000
set wildmenu
set wildmode=list:longest
set visualbell
set noerrorbells
set t_vb=
set mouse=a
set clipboard=unnamed

" File Type Settings
filetype plugin indent on
syntax on

" Color Scheme
set background=dark
colorscheme desert

" Backup Settings
set nobackup
set noswapfile
set nowritebackup

" Status Line
set laststatus=2
set statusline=%F%m%r%h%w\ [FORMAT=%{&ff}]\ [TYPE=%Y]\ [POS=%l,%v][%p%%]\ %{strftime(\"%d/%m/%y\ -\ %H:%M\")}

" Key Mappings
map <C-n> :NERDTreeToggle<CR>
map <C-p> :FZF<CR>
map <C-f> :Ag<CR>
```

### Advanced Settings
```vim
" Performance
set lazyredraw
set ttyfast
set timeoutlen=1000
set ttimeoutlen=0

" Indentation
set smartindent
set cindent
set cinoptions=(0

" Folding
set foldmethod=indent
set foldlevel=99

" Completion
set completeopt=menu,preview
set pumheight=10

" Scrolling
set scrolloff=5
set sidescrolloff=5
```

## Plugin Management

### Vim-Plug Installation
```bash
# Unix
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim

# Windows (PowerShell)
iwr -useb https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim |`
    ni $HOME/vimfiles/autoload/plug.vim -Force
```

### Plugin Configuration
Add to your `.vimrc`:

```vim
" Plugin Management
call plug#begin('~/.vim/plugged')

" File Navigation
Plug 'scrooloose/nerdtree'
Plug 'junegunn/fzf', { 'do': { -> fzf#install() } }
Plug 'junegunn/fzf.vim'
Plug 'ctrlpvim/ctrlp.vim'

" Git Integration
Plug 'tpope/vim-fugitive'
Plug 'airblade/vim-gitgutter'

" Language Support
Plug 'neoclide/coc.nvim', {'branch': 'release'}
Plug 'dense-analysis/ale'
Plug 'sheerun/vim-polyglot'

" Python
Plug 'vim-scripts/indentpython.vim'
Plug 'nvie/vim-flake8'
Plug 'klen/python-mode'

" Data Science
Plug 'jupyter-vim/jupyter-vim'
Plug 'lervag/vimtex'
Plug 'vim-pandoc/vim-pandoc'
Plug 'vim-pandoc/vim-pandoc-syntax'

" UI Enhancements
Plug 'vim-airline/vim-airline'
Plug 'vim-airline/vim-airline-themes'
Plug 'morhetz/gruvbox'
Plug 'tomasiser/vim-code-dark'

" Productivity
Plug 'tpope/vim-surround'
Plug 'tpope/vim-repeat'
Plug 'tpope/vim-commentary'
Plug 'easymotion/vim-easymotion'
Plug 'michaeljsmith/vim-indent-object'

call plug#end()
```

### Essential Plugins for Data Science

#### NERDTree (File Explorer)
```vim
" Configuration
let NERDTreeShowHidden=1
let NERDTreeIgnore=['\.pyc$', '\~$', '__pycache__']
map <C-n> :NERDTreeToggle<CR>
```

#### FZF (Fuzzy Finder)
```vim
" Configuration
let g:fzf_action = {
  \ 'ctrl-t': 'tab split',
  \ 'ctrl-s': 'split',
  \ 'ctrl-v': 'vsplit'
  \ }
```

#### COC (Language Server)
```vim
" Configuration
let g:coc_global_extensions = [
  \ 'coc-python',
  \ 'coc-jedi',
  \ 'coc-json',
  \ 'coc-yaml',
  \ 'coc-markdownlint'
  \ ]
```

## Data Science Setup

### Python Development
```vim
" Python-specific settings
autocmd FileType python setlocal
    \ tabstop=4
    \ softtabstop=4
    \ shiftwidth=4
    \ textwidth=88
    \ expandtab
    \ autoindent
    \ fileformat=unix

" Python syntax highlighting
let python_highlight_all = 1
```

### Jupyter Integration
```vim
" Jupyter-vim configuration
let g:jupyter_mapkeys = 0
let g:jupyter_auto_connect = 1
```

### R Support
```vim
" R configuration
autocmd FileType r setlocal
    \ tabstop=2
    \ softtabstop=2
    \ shiftwidth=2
    \ expandtab
```

### LaTeX Support
```vim
" VimTeX configuration
let g:tex_flavor = 'latex'
let g:vimtex_view_method = 'skim'
let g:vimtex_compiler_method = 'latexmk'
```

## Advanced Features

### Macros
```vim
" Record macro
qa          " Start recording macro 'a'
...commands " Execute commands
q           " Stop recording

" Play macro
@a          " Play macro 'a'
@@          " Play last macro
```

### Registers
```vim
" Named registers
"ayy        " Yank line to register 'a'
"ap         " Paste from register 'a'
:reg        " Show all registers
```

### Marks
```vim
ma          " Set mark 'a' at current position
'a          " Jump to mark 'a'
:marks      " Show all marks
```

### Windows and Tabs
```vim
" Window management
:split      " Split horizontally
:vsplit     " Split vertically
Ctrl+w h/j/k/l " Navigate windows
Ctrl+w =    " Equalize window sizes

" Tab management
:tabnew     " New tab
:tabclose   " Close tab
gt          " Next tab
gT          " Previous tab
```

### Visual Block Mode
```vim
Ctrl+v      " Enter visual block mode
I           " Insert at beginning of block
A           " Insert at end of block
d           " Delete block
c           " Change block
```

## Troubleshooting

### Common Issues

#### Vim Not Starting
```bash
# Check if vim is installed
which vim

# Check vim version
vim --version

# Start vim in safe mode
vim -u NONE
```

#### Plugin Issues
```bash
# Reinstall plugins
vim +PlugClean +PlugInstall +qall

# Update plugins
vim +PlugUpdate +qall
```

#### Performance Issues
```vim
" Disable plugins temporarily
vim -u NONE

" Profile vim startup
vim --startuptime startup.log

" Check for slow plugins
:profile start profile.log
:profile func *
:profile file *
```

### Useful Commands
```vim
:help        " Open help
:help topic  " Help on specific topic
:version     " Show vim version and features
:scriptnames " Show loaded scripts
:set all     " Show all settings
```

### Recovery
```bash
# Recover swap file
vim -r filename

# List swap files
vim -r

# Remove swap file
rm .filename.swp
```

## Resources

- [Vim Documentation](https://vimhelp.org/)
- [Vim Cheat Sheet](https://vim.rtorr.com/)
- [Vim Interactive Tutorial](https://www.openvim.com/)
- [Vim Awesome](https://vimawesome.com/)
- [Vim Tips Wiki](https://vim.fandom.com/wiki/Vim_Tips_Wiki)
- [Learn Vim Progressively](http://yannesposito.com/Scratch/en/blog/Learn-Vim-Progressively/) 