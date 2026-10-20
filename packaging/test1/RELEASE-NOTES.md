# DeckCraft 0.8.0 for AROS — test 1

First test build for x86_64 AROS ABIv11.

## What is included

- The DeckCraft editor in a window on the Workbench screen: slides, layouts,
  themes, text, shapes, tables, pictures, notes, animations and the slide show.
- Opening and saving PowerPoint (PPTX) and `.deckcraft` files; export to PDF,
  images, an animated GIF and the outline.
- `deckcraft-cli` for conversions, rendering, scripted edits and MCP automation.
- The standard AROS file requester for Open, Save As, Export and Insert Picture.

## Installation

Unpack the archive into any drawer and double-click the DeckCraft icon. From a
Shell, run `Stack 4000000` before starting the editor.

## Known limitations

- The interface reacts slowly, most in a large window.
- Sound and video play silently; no printing (export to PDF instead).
- No pictures or formatted text through the clipboard, no file drops.
- Japanese, Chinese and Arabic text is shown with boxes.
- Letters typed with a keymap for another 8-bit charset (such as the Polish
  one) come out as the wrong characters.
