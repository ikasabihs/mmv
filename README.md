# mmv - Magic Move/Copy/Delete/Rename

A terminal-based interactive file manager using `fzf` for hierarchical navigation.

---

## Why mmv?

Moving files with a GUI file manager has these problems:

- You need to open two windows and arrange them side by side
- Windows overlap and you can't see the destination
- Files accidentally drop into the wrong folder while dragging
- It consumes too much screen space

**mmv solves all of this — one terminal window, keyboard only.**

---

## Features

- 🗂️ **Hierarchical navigation** — drill down into folders or go back up, step by step
- ⌨️ **Keyboard only** — no mouse needed, navigate with arrow keys
- 📁 **Move or copy folders** — Tab to select a folder itself, Enter to go inside
- ☑️ **Multi-file selection** — Space to select multiple files at once
- ↔️ **Move or Copy** — choose per file/folder with a single keypress (`m` / `c`)
- 🗑️ **Delete** — with confirmation prompt
- ✏️ **Rename** — edit filename inline with current name pre-filled
- ⚠️ **Overwrite protection** — prompts before overwriting existing files
- 🔁 **Continuous operation** — move/copy multiple files in one session
- 🇯🇵 **Japanese filename support** — handles spaces and multibyte characters
- 📦 **Simple install** — single bash script, only dependency is `fzf`

---

## Requirements

- `bash`
- `fzf` — [https://github.com/junegunn/fzf](https://github.com/junegunn/fzf)
- Linux / macOS

### Install fzf

```bash
# Debian / Ubuntu
sudo apt install fzf

# Arch Linux / Manjaro / Mabox
sudo pacman -S fzf

# Fedora / RedHat
sudo dnf install fzf

# openSUSE
sudo zypper install fzf

# Alpine Linux
sudo apk add fzf

# macOS (Homebrew)
brew install fzf

# Universal (works on any distro)
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
```

---

## Installation

```bash
curl -O https://raw.githubusercontent.com/ikasabihs/mmv/main/mmv
chmod +x mmv
sudo cp mmv /usr/local/bin/
```

Or manually:

```bash
chmod +x mmv
sudo cp mmv /usr/local/bin/
```

---

## Usage

```bash
mmv
```

### Controls

| Key | Action |
|-----|--------|
| `↑` `↓` | Move up and down |
| `Enter` | Enter a folder / select a file |
| `Space` | Select multiple files (toggle) |
| `Tab` | Select folder itself as source (not enter) |
| `⬆ .. (上に戻る)` | Go up one level |
| `✅ ここに決定` | Set current folder as destination |
| `m` | Move selected file(s)/folder |
| `c` | Copy selected file(s)/folder |
| `d` | Delete selected file(s)/folder |
| `r` | Rename (single selection only) |
| `q` / `Esc` | Cancel / Quit |

### Flow

```
Launch mmv
    ↓
[Source] Navigate from home directory
         Space  → select multiple files
         Enter  → enter a folder / select a file
         Tab    → select folder itself
    ↓
[m] Move / [c] Copy / [d] Delete / [r] Rename / [q] Cancel
    ↓
[Destination] Navigate to select destination folder
    ↓
Done → back to file selection (Esc to quit)
```

---

## Background

Moving files in the terminal requires typing full paths manually — tedious and error-prone.
GUI file managers take up too much screen space and require dragging between overlapping windows.

By combining `fzf` with hierarchical navigation, mmv lets you move, copy, delete, and rename
files and folders intuitively with just a keyboard, in a single terminal window.

---

## Changelog

### v1.2 (latest)
- Added multi-file selection with Space key
- Added rename function (`r`) with inline editing
- Added delete function (`d`) with confirmation
- Added overwrite protection for move and copy

### v1.1
- Added folder move/copy support (Tab to select folder itself)
- Emoji icons for clearer navigation display

### v1.0
- Initial release
- Interactive file move/copy with fzf
- Hierarchical navigation
- Japanese filename support

---

## License

MIT © ikasabihs
