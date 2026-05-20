# AlarmDuration — LMS Plugin

A plugin for [Lyrion Music Server (LMS)](https://lyrion.org/) that adds **per-alarm duration and volume** settings. The built-in LMS alarm system supports only a single global sleep timer and volume level — this plugin lets each alarm have its own.

## Features

- Set a **play duration** (in minutes) for each alarm — overrides the global sleep timer when that alarm fires
- Set a **volume level** for each alarm — applied when the alarm starts
- **Volume is restored** to its previous level when the player stops
- Settings page works in the **classic LMS skin**
- Full **Material Skin integration** — Duration and Volume fields are injected directly into the Add/Edit Alarm dialog, with pre-population when editing existing alarms

## Requirements

- Lyrion Music Server 8.0 or later
- [Material Skin](https://github.com/CDrummond/lms-material) (optional, for dialog integration)

## Installation

### Plugin

1. Copy the `AlarmDuration` folder to your LMS plugins directory:
   - Typical path: `/var/lib/squeezeboxserver/Plugins/AlarmDuration/`
2. Restart LMS.
3. The plugin will appear under **Settings → Plugins** in the LMS web interface.

> **Tip:** You can also point LMS at your plugins folder by adding `--plugindir /path/to/Plugins` to your LMS options (e.g. in `/etc/default/lyrionmusicserver`).

### Material Skin integration (optional but recommended)

The `material-skin/` folder contains two files that enable the Duration and Volume fields inside Material Skin's Add/Edit Alarm dialog.

1. **`custom.js`** — Copy to your Material Skin prefs folder:
   ```
   /var/lib/squeezeboxserver/prefs/material-skin/custom.js
   ```
   > If you already have a `custom.js`, merge the contents rather than replacing it.

2. **`actions.json`** — Copy to the same folder:
   ```
   /var/lib/squeezeboxserver/prefs/material-skin/actions.json
   ```
   This adds an **Alarm Duration & Volume** shortcut to the Material Skin home screen, opening the settings page as an iframe.
   > If you already have an `actions.json`, add the entry from this file into your existing one.

3. Hard-refresh Material Skin in your browser (Cmd+Shift+R / Ctrl+Shift+R) after copying the files — no LMS restart needed.

## Usage

### Via Material Skin dialog (recommended)

After installing `custom.js`, open the alarm list for your player in Material Skin. When you **add or edit an alarm**, two extra fields appear below the Repeat toggle:

- **Duration (mins)** — how long the alarm plays before the player sleeps. Leave empty for no limit.
- **Volume** — the playback volume for this alarm (0–100%). The slider defaults to 50%.

These values are saved automatically when you click **Save** in the alarm dialog.

### Via the settings page

Open **Alarm Duration & Volume** from the Material Skin home screen (or navigate to the plugin settings page in the classic skin). Each alarm is listed with its current Duration and Volume. Edit and click **Save Settings**.

## How it works

- When an alarm fires, the plugin checks for saved duration and volume for that alarm ID.
- If a volume is set, it saves the current volume, then sets the alarm volume.
- If a duration is set, it starts a sleep timer for that number of minutes.
- When the player powers off (sleep timer expires or manual stop), the original volume is restored.

## Security

This plugin is safe for community use. It:
- Makes no external network calls
- Uses only LMS internal Perl APIs
- Validates all user input before saving
- Uses LMS's built-in CSRF protection on the settings page
- Executes no shell commands or external binaries
- Has no third-party dependencies beyond core LMS modules

## License

MIT
