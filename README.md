# Switch Homebrew Skill

[![Codex Skill](https://img.shields.io/badge/Codex-Skill-111827?logo=openai&logoColor=white)](https://github.com/Lili679280/switch-homebrew)
[![libnx](https://img.shields.io/badge/libnx-v4.11.1-2563eb)](https://github.com/switchbrew/libnx/releases/tag/v4.11.1)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

**English** | [简体中文](README.zh-CN.md)

A self-contained Codex skill for Nintendo Switch homebrew development with libnx and devkitPro. It gives Codex versioned, local references for writing and checking C/C++ homebrew code instead of relying on guessed APIs or machine-specific paths.

> This repository is an unofficial community project. It is not affiliated with, sponsored by, or endorsed by Nintendo.

## Quick Start

Clone the repository into your personal Codex skills directory:

```powershell
git clone https://github.com/Lili679280/switch-homebrew.git "$env:USERPROFILE\.codex\skills\switch-homebrew"
```

Restart Codex or start a new task so the skill can be discovered. Then ask for Switch homebrew work normally, for example:

```text
Create a Switch NRO menu with libnx and display the system A/B button glyphs.
```

The skill automatically applies when a request involves Switch homebrew, libnx, devkitA64, NRO development, Joy-Con input, graphics, audio, networking, NFC/Amiibo, or related APIs.

## Why This Skill Exists

Switch homebrew APIs are easy to misremember, and current online documentation may not match the version used by a project. This skill keeps its reference material inside the repository and instructs Codex to verify API signatures against the bundled headers before generating code.

- No hard-coded path to a particular computer
- No network lookup required for the bundled references
- No dependency on an externally changing branch during normal use
- libnx headers and examples remain a known, reproducible reference set

## How It Works

1. `SKILL.md` tells Codex when and how to handle Switch homebrew tasks.
2. `references/api-index.md` directs it to the relevant bundled libnx header.
3. The header under `references/libnx/` is treated as the API source of truth.
4. `references/examples-map.md` directs it to an official bundled example when a usage pattern is needed.
5. Button prompts use the bundled TTF and its mapping guide instead of redrawn artwork.

## What's Included

| Component | Bundled version | Purpose |
|---|---:|---|
| [libnx](https://github.com/switchbrew/libnx) | v4.11.1 release | Authoritative headers, source, and code style |
| [switch-examples](https://github.com/switchbrew/switch-examples) | `669786898205b7beb25ff1731e72982e6d0397d3` | Official usage patterns |
| `NintendoSwitchButtonIcons.ttf` | bundled asset | Switch-style system button prompts |
| API and example indexes | repository version | Fast navigation to the correct bundled files |

The bundled references are intentionally pinned. They do not update automatically, which keeps generated code consistent with the documented version.

## Supported Areas

- NRO application structure and devkitPro Makefiles
- Joy-Con and controller input through the Pad and HID APIs
- Console output, software framebuffer, SDL2, OpenGL, and deko3d
- Audio output and audio renderer APIs
- SD card, RomFS, and save-data access
- BSD sockets, libcurl, and local wireless
- System applets, software keyboard, browser, and user selection
- NFC/Amiibo, system information, battery status, threading, and synchronization

## Button Icon Font

For Switch system button prompts, the skill uses:

```text
assets/fonts/NintendoSwitchButtonIcons.ttf
references/button-icon-font.md
```

The font contains private-use Unicode glyphs. A project must load the TTF through a compatible renderer and use a verified codepoint mapping. Display glyphs and `HidNpadButton_*` input masks are separate concepts; the skill is instructed not to invent mappings.

## Repository Layout

```text
switch-homebrew/
├── SKILL.md
├── README.md
├── README.zh-CN.md
├── assets/
│   └── fonts/
│       ├── NintendoSwitchButtonIcons.ttf
│       ├── LICENSE.txt
│       └── README.md
└── references/
    ├── libnx/
    ├── switch-examples/
    ├── api-index.md
    ├── examples-map.md
    └── button-icon-font.md
```

## Updating

To update an installed copy:

```powershell
Set-Location "$env:USERPROFILE\.codex\skills\switch-homebrew"
git pull
```

Bundled upstream material should only be replaced deliberately. When updating libnx or switch-examples, pin the exact release or commit, retain its license information, and update every version reference in the repository.

## Contributing

Contributions that improve correctness, navigation, or subsystem coverage are welcome. Please read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request. Security-sensitive reports should follow [SECURITY.md](SECURITY.md).

## License and Third-Party Material

Original skill instructions and repository documentation are available under the [MIT License](LICENSE). Bundled third-party works retain their respective terms; see [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) and the license files stored with each component.

Nintendo, Nintendo Switch, Joy-Con, and related names and marks belong to their respective owners. Users are responsible for ensuring that their use and redistribution of bundled assets comply with applicable terms and law.
