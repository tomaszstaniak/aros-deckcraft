DeckCraft @VERSION@ for AROS
==========================

DeckCraft is a presentation program. It opens and saves PowerPoint
presentations (PPTX, POTX, PPSX) and its own .deckcraft files, imports text
outlines, and exports PDF, PNG pictures and Markdown. This is its AROS port,
based on DeckCraft @UPSTREAM_VERSION@.

This package is for: @SYSTEM@.
It does not run on other AROS systems; use the package made for yours.

The drawer holds two programs:

  DeckCraft       the presentation editor and slide show
  deckcraft-cli   the command-line program: shows a presentation's slides,
                  renders slides to PNG, converts between formats, runs
                  editing commands and serves an MCP automation interface
                  over standard input and output

Installation
------------

Unpack the archive anywhere writable, for example to Work:. It creates the
drawer DeckCraft and its icon. Both programs use only libraries that come
with AROS; nothing in the system is replaced or added.

To upgrade, unpack the new version to a new place (not over the old drawer),
then move the drawer DeckCraftData from the old DeckCraft drawer into the
new one to keep your settings, recent files and AutoRecover data.

Starting
--------

Workbench: double-click DeckCraft. The program takes the stack it needs
itself; it also works from a Shell with the default stack.

Shell:

  DeckCraft [--gui-renderer CPU|AUTO|GPU] [--window WxH] [file ...]
  DeckCraft --sample
  DeckCraft --show talk.pptx

Files named on the command line are opened at start; --sample opens a
sample presentation and --show starts the slide show. A PowerPoint Show
(.ppsx) named last starts its slide show. Open, Save As, Insert Picture and
Browse for Themes use the standard file requester. To close DeckCraft, use
the window's close gadget; with unsaved changes it asks whether to save
them. Presentations left unsaved when DeckCraft stopped unexpectedly are
offered again at the next start (AutoRecover).

  deckcraft-cli info talk.pptx
  deckcraft-cli render talk.pptx --slide 0 slide1.png
  deckcraft-cli convert talk.pptx talk.pdf
  deckcraft-cli convert talk.pptx talk.md
  deckcraft-cli --help

convert writes the format given by the output name's extension (pptx,
deckcraft, pdf, md). In the AROS Shell an asterisk inside quotes starts an
escape, so write ** for a * in a command's text.

deckcraft-cli mcp runs an MCP server on standard input and output for a
client that starts it as a child process. It works on its own presentation,
not on the one open in the DeckCraft window.

Settings and log
----------------

Settings, recent files, AutoRecover data and the log file DeckCraft.log are
kept in the drawer DeckCraftData inside the DeckCraft drawer; it is created
at the first start. The variable DECKCRAFT_CONFIG_DIR names another drawer
instead. DECKCRAFT_LOG names another log file.

Clipboard
---------

Text is exchanged with other programs through the system clipboard. A
picture on the clipboard (an ILBM written by Capture, MultiView or a paint
program) can be pasted onto a slide with Ctrl+V; this has not been tested
in this release.

Interface renderer
------------------

The ToolType GUI_RENDERER in the program's icon, or the Shell argument
--gui-renderer, chooses how the interface is drawn. A Shell argument takes
priority; the icon is not read when the program is started from the Shell.
Values ignore letter case.

  AUTO  OpenGL if it opens and is not Mesa's software renderer, otherwise
        CPU; the reason is written to DeckCraft.log (default)
  CPU   the processor draws the interface; OpenGL is not opened
  GPU   OpenGL (the software renderer too), or the program stops with an
        error

Known limitations
-----------------

@LIMITATIONS@

Licences
--------

DeckCraft is available under the MIT license or the Apache License 2.0, at
your option. The texts, the licences of the fonts built into the programs,
and the licences of everything linked into the programs are in the Licenses
drawer. BUILD.txt identifies this build.

DeckCraft: https://github.com/storytold/deckcraft
AROS port: https://github.com/tomaszstaniak/aros-deckcraft
