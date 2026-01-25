---
name: Terminal Guide
description: Use this skill when the user asks about terminal configuration, Ghostty setup, Warp features, zsh customization, vim keybindings, or macOS terminal tips. Trigger phrases include "configure terminal", "ghostty settings", "warp keybindings", "zsh plugins", "vim config", "terminal shortcuts", "lsd colors", "autosuggestions".
version: 1.0.0
---

# Terminal Configuration Guide

Comprehensive guide for setting up a modern terminal experience on macOS with Ghostty, Warp-like features, zsh enhancements, and IDE-like vim keybindings.

## Quick Reference

### Keybindings Across All Apps

| Shortcut | Shell/Terminal | Vim | Action |
|----------|---------------|-----|--------|
| `Option+←/→` | Skip word | Skip word | Word navigation |
| `Cmd+←/→` | Line start/end | Line start/end | Line navigation |
| `Option+Backspace` | Delete word | Delete word | Delete backward |
| `Cmd+D` | Split pane | Duplicate line | Context-specific |
| `Cmd+K` | Clear screen | - | Clear terminal |
| `→` (Right Arrow) | Accept suggestion | - | Autosuggestion |

---

## Ghostty Configuration

### Config Location
- **macOS**: `~/Library/Application Support/com.mitchellh.ghostty/config`
- **Linux**: `~/.config/ghostty/config`

### Essential Settings

```ini
# Theme
theme = dark:Catppuccin Mocha,light:Catppuccin Latte

# macOS Option key as Alt (REQUIRED for keybindings)
macos-option-as-alt = true

# Transparency (set to 1.0 if not working)
background-opacity = 0.85
background-blur = true

# Session persistence
window-save-state = always
quit-after-last-window-closed = false

# Cursor
cursor-style = block
cursor-style-blink = false

# Shell integration
shell-integration = detect
shell-integration-features = no-cursor,sudo,title
```

### Keybindings

```ini
# Navigation
keybind = cmd+right=text:\x05          # End of line
keybind = cmd+left=text:\x01           # Start of line
keybind = alt+right=text:\x1bf         # Forward word
keybind = alt+left=text:\x1bb          # Backward word
keybind = alt+backspace=text:\x17      # Delete word

# Splits
keybind = cmd+d=new_split:right
keybind = cmd+shift+d=new_split:down
keybind = cmd+alt+up=goto_split:top
keybind = cmd+alt+down=goto_split:bottom
keybind = cmd+alt+left=goto_split:left
keybind = cmd+alt+right=goto_split:right

# Tabs
keybind = cmd+t=new_tab
keybind = cmd+w=close_surface
keybind = cmd+1=goto_tab:1
keybind = cmd+2=goto_tab:2
# ... up to cmd+9

# Prompt jumping (requires shell integration)
keybind = cmd+up=jump_to_prompt:-1
keybind = cmd+down=jump_to_prompt:1
```

### Troubleshooting

**Transparency not working:**
1. Exit fullscreen mode (macOS disables transparency in fullscreen)
2. Fully quit Ghostty (Cmd+Q) and reopen
3. Check System Settings > Accessibility > Display > "Reduce transparency" is OFF
4. On macOS 13, some transparency features may be limited

**Option+Arrow not working:**
- Ensure `macos-option-as-alt = true` is set
- Reload config with Cmd+Shift+,

**Tabs not restoring:**
- On macOS 13, tab restoration has known issues
- Fixed in macOS 14+

---

## Warp Terminal

### Config Location
- **Themes**: `~/.warp/themes/`
- **Keybindings**: Settings > Keyboard Shortcuts

### Custom Theme (YAML)

```yaml
name: My Custom Theme
accent: '#268bd2'
cursor: '#95D886'
background: '#002b36'
foreground: '#839496'
details: darker
terminal_colors:
  bright:
    black: '#002b36'
    blue: '#839496'
    cyan: '#93a1a1'
    green: '#586e75'
    magenta: '#6c71c4'
    red: '#cb4b16'
    white: '#fdf6e3'
    yellow: '#657b83'
  normal:
    black: '#073642'
    blue: '#268bd2'
    cyan: '#2aa198'
    green: '#859900'
    magenta: '#d33682'
    red: '#dc322f'
    white: '#eee8d5'
    yellow: '#b58900'
```

### Key Features
- **AI Command Search**: Type `#` to search commands with AI
- **Blocks**: Commands grouped into blocks for easy navigation
- **Workflows**: Save and share command sequences
- **Built-in autosuggestions**: No plugins needed

### Default Keybindings
- `Cmd+D`: Split pane right
- `Cmd+Shift+D`: Split pane down
- `Cmd+[/]`: Navigate between panes
- `Cmd+Enter`: Maximize pane
- `Ctrl+R`: Search command history with AI

---

