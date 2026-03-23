# Krutons Media Controls

System-wide media playback controls for [OpenDeck](https://github.com/nekename/OpenDeck) on Windows. Each button sends the corresponding Windows media key — works with Spotify, Amazon Music, browsers, Windows Media Player, and anything else that responds to system media keys.

---

## Buttons

| Action | What it does | Default colour |
|--------|-------------|----------------|
| **Play / Pause** | Toggle playback | Teal |
| **Next Track** | Skip forward | Purple |
| **Previous Track** | Skip back | Purple |
| **Stop** | Stop playback | Red |
| **Volume Up** | Raise system volume | Orange |
| **Volume Down** | Lower system volume | Orange |
| **Mute** | Toggle mute — turns red when active | Orange |

All buttons are independent — you can mix and match any combination on your layout.

---

## Features

- **Near-instant response** — on first launch the plugin compiles a tiny native key-sender using the .NET Framework compiler built into every Windows install. After that each button press fires in ~5ms with no PowerShell overhead
- **Per-button colour** — each button has its own colour picker
- **Per-button label** — customise the text shown below the icon (up to 6 chars)
- **Mute indicator** — the mute button turns red and shows "MUTED" when active so you always know at a glance
- **Static rendering** — buttons only redraw when something actually changes, keeping CPU usage minimal

---

## Installation

1. Download `krutons-media.sdPlugin.zip` from the [Releases](../../releases) page
2. Extract — you should have a folder called `media.sdPlugin`
3. Copy it to your OpenDeck plugins directory:
   ```
   %APPDATA%\OpenDeck\plugins\
   ```
4. Restart OpenDeck
5. All 7 buttons will appear in the actions list under **Krutons Media Controls**

> **Note:** On first launch the plugin compiles a small helper exe and saves it to `%TEMP%`. This takes a few seconds once and is silent. Subsequent launches are instant.

---

## Customisation

Click any placed button to open its settings panel:

| Setting | Description |
|---------|-------------|
| **Label** | Text shown below the icon. Max 6 characters. Defaults to PLAY, NEXT, etc. |
| **Colour** | Arc/icon colour. Each button is styled independently. |

A **Reset to defaults** button restores the original colour and label for that action.

---

## How it works

On startup the plugin:

1. Writes a 20-line C# file to `%TEMP%`
2. Compiles it to `kruton_key.exe` using `csc.exe` from `.NET Framework 4.x`, which ships with every Windows install at `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\`
3. From then on, every button press calls `spawn("kruton_key.exe", [vkCode])` — detached, fire-and-forget, never blocks the main thread

If `csc.exe` isn't found (very rare), it falls back to a PowerShell script instead.

The virtual key codes used:

| Button | VK Code |
|--------|---------|
| Play / Pause | `0xB3` — `VK_MEDIA_PLAY_PAUSE` |
| Next | `0xB0` — `VK_MEDIA_NEXT_TRACK` |
| Previous | `0xB1` — `VK_MEDIA_PREV_TRACK` |
| Stop | `0xB2` — `VK_MEDIA_STOP` |
| Volume Up | `0xAF` — `VK_VOLUME_UP` |
| Volume Down | `0xAE` — `VK_VOLUME_DOWN` |
| Mute | `0xAD` — `VK_VOLUME_MUTE` |

---

## Requirements

- **OpenDeck** (Windows 10+)
- **.NET Framework 4.x** — pre-installed on all modern Windows machines
- Node.js 20+ (bundled with OpenDeck)

---

## Project structure

```
media.sdPlugin/
├── index.js                      # Plugin backend — key sending, rendering, WebSocket
├── manifest.json                 # OpenDeck plugin manifest
├── images/
│   ├── plugin-icon.png
│   ├── playpause-icon.png
│   ├── next-icon.png
│   ├── prev-icon.png
│   ├── stop-icon.png
│   ├── volup-icon.png
│   ├── voldown-icon.png
│   └── mute-icon.png
├── propertyinspector/
│   └── button.html               # Settings panel (colour + label)
└── node_modules/
    └── ws/                       # WebSocket client
```

---

## Building from source

```bash
git clone https://github.com/kruton/krutons-media-controls
cd krutons-media-controls/media.sdPlugin
npm install
```

Then zip the `media.sdPlugin` folder and install as above.

---

## Credits

- WebSocket via [ws](https://github.com/websockets/ws)
- Built for [OpenDeck](https://github.com/nekename/OpenDeck) by nekename

---

## License

MIT
