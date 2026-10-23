# DeckCraft 0.8.0 for AROS — test 4

For AROS x86_64 ABIv11. Experimental and not optimised; the window is drawn by the CPU.

## Changes since test 3

- A document opened from a Shell with a name relative to the current drawer (`DeckCraft talk.pptx`) is now remembered with its full path, so it opens from the Recent list after a start from Workbench or from another drawer. Entries that earlier versions stored with a short name stay as they were and may show as missing; open such a document once through the file requester.
- If DeckCraft cannot lock the drawer it was started from, it now stops at once with return code 20 instead of starting with only a warning. This is a safeguard; no case is known where it happens.

The crash after closing that the test 3 notes describe as fixed was found in PdfCraft. In DeckCraft no way for it to happen is known, so that change is a safeguard too.

## Installation

Unpack the archive into any drawer and double-click the DeckCraft icon. From a Shell, run `Stack 4000000` before starting `DeckCraft`. With apkg: `apkg install deckcraft`.

## Known limitations

- The interface reacts slowly, most in a large window.
- Sound and video play silently; no printing (export to PDF instead).
- No pictures or formatted text through the clipboard, no file drops.
- Japanese, Chinese and Arabic text is shown with boxes.
- Letters typed with a keymap for another 8-bit charset (such as the Polish one) come out as the wrong characters.
- A file name with accented letters typed in a Shell is refused with "argument is not valid text"; use names without them.
