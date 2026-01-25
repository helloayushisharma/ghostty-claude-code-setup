# Ghostty Claude Code Setup

A Claude Code plugin to set up a modern terminal experience on macOS with Warp-like features.

## Features

- **Auto-detects terminal** (Ghostty, Warp, iTerm2, Apple Terminal)
- **Ghostty configuration**: Catppuccin theme, transparency, IDE keybindings, session persistence
- **Zsh enhancements**: lsd (colorized ls), autosuggestions, syntax highlighting
- **Vim configuration**: IDE-like keybindings (Cmd+S, Cmd+Z, word navigation)
- **Automatic backups** before modifying any config files

## Installation

### Option 1: Clone from GitHub
```bash
git clone https://github.com/awwsillylife/ghostty-claude-code-setup.git ~/.claude/plugins/ghostty-claude-code-setup
```

### Option 2: Use directly with Claude Code
```bash
claude --plugin-dir /path/to/ghostty-claude-code-setup
```

### Option 3: Copy to project
Copy this folder to your project's `.claude-plugin/` directory.

## Usage

### Command: `/setup-terminal`

```bash
/setup-terminal              # Set up everything (auto-detect terminal)
/setup-terminal --ghostty    # Only configure Ghostty
/setup-terminal --zsh-only   # Only configure zsh and install packages
/setup-terminal --vim-only   # Only configure vim
```

### Skill: Terminal Guide

Ask questions like:
- "How do I configure Ghostty keybindings?"
- "What are the zsh autosuggestion shortcuts?"
- "How do I set up vim with IDE keybindings?"

## What Gets Installed

### Packages
- **lsd** v1.2.0 - Modern ls replacement with colors and icons
- **zsh-autosuggestions** - Fish-like command suggestions
- **zsh-syntax-highlighting** - Commands colored as you type

### Keybindings

| Shortcut | Action |
|----------|--------|
| `Option+←/→` | Skip words |
| `Cmd+←/→` | Line start/end |
| `Option+Backspace` | Delete word backward |
| `Cmd+D` | Split pane (terminal) / Duplicate line (vim) |
| `Cmd+K` | Clear screen |
| `→` (Right Arrow) | Accept autosuggestion |
| `Cmd+Up/Down` | Jump between prompts |

### Config Locations

- **Ghostty**: `~/Library/Application Support/com.mitchellh.ghostty/config`
- **Zsh**: `~/.zshrc`
- **Vim**: `~/.vimrc`

## Session Persistence

Ghostty is configured with:
```ini
window-save-state = always
quit-after-last-window-closed = false
```

This means:
- All tabs and windows are saved when you quit
- Reopening Ghostty restores your previous session
- Working directories are preserved (requires shell integration)

**Note**: On macOS 13 (Ventura), tab restoration may have limitations. This is fixed in macOS 14+.

## Troubleshooting

### Transparency not working
1. Make sure you're not in fullscreen mode
2. Fully quit Ghostty (Cmd+Q) and reopen
3. Check System Settings > Accessibility > Display > "Reduce transparency" is OFF

### Option+Arrow not working
- Ensure `macos-option-as-alt = true` is in your Ghostty config
- Reload config with Cmd+Shift+,

### Brew install is slow
On macOS 13 and below, Homebrew may compile Rust tools from source. This plugin uses direct binary downloads to avoid this.

## Author

**Ayushi Sharma** - [@awwsillylife](https://github.com/awwsillylife)

## License

MIT
