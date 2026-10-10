# GDBot — a farming bot for Grim Dawn on Linux

GDBot plays Grim Dawn for you while you watch: it clears the area around your character, fights, drinks
potions, picks up loot and (optionally) goes to town to stash and sell. You control it from a small menu
**inside the game** — press **F8** to open it.

> Version 0.1 — early release. Expect rough edges and please report what goes wrong (see [Contact](#contact)).

## What it does (v0.1)

| Menu item | What happens |
|---|---|
| **Clear the area** | Kills every enemy around you, explores what it hasn't seen, loots, then walks on to the next area. |
| **Assistant** | You walk; the bot shoots whatever is in range and drinks potions for you. |
| **Settings** | Sell when the bag is full · Fight bosses · Save screenshots to report problems · Language (English / Português). |

## Requirements

- **Ubuntu 24.04, Pop!_OS 24.04** or another distro based on Ubuntu 24.04 (amd64).
- A **Wayland** desktop (tested on COSMIC; GNOME and KDE should work too).
- Grim Dawn on **Steam with Proton**.
- Game settings (the installer checks them for you):
  - Fullscreen, **1920×1080**
  - Interface scale (**uiScale**) **0.5**
  - Language **English**
  - Monster health bars **on**, including on undamaged monsters
  - Auto-loot radius at **maximum**
- Any character, any class. Your skill bar and key bindings are read from the game.

## Install

1. Download `gdbot_0.1.0_amd64.deb` from the [Releases](https://github.com/nemonox/gdbot/releases) page.
2. Open a terminal in the folder where you saved it and run:

   ```bash
   sudo apt install ./gdbot_0.1.0_amd64.deb
   ```

3. Run the setup once (it finds the game, checks your game settings and your system):

   ```bash
   gdbot setup
   ```

4. Log out and log back in (so the new keyboard/mouse permissions apply).

To uninstall: `sudo apt remove gdbot`. Your settings and maps stay in `~/.local/share/gdbot` and
`~/.config/gdbot` — delete those folders to remove everything.

## How to use

1. Start Grim Dawn and load your character.
2. Open **GDBot** from your applications menu. The bot menu shows up at the top right of the game.
3. **↓ / ↑** move, **F9** chooses. **F8** opens the menu again after it closes.
4. While a mode is running: **F9** stops it, **F10** pauses and resumes.
5. To turn the bot off, choose **Turn off the bot menu** or click GDBot again.

### The bot does NOT run in the background

GDBot is not a headless or background bot. It **moves your real mouse and presses real keys** in the game
window, exactly like a person would. While it is running:

- the game must stay **open, in fullscreen and in focus**;
- you can't use your computer for other things (Alt+Tab away and the bot clicks on whatever is on screen —
  pause it with **F10** first);
- the screen must stay on (no screen lock).

It also can't run with the game minimized or on another workspace: it needs to *see* the game (next section).

## Why it asks to share your screen

The first time you start a mode, your desktop shows a **"Share your screen"** dialog. Choose your monitor and
allow it. This is the normal Wayland permission any app needs to see the screen — the same one used by OBS,
Discord or video calls.

The bot needs it because it **looks at the game the same way you do**: health and energy orbs, item labels
on the ground, the town windows (stash, merchant), menus and dialogs. The images are processed **in memory,
on your computer, a few times per second, and thrown away**. Nothing is recorded, saved or sent anywhere
(the only exception is the "Save screenshots" option in Settings, which saves screenshots to your own disk so
you can attach them to a bug report). The permission is remembered, so it asks only once.

## What else it touches — and what it doesn't

- **Game memory, read-only.** To know exactly where your character and the monsters are, the bot *reads* the
  game's memory. It never writes to it and never injects code into the game. The package installs a tiny
  helper (`/usr/lib/gdbot/gdbot-memoria`) that is only allowed to open the memory of the "Grim Dawn.exe"
  process owned by **you**, read-only.
- **Virtual mouse and keyboard** (`/dev/uinput`) — how it clicks and presses keys. A rule in
  `/usr/lib/udev/rules.d/70-gdbot.rules` gives access only to the user logged in at the computer.
- **Your keyboard keys F8–F11** are read so you can control the bot from inside the game.
- **Game files, read-only** (database, maps, key bindings) — names of items and areas, where the walls are.
- **No internet** by default. The only exception is opt-in: during `gdbot setup` you may enter an API key
  for TypeSafe (an AI service) to rate items instead of the built-in rules — then the *descriptions of your
  items* are sent to it. Just press Enter to skip it; the built-in rules work offline.

## Play responsibly

Use it in **single player / offline**. Using bots in multiplayer games is unfair to other players. Use at your
own risk.

## Troubleshooting

- Log of the last run: `~/.local/share/gdbot/.estado/ultimo.log`
- Log of the menu: `~/.local/share/gdbot/.estado/central.log`
- Check your setup again: `gdbot setup`

The bot's own log lines are in Portuguese for now (the menu and the panel follow the Language setting).

When reporting a problem, turn on **Save screenshots to report problems** in Settings,
reproduce it, and send the log plus a few screenshots from `~/.local/share/gdbot/.estado/debug/`.

## Contact

Questions, bugs and ideas: **nemonox.br@gmail.com** or open an
[issue](https://github.com/nemonox/gdbot/issues).

---

GDBot is free to download and use. It is not affiliated with or endorsed by Crate Entertainment.
Grim Dawn is a trademark of Crate Entertainment.