## Zsh Configuration

### Plugins to Install

```bash
# Via Homebrew (recommended - has pre-built bottles)
brew install zsh-autosuggestions zsh-syntax-highlighting

# Or via direct download
git clone https://github.com/zsh-users/zsh-autosuggestions ~/.zsh/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting ~/.zsh/zsh-syntax-highlighting
```

### ~/.zshrc Additions

```bash
# Autosuggestions
source /usr/local/share/zsh-autosuggestions/zsh-autosuggestions.zsh
# Or for Apple Silicon: /opt/homebrew/share/...

# Accept suggestion keybindings
bindkey '^[[F' autosuggest-accept      # End key
bindkey '^ ' autosuggest-accept        # Ctrl+Space

# Syntax highlighting (MUST be last)
source /usr/local/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

### lsd (Modern ls)

```bash
# Install via direct binary (fast, no compile)
cd /tmp
curl -LO https://github.com/lsd-rs/lsd/releases/download/v1.2.0/lsd-v1.2.0-x86_64-apple-darwin.tar.gz
tar xzf lsd-v1.2.0-x86_64-apple-darwin.tar.gz
mv lsd-v1.2.0-x86_64-apple-darwin/lsd /usr/local/bin/

# Aliases
alias ls='lsd --group-directories-first'
alias ll='lsd -l --group-directories-first'
alias la='lsd -la --group-directories-first'
alias lr='lsd -l --timesort'
alias lt='lsd --tree --depth=2'
```

### Colored Output

```bash
# Enable colors
export CLICOLOR=1
export LSCOLORS=GxFxCxDxBxegedabagaced

# Colored grep
alias grep='grep --color=auto'

# Colored man pages
export LESS_TERMCAP_mb=$'\e[1;32m'
export LESS_TERMCAP_md=$'\e[1;36m'
export LESS_TERMCAP_me=$'\e[0m'
export LESS_TERMCAP_so=$'\e[01;44;33m'
export LESS_TERMCAP_se=$'\e[0m'
export LESS_TERMCAP_us=$'\e[1;4;32m'
export LESS_TERMCAP_ue=$'\e[0m'
```

---

## Vim Configuration

### IDE-like Keybindings for ~/.vimrc

```vim
" General settings
set number relativenumber
set mouse=a
set clipboard=unnamed
set cursorline

" Cursor shape (block normal, line insert)
let &t_SI = "\e[6 q"
let &t_EI = "\e[2 q"

" Option+Left/Right - Word navigation
nmap <Esc>b b
nmap <Esc>f w
imap <Esc>b <C-o>b
imap <Esc>f <C-o>w

" Cmd+Left/Right - Line navigation
nmap <C-a> 0
nmap <C-e> $
imap <C-a> <C-o>0
imap <C-e> <C-o>$

" Cmd+S - Save
nmap <C-s> :w<CR>
imap <C-s> <C-o>:w<CR>

" Cmd+Z - Undo
nmap <C-z> u
imap <C-z> <C-o>u
```

---

## Package Installation

### Fast Install (Direct Binaries)

For macOS systems where Homebrew needs to compile from source:

```bash
# lsd (no dependencies)
curl -LO https://github.com/lsd-rs/lsd/releases/download/v1.2.0/lsd-v1.2.0-x86_64-apple-darwin.tar.gz
tar xzf lsd-v1.2.0-x86_64-apple-darwin.tar.gz
sudo mv lsd-v1.2.0-x86_64-apple-darwin/lsd /usr/local/bin/

# zsh plugins (have bottles, install quickly)
brew install zsh-autosuggestions zsh-syntax-highlighting
```

### Why Brew is Slow for Some Packages

On older macOS versions (13 and below), Homebrew may not have pre-built "bottles" for Rust-based tools like `eza`, `lsd`, `bat`. It then compiles from source, requiring:
- Rust compiler
- LLVM
- Python (for docs)
- Many other dependencies

**Solution**: Download pre-built binaries from GitHub releases.

---

## Terminal Detection

To detect which terminal is running:

```bash
if [[ -n "$GHOSTTY_RESOURCES_DIR" ]]; then
    echo "Ghostty"
elif [[ "$TERM_PROGRAM" == "WarpTerminal" ]]; then
    echo "Warp"
elif [[ "$TERM_PROGRAM" == "iTerm.app" ]]; then
    echo "iTerm2"
elif [[ "$TERM_PROGRAM" == "Apple_Terminal" ]]; then
    echo "Apple Terminal"
fi
```

---

## See Also

- Run `/setup-terminal` to automatically configure everything
- Ghostty docs: https://ghostty.org/docs
- Warp docs: https://docs.warp.dev
- lsd: https://github.com/lsd-rs/lsd
- zsh-autosuggestions: https://github.com/zsh-users/zsh-autosuggestions
