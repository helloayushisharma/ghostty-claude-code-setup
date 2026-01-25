---
name: setup-terminal
description: Auto-detect terminal (Ghostty/Warp/iTerm2) and set up a modern terminal experience with terminal config, zsh enhancements (lsd, autosuggestions, syntax highlighting), and IDE-like vim keybindings
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - WebFetch
argument-hint: "[--ghostty | --warp | --iterm | --zsh-only | --vim-only | --all]"
---

# Terminal Setup Command

Set up a complete modern terminal experience on macOS. Auto-detects your terminal emulator.

## What This Command Does

1. **Auto-detects terminal** (Ghostty, Warp, iTerm2, or Apple Terminal)
2. **Backs up existing configs** (creates .backup files)
3. **Installs packages:**
   - lsd (colorized ls with icons) - direct binary download
   - zsh-autosuggestions (history suggestions)
   - zsh-syntax-highlighting (command coloring)
4. **Configures detected terminal:**
   - **Ghostty**: Catppuccin theme, transparency, IDE keybindings
   - **Warp**: Custom theme, keybindings guide
   - **iTerm2**: Color scheme, key mappings
5. **Configures Zsh:**
   - lsd aliases (ls, ll, la, lr, lt)
   - Colored grep, man pages
   - Autosuggestion keybindings
6. **Configures Vim:**
   - IDE-like keybindings (Cmd+S, Cmd+Z, etc.)
   - Word/line navigation with Option/Cmd arrows
   - Line numbers, mouse support, system clipboard

## Instructions

When the user runs this command:

### Step 0: Auto-detect Terminal

```bash
# Detect which terminal is running
if [[ -n "$GHOSTTY_RESOURCES_DIR" ]]; then
    DETECTED_TERMINAL="ghostty"
elif [[ "$TERM_PROGRAM" == "WarpTerminal" ]]; then
    DETECTED_TERMINAL="warp"
elif [[ "$TERM_PROGRAM" == "iTerm.app" ]]; then
    DETECTED_TERMINAL="iterm"
elif [[ "$TERM_PROGRAM" == "Apple_Terminal" ]]; then
    DETECTED_TERMINAL="terminal"
else
    DETECTED_TERMINAL="unknown"
fi
```

Inform the user which terminal was detected and ask for confirmation.

1. **Parse arguments** to determine scope:
   - `--ghostty-only`: Only configure Ghostty
   - `--zsh-only`: Only configure zsh and install packages
   - `--vim-only`: Only configure vim
   - `--all` or no args: Do everything

2. **Create backups** before modifying any file:
   ```bash
   # For each config file that exists, create a backup
   cp ~/.zshrc ~/.zshrc.backup.$(date +%Y%m%d_%H%M%S)
   cp ~/.vimrc ~/.vimrc.backup.$(date +%Y%m%d_%H%M%S)
   cp "~/Library/Application Support/com.mitchellh.ghostty/config" \
      "~/Library/Application Support/com.mitchellh.ghostty/config.backup.$(date +%Y%m%d_%H%M%S)"
   ```

3. **Install packages:**
   ```bash
   # Install lsd via direct binary (fast, no compile)
   cd /tmp && curl -LO https://github.com/lsd-rs/lsd/releases/download/v1.2.0/lsd-v1.2.0-x86_64-apple-darwin.tar.gz
   tar xzf lsd-v1.2.0-x86_64-apple-darwin.tar.gz
   mv lsd-v1.2.0-x86_64-apple-darwin/lsd /usr/local/bin/
   rm -rf lsd-v1.2.0-x86_64-apple-darwin*

   # Install zsh plugins via brew (these have bottles, instant install)
   brew install zsh-autosuggestions zsh-syntax-highlighting
   ```

4. **Write config files** using the templates from `${CLAUDE_PLUGIN_ROOT}/scripts/`:
   - Read `ghostty-config.txt` and write to Ghostty config location
   - Read `zshrc-additions.txt` and append/merge to ~/.zshrc
   - Read `vimrc.txt` and write to ~/.vimrc

5. **Verify installation:**
   ```bash
   lsd --version
   ls /usr/local/share/zsh-autosuggestions/
   ls /usr/local/share/zsh-syntax-highlighting/
   ```

6. **Inform the user:**
   - Tell them to run `source ~/.zshrc` to apply zsh changes
   - Tell them to restart Ghostty (Cmd+Q) for all changes to take effect
   - Show a summary of what was configured

## Config File Locations

- **Ghostty**: `~/Library/Application Support/com.mitchellh.ghostty/config`
- **Zsh**: `~/.zshrc`
- **Vim**: `~/.vimrc`

## Reference

For detailed configuration options and customization, load the terminal-guide skill.
