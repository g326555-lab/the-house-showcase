<p align="center"><img src="docs/cover.png" alt="THE HOUSE: A first-person 3D escape-room game in Unity 6. Idea to store build in about four weeks, with no prior game-engine experience." width="100%"></p>

# THE HOUSE

A first-person 3D escape-room game in Unity 6. Idea to store build in about four weeks, with no prior game-engine experience.

**Status:** Testing (Google Play internal test, TestFlight) &nbsp;·&nbsp; **Platforms:** Android · iOS &nbsp;·&nbsp; **Built:** Sep – Oct 2026
<br>[Full case study](https://hyunjunkim.vercel.app/projects/the-house/)

> [!NOTE]
> This repository is a **case study**, not the source code. The code is private. I'm happy to walk through the codebase live in an interview, or to give a hiring team temporary read access on request.

## The problem

Bring the tension of a real escape room to a phone: free 3D look and movement, puzzles you solve by handling objects, cinematics, and a story told across three cases.

## Screens

|   |   |
|:-:|:-:|
| <img src="docs/screens/hero-01-feature-graphic.webp" alt="Store feature graphic" width="100%"> | <img src="docs/screens/scene-01-en.webp" alt="Opening scene" width="100%"> |
| <sub>Store feature graphic</sub> | <sub>Opening scene</sub> |
| <img src="docs/screens/scene-02-en.webp" alt="Stealth sequence" width="100%"> | <img src="docs/screens/scene-04-en.webp" alt="Cinematic: hold your breath" width="100%"> |
| <sub>Stealth sequence</sub> | <sub>Cinematic: hold your breath</sub> |
| <img src="docs/screens/scene-05-en.webp" alt="Puzzle room" width="100%"> | <img src="docs/screens/scene-07-en.webp" alt="Room puzzle: the floor moves" width="100%"> |
| <sub>Puzzle room</sub> | <sub>Room puzzle: the floor moves</sub> |
| <img src="docs/screens/the-house-06.webp" alt="Store listing in four languages" width="100%"> | <img src="docs/screens/scene-08-en.webp" alt="Can you escape?" width="100%"> |
| <sub>Store listing in four languages</sub> | <sub>Can you escape?</sub> |

## Architecture

```mermaid
flowchart LR
    subgraph Runtime["Game runtime · Unity 6 · URP"]
        P["Player<br/>joystick + drag-to-look"]
        IN["Interaction<br/>inspect, combine, use"]
        C["Cinematics"]
        UI["UI<br/>10 panels, one base class"]
        CORE["Core<br/>saves, settings, localization"]
    end
    P --> IN --> C
    IN & C --> UI
    UI & IN --> CORE
    CORE --> IAP["In-app purchases<br/>official store SDKs"]
    subgraph Delivery["Delivery"]
        ADD["Addressables"]
        PAD["Play Asset Delivery"]
    end
    Runtime --> ADD --> PAD
    BUILD["Scripted release builds<br/>symbols and asset packs archived"] --> Delivery
```

## Key decisions

- **Unity 6 with real-time 3D.** Free look and movement instead of fixed panoramas, and native Apple Silicon support for building on a Mac.
- **Split downloads from the start of the second build.** Bundling every asset made the install too large. After checking the official docs, assets moved to Addressables with Play Asset Delivery, so players download only what the current case needs.
- **Every release goes through one script.** Hand-built versions kept missing settings. Building, archiving debug symbols and asset packs, and logging the result are now a single scripted step.
- **Localization is part of done.** English, Japanese, Thai and Traditional Chinese ship together, with the system language picked automatically.

## By the numbers

| | |
|--:|:--|
| **~4 weeks** | Idea to Google Play store build |
| **17,700+** | Lines of game runtime C# (~35,900 total) |
| **10+** | Store builds iterated (latest 1.0.10_27) |
| **4** | Languages, 1,452 strings each, zero missing keys |

## Problems solved along the way

**Install size.** The first builds were too heavy. Simple cleanup didn't fix the root cause; on-demand asset delivery did.

**Objects popping in while turning.** Objects appeared late and vanished when the camera moved. Describing the exact symptom to the AI agents, with the staging intent behind each scene, led to the fix.

## What's next

- Move from manual backups to a private Git repository.
- Split runtime code into Core, Cases and UI modules and inject references once at start.
- Tests for purchase and save logic, then build and tests on every commit.
- Subtitles, font size and color-blind options.

## Stack

`Unity 6` · `C#` · `URP` · `Input System` · `Addressables` · `Play Asset Delivery` · `IL2CPP`

## How it was built

I built this solo, with AI coding agents as my development team. I wrote the scenario, room layouts and puzzles, set the controls and release bar, directed the AI agents through 696 instructions in one working log, and play-tested every build.

More of my work: [portfolio](https://hyunjunkim.vercel.app) · [all case studies](https://github.com/g326555-lab)

---

<sub>© 2026 Hyunjun Kim. All rights reserved. See [LICENSE](LICENSE).</sub>
