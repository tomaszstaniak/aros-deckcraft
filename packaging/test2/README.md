# DeckCraft 0.8.0 for AROS — ABIv11 test 2

DeckCraft is a presentation program. It opens and saves PowerPoint presentations (PPTX) and its own `.deckcraft` files, and exports PDF, PNG, JPEG, an animated GIF and the outline as text or Markdown. The archive includes two programs for x86_64 AROS:

- **DeckCraft**: the editor, with the slide show.
- **deckcraft-cli**: a command-line tool. It shows a presentation's details, draws slides to PNG, converts between formats, runs editing commands and serves an MCP automation interface over standard input and output.

This is an early test build. The target is AROS x86_64 ABIv11. The interface is drawn by the CPU, so it reacts slowly, more in a large window than in a small one.

## Requirements

- AROS x86_64 ABIv11.
- About 180 MB of disk space. RAM use depends on the document.
- No extra libraries. Both programs use only libraries that come with AROS, and nothing in the system is replaced or added.

## Installation

Unpack the archive into any drawer, for example `Work:`:

    UnZip DeckCraft-0.8.0-test2.x86_64-aros-v11.zip -d Work:

This creates the drawer `Work:DeckCraft` and its icon. To remove DeckCraft, delete that drawer and its icon. DeckCraft keeps its settings and its AutoRecover copies of unsaved presentations in `DeckCraftData` inside its own drawer; set the variable `DECKCRAFT_CONFIG_DIR` to keep them elsewhere.

## Starting the editor

Double-click the **DeckCraft** icon. The icon gives the program the stack it needs (4,000,000 bytes).

From a Shell, set the stack first:

    Stack 4000000
    Work:DeckCraft/DeckCraft
    Work:DeckCraft/DeckCraft "Work:Talks/review.pptx"
    Work:DeckCraft/DeckCraft --sample

Files named on the command line are opened at start; `--sample` opens a sample presentation and `--show` starts the slide show. Open, Save As, Export and Insert Picture use the standard file requester. To close DeckCraft, use the window's close gadget; with unsaved changes it asks first.

## Command line

`deckcraft-cli` sets up the stack it needs itself:

    deckcraft-cli info review.pptx
    deckcraft-cli render review.pptx --slide 0 slide1.png
    deckcraft-cli convert review.pptx review.pdf
    deckcraft-cli help

`convert` writes the format given by the output name's extension (`.deckcraft`, `.pptx`, `.pdf`, `.txt`, `.md`, `.png`, `.jpg`, `.gif`).

`deckcraft-cli mcp` runs an MCP server on standard input and output for an MCP client that starts it as a child process. It works on its own presentation, not on the one open in the DeckCraft window. Use `--automation-read-root` and `--automation-write-root` to limit which drawers it may read and write.

## Known limitations

- Sound and video in presentations play silently: there is no audio output.
- Printing is not available; export to PDF instead.
- Text can be copied and pasted through the clipboard; pictures and formatted text cannot.
- Files dropped on the window are not opened.
- The control port (`--control`) and `deckcraft-cli app`, which drive a running window, are not available.
- Slides use the Latin fonts built into DeckCraft (Inter, Carlito, Liberation and others). Japanese, Chinese and Arabic text is shown with boxes.
- Letters typed with a keymap for another 8-bit charset (such as the Polish one) come out as the wrong characters; text in presentations is shown correctly.
- Two DeckCraft windows started at the same time can take each other's AutoRecover copies; run one at a time.

## Licenses

DeckCraft is licensed under the MIT license or the Apache License 2.0, at your option. The `Licenses` drawer holds these licenses and DeckCraft's asset attributions, the licenses of the fonts built into the programs, the notices for the AROS and Rust libraries linked into them, and the licenses of every Rust package they contain. `SOURCE.txt` names the exact sources; the source archive of this build holds the recipe and every change made for AROS.
