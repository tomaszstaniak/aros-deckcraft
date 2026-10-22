# DeckCraft for AROS

An AROS port of [DeckCraft](https://github.com/storytold/deckcraft), a presentation editor. It opens and saves PPTX presentations and exports PDF and images.

## Download and run

[Download v0.8.0-test2](https://github.com/tomaszstaniak/aros-deckcraft/releases/tag/v0.8.0-test2) for **AROS x86_64 ABIv11**, or find `deckcraft` in the apkg catalogue. The download includes DeckCraft and `deckcraft-cli`.

These builds are experimental and not optimised. They use CPU rendering and can be slow, especially in large windows.

Unpack the archive into a drawer and double-click the DeckCraft icon. From a Shell, run `Stack 4000000` before starting `DeckCraft`.

## Known limitations

There is no audio output or printing; export to PDF for printing elsewhere. The clipboard supports plain text, but not pictures or formatting. File drops are unavailable. Japanese, Chinese and Arabic text may appear as boxes, and non-Latin-1 keyboard layouts can produce incorrect characters. Run one instance at a time: simultaneous instances can interfere with AutoRecover files.

## Build from source

This repository contains source revisions, AROS patches and build scripts. See [Building](docs/development.md), [Patch workflow](docs/patching.md) and [Source revisions](upstreams.json). The window and renderer use [egui-aros](https://github.com/tomaszstaniak/egui-aros), pinned in `upstreams.json`.

The repository stores build recipes rather than a full upstream source tree. Licenses and author notices are included in the source and application archives.
