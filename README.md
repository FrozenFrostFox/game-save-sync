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
- **Private by design:** the generator is one static HTML file. Nothing you type leaves your browser; see [Privacy](#privacy).

## Quick start

1. Open the **[generator](https://frozenfrostfox.github.io/game-save-sync/)** (or download `index.html` and open it locally — it works offline).
2. Create the Drive folder that will hold the saves, and copy its link.
3. Pick your platform and follow the one-time setup shown in the sidebar:
   - **Everyone:** do the [one-time Google setup](#one-time-setup-your-own-google-sign-in-5-minutes-free) below and paste your `credentials.json` into the page.
   - **Windows:** Python 3.8+ (tick *Add python.exe to PATH* when installing).
   - **Android:** Termux from F-Droid, `termux-setup-storage`, `pkg install rclone coreutils findutils`, then paste the one-line Google Drive setup command the page gives you.
   - **Linux:** install rclone, then paste the one-line Google Drive setup command the page gives you.
4. Fill in the form, get the script, and launch your game through it from now on.

Want a double-clickable app on Windows? Press **Build .exe (copy command)**, open PowerShell in the
folder where you want it, and paste. It installs PyInstaller and builds `sync_<game>.exe` there.
Nothing is downloaded, so Windows doesn't flag anything as unsafe. There's also a
`build_<game>_exe.bat` download, which deletes itself after a successful build by default.

## One-time setup: your own Google sign-in (~5 minutes, free)

The scripts need permission to use your Google Drive. You give it through a free "OAuth client"
that you create once in your own Google account. Nobody else ever sees it.

1. Go to [console.cloud.google.com](https://console.cloud.google.com/) and create a project (e.g. *Cloud Save Sync*).
2. **APIs & Services → Library** → search *Google Drive API* → **Enable**.
3. **Google Auth Platform → Get started**: app name, your email as support email, Audience **External**, your email as contact → **Create**.
4. **Data Access → Add or remove scopes** → paste `https://www.googleapis.com/auth/drive` under *Manually add scopes* → **Add to table** → **Update** → **Save**.
5. **Branding**: fill in *Application home page* and *Application privacy policy link* (this page's URL works for both) and add the site's domain under *Authorized domains* (e.g. `frozenfrostfox.github.io`) → **Save**. Google won't let you publish without these.
6. **Clients → Create client** → Application type **Desktop app** → **Create** → **Download JSON**. Google only shows the secret once, so download it before closing the dialog.
7. **Audience → Publish app → Confirm**. ⚠️ Don't skip this: while the app is in *Testing*, Google signs you out every 7 days.
8. Open the downloaded `client_secret_….json` in Notepad, copy everything, and paste it into the generator (Windows step 5). Do this even if you only use Android or Linux: the page uses it to build your rclone setup command. The page remembers it in your browser only.

The first time a script runs, Google shows *"Google hasn't verified this app"*. That's your own
app, so click **Advanced → Go to (your app name)**. You only do this once per device.

### Good to know

- On Android 11+ apps can't read each other's private data without root, so native Android game saves only work if the game writes to shared storage. Emulator saves almost always do.
- Windows flags downloaded `.bat`/`.exe` files (SmartScreen / "Mark of the Web"). Use the copy-command build to avoid it, or right-click the file → Properties → **Unblock**, or run `Unblock-File` in PowerShell.
- If a script says your Google sign-in expired, it opens the browser to sign in again automatically. If that happens every week, your Google app is still in *Testing*: do step 7 of the setup.
- Not sure where a game keeps its saves? [PCGamingWiki](https://www.pcgamingwiki.com) lists the location for most PC games.

## Privacy

- The generator is a static page. Nothing you type is sent anywhere; your `credentials.json` is kept only in your own browser's local storage.
- The scripts talk only to Google. Your Google sign-in token is saved only on your own device, next to the script. Your saves go only to your own Google Drive.
- There's no server, no analytics and no account with anyone but Google. To remove access, delete the token file next to the script, or remove the app at [myaccount.google.com/permissions](https://myaccount.google.com/permissions).

## How it was built

A personal project, built iteratively with AI coding assistants (Claude and ChatGPT) and used
day-to-day to keep saves in sync across several Windows PCs and handhelds, an Android device and
Linux. Generator logic was checked with Node.js test harnesses and `bash -n` /
Python syntax checks before each release.

## License

[GPL-3.0](LICENSE) © 2026 FrozenFrostFox. You can use, change and share it; versions you
distribute must stay open source under the same license.
