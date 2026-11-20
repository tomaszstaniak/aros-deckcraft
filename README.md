# DeckCraft for AROS

An AROS port of [DeckCraft](https://github.com/storytold/deckcraft), a presentation editor. It opens and saves PPTX presentations and its own `.deckcraft` files and exports PDF, images and Markdown.

## Download and run

[Download 0.18.1](https://github.com/tomaszstaniak/aros-deckcraft/releases/tag/v0.18.1) for one of these systems, or install `deckcraft` with arospkg:

| Package | System |
|---|---|
| `DeckCraft-0.18.1-aros-x86_64-abiv11-pc.zip` | AROS x86_64, ABIv11 distributions |
| `DeckCraft-0.18.1-aros-x86_64-abiv1-pc.zip` | AROS x86_64, mainline ABIv1 (nightly 2026-11-06 or later) |
| `DeckCraft-0.18.1-aros-aarch64-abiv1-raspi.zip` | AROS on Raspberry Pi, AArch64 ABIv1 (mainline nightly 2026-11-06 or later) |

Each package runs only on the system it was built for and includes DeckCraft and `deckcraft-cli`. Unpack the archive anywhere writable and double-click the DeckCraft icon; from a Shell, run `DeckCraft [file]`. The program takes the stack it needs itself. `ReadMe.txt` in the drawer describes installation, the command line, the clipboard and the interface renderer.

## Known limitations

Media in presentations plays without sound, and there is no printing (export to PDF instead), no file drops, no formatted clipboard content and no Presenter View on a second display. Changes of the whole window (scrolling, zooming) redraw all of it and follow with a delay on slow machines and in emulators. The Raspberry Pi build was tested only in an emulator. The [release notes](docs/releases/v0.18.1.md) list the rest.

## Build from source

This repository contains source revisions, AROS patches and build scripts. See [Building](docs/development.md), [Patch workflow](docs/patching.md) and [Source revisions](upstreams.json). The window, clipboard and start-up code use [egui-aros](https://github.com/tomaszstaniak/egui-aros), pinned in `upstreams.json`, which also holds the build tools shared by the Craft ports; the Rust standard library for AROS comes from [rust-aros](https://github.com/tomaszstaniak/rust-aros).

The repository holds the build recipe and patches; the source archive of each release adds the pinned upstream inputs. Licenses and author notices are in the source and application archives.
