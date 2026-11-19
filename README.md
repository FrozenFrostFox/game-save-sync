# Game Save Sync

**[Open the generator →](https://frozenfrostfox.github.io/game-save-sync/)** · [Leer en español](README.es.md)

Keep your game saves in sync between your PC, Android phone/handheld and Linux machine, using
**your own Google Drive** — no server, no account with anyone but Google.

You fill in a form (game name, where the saves live, which Drive folder to use) and the page
generates a ready-to-run sync script for your platform. Run that script instead of launching the
game: it pulls the newest save from Drive, launches the game, waits for you to quit, and pushes
your progress back up.

![platforms](https://img.shields.io/badge/platforms-Windows%20%7C%20Android%20%7C%20Linux-6c63ff)
![license](https://img.shields.io/badge/license-GPL--3.0-a78bfa)

## What it does

| Platform | Generated script | How it syncs |
|---|---|---|
| 🪟 **Windows** | Python (`.py`, can be packaged as an `.exe`) | Full autopilot: syncs, launches the game, watches the process, syncs again when it closes, then closes itself. |
| 🤖 **Android** | Bash for [Termux](https://termux.dev) + rclone | One tap from a home-screen widget (Termux:Widget). Auto-detects which side is newer each tap. |
| 🐧 **Linux** | Bash + rclone | Autopilot like Windows if you give it the game's process name; otherwise press Enter when you're done playing. |

- **Newest wins:** each sync compares the local and Drive modification times and copies in the right direction.
- **Folder or file mode:** sync a whole save folder, or just one or more specific save files.
- **Re-import:** lost your settings? Load a script (or a Windows `.exe` the page built) back into the form and it fills everything in again.
- **Termux one-liner:** the Android tab can copy a single command that writes the script straight into Termux.
- **Private by design:** the generator is one static HTML file. Nothing you type leaves your browser; your Google credentials are only remembered in your own browser's local storage.

## Quick start

1. Open the **[generator](https://frozenfrostfox.github.io/game-save-sync/)** (or download `index.html` and open it locally — it works offline).
2. Create the Drive folder that will hold the saves, and copy its link.
3. Pick your platform and follow the one-time setup shown in the sidebar:
   - **Windows:** Python 3.8+, `pip install google-api-python-client google-auth-oauthlib`, and a free Google Cloud OAuth client (`credentials.json`, ~5 minutes — steps are in the sidebar).
   - **Android:** Termux from F-Droid, `termux-setup-storage`, `pkg install rclone coreutils findutils`, then `rclone config` for Google Drive.
   - **Linux:** install rclone and run `rclone config` for Google Drive.
4. Fill in the form, download the script, and launch your game through it from now on.

Want a double-clickable app on Windows? The generator can also give you a `build_<game>_exe.bat`
that installs PyInstaller and builds `sync_<game>.exe` for you.

### Good to know

- On Android 11+ apps can't read each other's private data without root, so native Android game saves only work if the game writes to shared storage. Emulator saves almost always do.
- Downloaded `.exe` files may be blocked by Windows SmartScreen / "Mark of the Web". Right-click → Properties → **Unblock**, or run `Unblock-File` in PowerShell.
- Not sure where a game keeps its saves? [PCGamingWiki](https://www.pcgamingwiki.com) lists the location for most PC games.

## How it was built

A personal project, built iteratively with AI coding assistants (Claude and ChatGPT) and used
day-to-day to keep saves in sync across several Windows PCs and handhelds, an Android device and
Linux. Generator logic was checked with Node.js test harnesses and `bash -n` /
Python syntax checks before each release.

## License

[GPL-3.0](LICENSE) © 2026 FrozenFrostFox. You can use, change and share it; versions you
distribute must stay open source under the same license.
