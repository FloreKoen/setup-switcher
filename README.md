# Setup Switcher

> Switch your Windows display layout and audio devices in one click — or with a global hotkey.

[**↓ Download the latest release**](https://github.com/FloreKoen/setup-switcher/releases/latest)

![Setups grid](docs/screenshot-setups.png)

A **Setup** is a saved bundle of your monitor topology + default audio output + default audio input. Build one for each context you actually switch between — focus mode, racing rig, podcast, work-from-laptop — and jump between them with a global hotkey, the tray menu, or a click.

Each section is independently optional. A "headphones + mic" Setup that leaves your display alone is perfectly fine. So is a "monitor-layout only" Setup that doesn't touch your audio.

## What it does

- **Monitor topology** — which displays are active, where they sit, which is primary, at what resolution.
- **Default audio output** — speakers / headphones / HDMI / DAC, whatever you call default.
- **Default audio input** — mic, including USB headsets and capture devices.

Apply via:

- A **global hotkey** registered system-wide (e.g. `Ctrl+Alt+1`).
- The **tray menu**, which shows every Setup with the currently-active one marked.
- A **click on a card** in the main window.

## Features

- **Snapshot mode** — arrange Windows the way you want, then capture it as a Setup. No fiddly form-filling.
- **Live partial-match hint** — if your current state matches a Setup's displays but not audio, the header tells you ("Currently: Custom — displays match Racing").
- **Single-step revert** — a global "undo last switch" hotkey, or the same option in the tray menu.
- **Update from current state** — re-snapshot an existing Setup in place after rearranging.
- **Auto-apply on launch** — pick a default Setup to apply on every app start.
- **Start at login** — autostart, with optional "start minimized to tray." No UAC prompt at any point.
- **Hotkey conflict detection** — the editor warns when a chord is already used by another Setup or by Revert.
- **Setup reorder** — up/down arrows on each card; the order shows in the tray menu too.
- **Dark / Light / System theme.**

## Editor

![Editor](docs/screenshot-editor.png)

The editor surfaces only the sections this Setup manages. Cleared sections stay cleared — they're treated as "don't touch when applying."

- Monitor diagram previews the saved layout, with the primary monitor highlighted.
- Audio dropdowns mark the currently-default device.
- Dirty-state cancel asks before discarding edits.

## Settings

![Settings](docs/screenshot-settings.png)

The Hotkeys panel doubles as a conflict overview — every binding is listed with a warning icon if two actions share a chord.

## Installing

Two installer formats on every release. Pick whichever:

- **MSI** — standard Windows Installer. Friendly for IT-managed machines and group policy deployment.
- **NSIS exe** — single-file Nullsoft installer. Smaller, quicker.

Either works without admin privileges. WebView2 Runtime is required and is auto-installed on Windows 11; the NSIS installer bundles it for older systems.

## System requirements

- **Windows 10** version 21H1 or later, **x64**.
- **WebView2 Runtime** (ships with Windows 11; bundled by the NSIS installer otherwise).
- No internet connection needed at runtime.

## Quick start

1. Download and run the installer.
2. Open Setup Switcher (it lives in the system tray after install).
3. Arrange your displays and audio defaults in Windows the way you want.
4. Click **Snapshot current setup** in the main window.
5. Give it a name, an emoji, and a hotkey, then **Save**.
6. Rearrange Windows again and snapshot a second Setup. Now press your hotkey.

## How it works

- **Display switching** via the Windows CCD API (`SetDisplayConfig`). Monitor identity is keyed on `monitorDevicePath`, which is stable across reboots, port changes, and reseating monitors — your Setups don't break when you swap a USB-C cable.
- **Audio default switching** via the undocumented `IPolicyConfig` COM interface, the same approach used by SoundSwitch / NirSoft / AudioSwitcher. There is no documented userland API for this on Windows; this trick has been stable since Windows 7.
- **Tray-resident, single-instance** — launching the app a second time surfaces the existing window instead of spawning a duplicate.

## Privacy & telemetry

None. Setup Switcher makes no network requests. State is stored locally in `%APPDATA%\com.fl0re.screen-switcher\store.json` — a plain JSON file you can back up, edit, or carry between machines.

## Project status

Windows-only by design. Source code lives in a separate private repository — releases here are built from it.
