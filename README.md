# mmv - Magic Move/Copy

A terminal-based interactive file mover and copier using `fzf` for hierarchical navigation.

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
- ↔️ **Move or Copy** — choose per file with a single keypress (`m` / `c`)
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

### Flow

```
Launch mmv
    ↓
[Source] Navigate from home directory to select a file
    ↓
[m] Move  /  [c] Copy  /  [q] Cancel
    ↓
[Destination] Navigate from home directory to select a folder
    ↓
Done → back to file selection (Esc to quit)
```

### Controls

| Key | Action |
|-----|--------|
| `↑` `↓` | Move up and down |
| `Enter` | Enter folder / select file / confirm destination |
| `.. (上に戻る)` | Go up one level |
| `ここに決定` | Set current folder as destination |
| `m` | Move the selected file |
| `c` | Copy the selected file |
| `q` / `Esc` | Cancel and go back to file selection |

---

## Background

Moving files in the terminal requires typing full paths manually — tedious and error-prone.
GUI file managers take up too much screen space and require dragging between overlapping windows.

By combining `fzf` with hierarchical navigation, mmv lets you move or copy files intuitively
with just a keyboard, in a single terminal window.

---

## License

MIT © ikasabihs
