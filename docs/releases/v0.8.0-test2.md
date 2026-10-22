# DeckCraft 0.8.0 for AROS — test 2

For AROS x86_64 ABIv11. Experimental and not optimised; the window is drawn by the CPU.

## Changes since test 1

- Fixed a possible crash after closing DeckCraft when it was started from Workbench. A background thread could make the C library release the starting drawer's lock that Workbench releases again on exit; the program now keeps that lock for itself.
- The programs are built with whole-program optimisation and stripped of unneeded symbols, so they are smaller.
- DeckCraft is available as `deckcraft` in the apkg catalogue. It installs with apkg 0.5.0 and later.

## Installation

Unpack the archive into any drawer and double-click the DeckCraft icon. From a Shell, run `Stack 4000000` before starting `DeckCraft`. With apkg: `apkg install deckcraft`.

## Known limitations

- The interface reacts slowly, most in a large window.
- Sound and video play silently; no printing (export to PDF instead).
- No pictures or formatted text through the clipboard, no file drops.
- Japanese, Chinese and Arabic text is shown with boxes.
- Letters typed with a keymap for another 8-bit charset (such as the Polish one) come out as the wrong characters.
