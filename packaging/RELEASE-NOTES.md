# DeckCraft 0.18.1 for AROS

The AROS port of DeckCraft 0.18.1, a presentation program. It opens and saves PowerPoint presentations (PPTX) and its own `.deckcraft` files and exports PDF, PNG pictures and Markdown. It is built from the DeckCraft 0.18.1 release (tag `v0.18.1`), for three AROS systems.

## Downloads

| File | System |
|---|---|
| `DeckCraft-0.18.1-aros-x86_64-abiv11-pc.zip` | AROS x86_64, ABIv11 distributions |
| `DeckCraft-0.18.1-aros-x86_64-abiv1-pc.zip` | AROS x86_64, mainline ABIv1 (nightly 2026-11-06 or later) |
| `DeckCraft-0.18.1-aros-aarch64-abiv1-raspi.zip` | AROS on Raspberry Pi, AArch64 ABIv1 (mainline nightly 2026-11-06 or later) |
| `DeckCraft-0.18.1-source.zip` | Port sources, patches, pinned inputs and build recipe |

Each package runs only on the system it was built for. `SHA256SUMS` lists the checksums. The packages can also be installed with the AROS package manager: `apkg install deckcraft`.

## What is new

This release moves from DeckCraft 0.8.0 (test 4) to DeckCraft 0.18.1. For the changes in the program itself, see the release notes of DeckCraft upstream. On the AROS side:

- Packages for mainline x86_64 and the Raspberry Pi, besides ABIv11 distributions.
- The window is drawn much faster with the CPU: only the parts that changed are drawn again. In the emulators used for testing, a typical frame takes 0.02 to 0.03 s.
- OpenGL can draw the window. The `GUI_RENDERER` ToolType in the icon, or `--gui-renderer` in a Shell, chooses `AUTO` (the default), `CPU` or `GPU`, as in the other Craft programs. `AUTO` uses OpenGL only when it is not Mesa's software renderer.
- The program takes the stack it needs itself; `Stack` is no longer needed before a Shell start.
- Pictures on the clipboard can be pasted onto a slide.
- Settings, AutoRecover data and the log file `DeckCraft.log` are kept in `DeckCraftData` in the program drawer. Recent files saved by 0.8.0 are kept.

With the package manager: `apkg upgrade deckcraft`.

## Installation

Unpack the archive for your system anywhere writable and double-click DeckCraft. From a Shell, run `DeckCraft [--gui-renderer CPU|AUTO|GPU] [file]`. To upgrade by hand, unpack the new version to a new place and move `DeckCraftData` into it. The archive also holds a `.arospkg` drawer with the arospkg description; after unpacking by hand it can be deleted.

`DeckCraft --version` and the first line of `DeckCraft.log` name the build (`0.18.1` and the target); please include them when you report a problem.

Tested on an ABIv11 distribution (AROS One 1.3), on mainline x86_64 AROS nightly 2026-11-15 and on the Raspberry Pi nightly 2026-11-10 in an emulator, not on a physical Raspberry Pi. On all three: the command-line program (information, a slide rendered to PNG, PDF and Markdown, an edit saved twice to one file, the MCP handshake), two starts of the window, and a scripted session from a Shell (opening a PPTX presentation, a new slide, Save As, undo and redo, Save) whose file was read back; then the package installed, used and removed with the package manager. On ABIv11 the package manager upgraded an installed 0.8.0 (test 4) and kept a user file in `DeckCraftData`. `GPU` was tried on ABIv11 (Mesa's software renderer) and on mainline x86_64 with VirGL in an emulator; both drew correctly, slowly. Not tested: Workbench start, the file requesters, the clipboard, typing with keymaps other than US, the slide show and video.

## Known limitations

- Media in presentations plays without sound: there is no audio output.
- No printing: export to PDF instead. Files dropped on the window are not opened, and Presenter View needs a second display, which is not offered.
- Formatted (HTML) clipboard content is not available; copy and paste use plain text and pictures.
- Changes of the whole window (scrolling, zooming, opening a dialog) redraw all of it; on slow machines and in emulators these follow with a delay.
- Run one DeckCraft at a time: AutoRecover cannot tell that another DeckCraft is running and may take over its recovery data.
- Only Latin-script fonts are built in. Chinese, Japanese, Korean, Arabic and emoji characters are kept in presentations but shown as boxes.
- The slide show, transitions, video, pasting a picture from the clipboard, the file requesters and keyboard layouts other than the US one have not been tested on AROS in this release.
- No automatic updates: install new versions with the AROS package manager or by unpacking the new archive. The MCP server (deckcraft-cli mcp) has been tested only for its handshake on AROS.
- ABIv11: AROS One 1.3 has only Mesa's software renderer, so `AUTO` uses the CPU there. `GPU` works with it but draws a frame in seconds.
- Mainline x86_64: `AUTO` uses the CPU, because a program that has opened OpenGL there leaves its log file in use after it ends and the Shell it ran in stops redirecting output (seen on nightly 2026-11-10). `GPU` still opens OpenGL; with VirGL in an emulator it drew correctly but took about ten seconds a frame. Hardware OpenGL drivers have not been tested.
- Raspberry Pi: tested in an emulator only. OpenGL has not been tested on Pi hardware, so `AUTO` uses the CPU; save your work before you try `GPU`.

Licences and notices for DeckCraft, the fonts built into it and everything else linked into it are in the `Licenses` drawer of each package.
