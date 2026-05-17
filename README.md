# mmv - Magic Move/Copy/Delete/Rename

A terminal-based interactive file manager using `fzf` for hierarchical navigation.
Supports network drives (SMB/Samba) for cross-machine file operations.

---

## Why mmv?

Moving files with a GUI file manager has these problems:

- You need to open two windows and arrange them side by side
- Windows overlap and you can't see the destination
- Files accidentally drop into the wrong folder while dragging
- It consumes too much screen space
- Accessing network drives requires mounting manually

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
- 🌐 **Multiple network drives** — connect to several SMB/Samba shares simultaneously
- 🔖 **Bookmarks** — save and reuse network connections, select multiple at once
- 🔁 **Continuous operation** — move/copy multiple files in one session
- 🇯🇵 **Japanese filename support** — handles spaces and multibyte characters
- 🐧 **Cross-distro** — works on any Linux distro and macOS
- 📦 **Simple install** — single bash script, only dependency is `fzf`

---

## Requirements

- `bash`
- `fzf` — [https://github.com/junegunn/fzf](https://github.com/junegunn/fzf)
- `cifs-utils` (Linux only, for network drive support)
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

### Install cifs-utils (for network drive support)

```bash
# Debian / Ubuntu / Lubuntu
sudo apt install cifs-utils

# Arch Linux / Manjaro / Mabox
sudo pacman -S cifs-utils

# Fedora / RedHat
sudo dnf install cifs-utils
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

### Startup menu

```
  mmv - Magic Move/Copy/Delete/Rename
  [n] Connect to network drive
  [s] Start without network
  [q] Quit
```

### Network connection menu

```
  [b] Select from bookmarks (Space to select multiple)
  [n] Enter new connection manually
  [d] Show connected drives
  [q] Done — start mmv
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
| `🌐 name (ip)` | Enter network drive |
| `m` | Move selected file(s)/folder |
| `c` | Copy selected file(s)/folder |
| `d` | Delete selected file(s)/folder |
| `r` | Rename (single selection only) |
| `q` / `Esc` | Cancel / Quit |

### Flow

```
Launch mmv
    ↓
[n] Connect to network drives (optional)
    - [b] Select from bookmarks (Space for multiple)
    - [n] Enter new IP/share/username/password
    - [d] Check connected drives
    - [q] Done, start mmv
    ↓
[Source] Navigate from home directory
         Space  → select multiple files
         Enter  → enter a folder / select a file
         Tab    → select folder itself
    ↓
[m] Move / [c] Copy / [d] Delete / [r] Rename / [q] Cancel
    ↓
[Destination] Navigate — network drives appear at the top
    ↓
Done → back to file selection (Esc to quit)
    ↓
Network drives are automatically unmounted on exit
```

---

## Network drive setup (server side)

To share folders or drives over the network, you need to set up Samba on the host machine (server side).

### Step 1: Install Samba

```bash
# Debian / Ubuntu / Lubuntu
sudo apt install samba

# Arch Linux / Manjaro / Mabox
sudo pacman -S samba

# Fedora / RedHat
sudo dnf install samba
```

### Step 2: Edit /etc/samba/smb.conf

Add share definitions at the bottom of `/etc/samba/smb.conf`:

```bash
sudo nano /etc/samba/smb.conf
```

#### Share a home folder

```
[HOME]
path = /home/yourusername
browseable = yes
writable = yes
guest ok = no
valid users = yourusername
```

#### Share a USB drive or external HDD

First check where the USB drive is mounted:

```bash
lsblk
```

The mount path is typically `/media/yourusername/DRIVE_LABEL`.

Then add to smb.conf:

```
[USBDISK]
path = /media/yourusername/DRIVE_LABEL
browseable = yes
writable = yes
guest ok = no
valid users = yourusername
```

> **Note:** The share name (e.g. `USBDISK`) is what you enter in mmv when connecting.
> It is **not** the full path — just the label in brackets `[ ]`.

#### Share any folder

```
[SHARENAME]
path = /path/to/any/folder
browseable = yes
writable = yes
guest ok = no
valid users = yourusername
```

### Step 3: Set Samba password

```bash
sudo smbpasswd -a yourusername
```

> **Note:** The Samba password is separate from your Linux login password.
> You can set them to be the same or different.

### Step 4: Restart Samba

```bash
sudo systemctl restart smbd
```

### Step 5: Verify

Test the connection locally on the server machine:

```bash
sudo apt install smbclient   # if not installed
smbclient //localhost/SHARENAME -U yourusername
```

If you see `smb: \>` the setup is successful.

---

## Bookmark system

mmv saves frequently used network connections to `~/.mmv_bookmarks` so you don't have to re-enter IP addresses and share names every time.

### Saving a bookmark

When entering a new connection with `[n]`, you will be asked:

```
  この接続先を保存しますか? [y/N]:
```

Press `y` to save. The connection (IP, share name, username) is saved to `~/.mmv_bookmarks`.

> **Note:** Passwords are never saved to the bookmark file for security reasons.
> You will be prompted for a password each time you connect.

### Using bookmarks

Launch mmv and select `[n]` → `[b]`:

```
  [b] Select from bookmarks (Space to select multiple)
```

- Use arrow keys to select a connection
- Press `Space` to select multiple connections at once
- Press `Enter` to confirm
- Enter the password for each connection when prompted

### Connecting to multiple drives at once

You can connect to several network drives simultaneously:

1. Launch mmv → `[n]` → `[b]`
2. Press `Space` to select multiple bookmarks
3. Press `Enter` to confirm
4. Enter password for each connection
5. Press `[q]` to finish connecting and start mmv

All connected drives will appear at the top of the destination list as `🌐 SHARENAME (IP)`.

### Managing bookmarks

Bookmarks are stored as plain text in `~/.mmv_bookmarks`:

```
192.168.1.100|HOME|username
192.168.1.100|USBDISK|username
192.168.1.200|SHARENAME|otheruser
```

To edit or delete bookmarks manually:

```bash
nano ~/.mmv_bookmarks
```

---

## Connecting from client side (mmv)

Once the server is set up, launch mmv on the client machine:

```bash
mmv
```

Select `[n]` → `[n]` and enter:

- **IP address**: the server's IP (e.g. `192.168.1.100`)
- **Share name**: the name in brackets from smb.conf (e.g. `HOME`, `USBDISK`)
- **Username**: your username on the server
- **Password**: your Samba password

The network drive will appear at the top of the destination list as `🌐 SHARENAME (IP)`.

---

## Background

Moving files in the terminal requires typing full paths manually — tedious and error-prone.
GUI file managers take up too much screen space and require dragging between overlapping windows.
Accessing network drives usually means mounting manually before starting work.

mmv combines `fzf` hierarchical navigation with automatic SMB mounting, letting you move,
copy, delete, and rename files across local and network storage — all from a single terminal
window, keyboard only.

---

## Changelog

### v1.3 (latest)
- Added network drive (SMB/Samba) support
- Added bookmark system for saving network connections
- Multiple network drives can be connected simultaneously
- Bookmarks support multi-select with Space key
- Password prompt added for secure authentication
- Network drives appear at the top of destination list automatically
- Auto-unmount on exit
- macOS support for SMB mounting

### v1.2
- Added multi-file selection with Space key
- Added rename function (`r`) with inline editing
- Added delete function (`d`) with confirmation
- Added overwrite protection for move and copy

### v1.1
- Added folder move/copy support (Tab to select folder itself)
- Hierarchical navigation for both source and destination
- Emoji icons for clearer navigation display

### v1.0
- Initial release
- Interactive file move/copy with fzf
- Japanese filename support

---

## License

MIT © ikasabihs
