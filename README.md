# Claude Menu Bar

<a href="https://github.com/cdbkk/claude-menubar/releases/latest/download/ClaudeMenuBar.dmg"><img src="assets/download.png" alt="Download ClaudeMenuBar.dmg for macOS" width="260"></a>
<br>

A tiny macOS menu bar app that shows **Claude Code's live status**: an animated Claude icon while it's thinking or running a tool, a yellow dot when it's awaiting your permission, and the elapsed time of the current turn. Lightweight, no window, no dock icon, no usage dashboards.

> Built so you can tab away during a long "thinking" stretch and still see, at a glance, whether Claude is working, waiting on you, or done._

> [!IMPORTANT]
> **Multi-session support.** This is built for one active Claude Code session at a time. If you
> run multiple sessions at once (several terminals, or a terminal plus the desktop app), the menu
> bar follows the most recently active one.

---

## What it shows

- **Thinking / working** — the icon animates, with a live `1m 1s` timer.
- **Running a tool** — a short label (`Editing`, `Reading`, `Running command`, `Using tool`, …).
- **Awaiting permission** — a paused yellow dot, in both the CLI and the Desktop app.
- **Idle / done** — rests on the Claude logo.

Everything is controlled from the menu:

- **Show timer:** toggle the elapsed `1m 1s` clock.
- **Play completion sound:** a soft chime when a turn longer than a minute finishes (off by default).
- **Animation style:**
  - **Claude Spark**, the web/chat "morph" spark
  - **Claude Code**, the terminal glyph spinner
  - **Crab Walking**, a pixel-art Clawd crab that scuttles while Claude works
- **Icon color:** **Teal** or **System** (adaptive black/white). The Claude and Claude Code styles follow this setting; Crab Walking is always its orange pixel-art self.
- **Version:** the menu shows your current version. No update checks, no network calls.

## Where it works

| Surface | Tracked? |
|---|---|
| Claude Code CLI (terminal) | ✅ |
| Claude Code Desktop — **Code** tab | ✅ |
| Cursor (Claude Code extension) | ✅ |
| Claude Desktop — **Chat** tab | ❌ |
| **Cowork** | ❌ |

## Requirements

- macOS 12+
- [Claude Code](https://claude.com/claude-code) (CLI or the Desktop app)
- Node.js

## Install

### Option A — DMG (recommended) 

Ad-hoc signed (not notarized). Open it, drag the app to Applications, then **right-click the app → Open** the first time to get past Gatekeeper.

1. Download the latest `ClaudeMenuBar.dmg` from [Releases](../../releases).
2. Open it and drag **Claude Menu Bar** into Applications.
3. Right-click → **Open** once. On first launch it wires up the Claude Code hooks for you automatically.
4. Start a new Claude Code session, the icon appears whenever Claude Code is running.

### Updating

Download the latest DMG and drag it into Applications (choose **Replace**). 
Launch it once, it refreshes its hooks on a version change, then restart Claude Code to pick them up.

### Option B — Claude Code plugin

Installs the hooks (status + open/close lifecycle) automatically from inside Claude Code:

```
/plugin marketplace add cdbkk/claude-menubar
/plugin install claude-menubar@claude-menubar
```

The plugin installs the hooks but not the app itself, so drag **Claude Menu Bar** into Applications once (from the DMG). The plugin launches it automatically on session start.

## How it works

The app is stateless. Claude Code hooks write the current status to `~/.claude/statusbar/state.json`; the app polls that file every 0.4s and renders the icon and label. `SessionStart` launches it; it self-quits once the Claude desktop app is closed and no Claude Code session is active (each active session is a file under `~/.claude/statusbar/sessions.d/`).

The installer merges its hooks into `~/.claude/settings.json` (backing it up first). The app makes **no network calls** and has no servers ([details](docs/privacy.md)).

## Uninstall

```bash
node "/Applications/ClaudeMenuBar.app/Contents/Resources/uninstall.js"   # removes only our hooks
```
Then drag the app to the Trash.

## Trademark / Not Affiliated

This is an unofficial, open-source side project. **It is not affiliated with, endorsed by, or sponsored by Anthropic.** "Claude" and the Claude spark logo are trademarks of Anthropic, used here nominatively. This project is MIT licensed, but that covers the source code only and conveys no rights to Anthropic's trademarks or brand.

This is a free side project, not monetized.

## Credits

A fork of [claude-status-bar](https://github.com/m1ckc3s/claude-status-bar) by Mick Cesanek. Rebranded as Claude Menu Bar with a teal accent and the update check removed (no network calls). Original work MIT licensed.

## License

MIT
