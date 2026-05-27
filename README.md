<!--
Pisces Moon OS — Web Emulator
Copyright (C) 2026 Eric Becker / Fluid Fortune
SPDX-License-Identifier: AGPL-3.0-or-later

fluidfortune.com
-->

# Pisces Moon Demo

> *Pisces Moon OS in your browser. No hardware required.*

A self-contained, browser-based emulator for Pisces Moon OS. Boots the launcher, runs the apps, plays the games — all in a single HTML file, with no build step, no install, no dependencies.

**Live:** [piscesdemo.fluidfortune.com](https://piscesdemo.fluidfortune.com)
**Main OS:** [github.com/FluidFortune/PiscesMoon](https://github.com/FluidFortune/PiscesMoon)
**IDE:** [github.com/FluidFortune/lety](https://github.com/FluidFortune/lety)
**License:** AGPL-3.0-or-later

---

## What This Is

Pisces Moon OS is an open-source operating system for ESP32-S3 class hardware — the LilyGO T-Deck Plus, T-LoRa Pager, M5Stack Cardputer ADV, LCDwiki C28P, and similar devices. It's a complete software stack: launcher, multi-device apps, audio, games, voice terminal, RF intelligence tools, and a custom filesystem.

The **Pisces Moon Demo** is a faithful browser-based emulation of that operating system. It exists for three reasons:

1. **You don't have to buy hardware to try it.** Open the page. The launcher boots. Click around.
2. **It's a teaching surface.** Multi-device design is hard to demonstrate with a slide deck. The demo lets you switch between simulated device form factors and see how the same code adapts.
3. **It's a development aid.** When iterating on UI, having a browser tab that boots in 200ms beats waiting 60 seconds for a real device to flash.

This is not a marketing toy. It's the same app code, the same launcher logic, the same audio sequencing, running in JavaScript instead of on Xtensa.

---

## Running Locally

`index.html` is a single self-contained file. To run it:

```bash
open index.html
```

Or serve it through any static web server:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

No dependencies. No build step. No npm install. The entire emulator — UI, app logic, audio synthesis, simulated SD card, simulated touch input — lives in one HTML file with inline JavaScript.

---

## What You Can Try

### Apps that work in the emulator

- **Launcher** — Touch / click tiles, navigate sub-menus, pagination
- **Games** — Tetris, Snake, Pac-Man, Galaga with audio
- **AI terminal** — Type prompts (requires API key bridge in production; emulator simulates responses)
- **Weather** — Live Open-Meteo data, multi-device layout switching
- **RSS reader** — Live feed parsing from BBC, NPR, AP, Hacker News
- **Audio player** — MP3 playback via Web Audio API
- **System settings** — Backlight, sound, storage stats
- **Wardrive UI** — Simulated AP/BLE scan visualization

### Features that are real-hardware-only

These exist in the firmware but not in the emulator:

- **LoRa mesh messaging** (no LoRa radio in browser)
- **Live BLE / WiFi scanning** (the wardrive UI shows mocked data in the emulator)
- **GPS** (no GNSS in browser)
- **Voice recording via microphone** (TTS playback works; capture is hardware-specific)
- **Real SD card I/O** (emulator uses IndexedDB to simulate persistence)
- **NimBLE / nRF24 / SX1262** (all radio peripherals)

The demo is honest about what it can't do — those apps either gray out or display a "real-hardware-only" message.

---

## Device Form Factors

The emulator simulates four device shapes via a top-bar dropdown:

| Device | Resolution | Form Factor |
|---|---|---|
| **T-Deck Plus** | 320×240 landscape | Keyboard + trackball + touch |
| **T-LoRa Pager** | 480×222 landscape | NES buttons + encoder + keyboard |
| **Cardputer ADV** | 240×135 landscape | QWERTY keyboard only |
| **C28P** | 240×320 portrait | Touch only |

Switching devices reloads the launcher with that device's layout. This is the same per-device-layout logic that runs in production — the emulator just routes display + input through DOM events instead of GPIO.

---

## How This Fits Into the Pisces Moon Ecosystem

Pisces Moon is built across multiple repositories that compose into one experience:

```
┌─────────────────────────────────────────────────────────┐
│                  Pisces Moon Ecosystem                   │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  ┌──────────────┐    ┌──────────────┐    ┌────────────┐ │
│  │ PiscesMoon   │    │    lety      │    │  Pisces    │ │
│  │  (firmware)  │◄──►│ (web IDE)    │    │ Moon Demo  │ │
│  │              │    │              │    │ (you here) │ │
│  └──────┬───────┘    └──────────────┘    └────────────┘ │
│         │                                                │
│         ▼                                                │
│  ┌──────────────┐    ┌──────────────┐                   │
│  │ ESP32-S3     │    │  SDL2        │                   │
│  │  devices     │    │  emulator    │                   │
│  └──────────────┘    │  (desktop)   │                   │
│                      └──────────────┘                   │
└─────────────────────────────────────────────────────────┘
```

- **PiscesMoon** — the embedded firmware, the source of truth, runs on real hardware
- **lety** — browser-based IDE that lets users write apps, build them server-side, and flash directly to devices via Web Serial
- **Pisces Moon Demo (this repo)** — standalone emulator, no build step, no backend
- **SDL2 emulator** — desktop emulator for development, separate repo

The demo is the lowest-friction entry point. No account, no flash, no install. Open the page. The OS boots.

---

## File Structure

```
pisces-moon-demo/
├── index.html       # The entire emulator (HTML + CSS + JS inline)
├── README.md        # This file
├── CLA.md           # Contributor License Agreement
└── CNAME            # GitHub Pages custom domain config
```

That's it. One emulator file, one readme, one CLA, one CNAME. Deployment is "push to main, GitHub Pages serves it."

---

## Contributing

We welcome contributions to the demo. The most impactful areas:

### High-value contributions

- **Performance work** — the emulator is currently CPU-heavy on older devices; optimizations welcomed
- **New app emulations** — when a new app ships in PiscesMoon, mirroring it here keeps the demo current
- **Accessibility** — keyboard navigation, screen reader hints, color contrast
- **Mobile touch handling** — the emulator works on mobile but the controls aren't yet ideal
- **Browser compatibility** — currently tested on Chrome, Firefox, Safari. Edge cases welcome.

### Lower-priority but accepted

- **Visual polish** — pixel-accurate device chrome, better animations
- **Sound improvements** — Web Audio synthesis fidelity
- **Documentation** — README contributions, new emulator tutorials

### Not accepted

- **Forks that strip Fluid Fortune branding** without replacing it. The demo is the public face of an active project; misrepresenting authorship causes user confusion.
- **Changes that pull in external dependencies** without strong justification. The "single self-contained file" property is intentional.
- **Modifications to make the emulator emulate features the real hardware doesn't have.** The demo's value depends on its honesty.

### How to contribute

1. Read **CLA.md** in this repository. By submitting a pull request, you agree to its terms.
2. Fork the repo, make your changes, open a PR against `main`.
3. PRs require a clear description of what changed and why. "Improved performance" is fine; "Fixed bug" is not — say what bug.
4. Maintainer review may include style feedback, scope-tightening requests, or alternative approaches.

---

## Reporting Bugs

Open an issue on GitHub. Useful information to include:

- Browser and version
- Operating system
- Device form factor selected in the emulator
- Steps to reproduce
- What you expected vs. what happened
- Screenshot or screen recording if relevant

For bugs that involve actual ESP32-S3 hardware (mismatches between emulator and real device), open the issue in the main PiscesMoon repository instead — the emulator is meant to faithfully reflect the firmware, so a discrepancy usually means the firmware needs the fix, not the emulator.

---

## License

This project is licensed under **AGPL-3.0-or-later**, the same as the rest of the Pisces Moon ecosystem.

What AGPL means in plain English: you can use, modify, and redistribute this software freely, including in commercial settings. If you modify it and let other people use it (including over a network), you must make your modifications available under the same license.

The full license terms apply to the source code in this repository.

For commercial licensing that allows proprietary modifications, contact Fluid Fortune directly via [fluidfortune.com](https://fluidfortune.com).

---

## Acknowledgments

Pisces Moon OS is developed by Eric Becker / Fluid Fortune with AI-assisted development workflows. Contributors, testers, and reviewers across the broader ESP32-S3 community have shaped the project meaningfully.

Specific shouts:

- **The LilyGO, M5Stack, Heltec, and LCDwiki teams** for building the hardware that makes this project possible
- **The KodeDot community** for parallel work on ESP32-S3 handhelds and shared architectural challenges
- **The Anthropic Claude family of models** for being the primary development collaborator
- **Codex** for ongoing code review and bug catches

---

## Voice and Tone

The Pisces Moon project maintains two distinct public voices:

- **GitHub repositories (including this one)** — professional, technical, focused on the work
- **fluidfortune.com** — entertainment and personality, where the Court Jester of Vibe Code lives

Issue comments, pull requests, and repository documentation stay in the professional voice. The playful side has its own home.

---

*For Jennifer.*

🎺
