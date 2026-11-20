# Guest scripts

AmigaDOS scripts for one run on an AROS guest. Put the stripped programs from a
build output, `tests/fixtures/hello.pptx`, `mcp-in.txt`, `steps.json` and the
scripts into one directory, unpack it to `RAM:dc` in the guest and `Execute`
a script with a new, existing results directory as its argument. The `.rc`
files there hold each command's return code.

- `run-smoke`: `deckcraft-cli` version, information, a slide rendered to PNG,
  PDF and Markdown, a scripted edit saved twice to the same file and the MCP
  handshake over stdio; then DeckCraft's version, two unattended starts with
  `--screenshot`, and the scripted session of `steps.json` (a new slide,
  Save As, undo, redo, Save, quit), whose file is read back by the CLI.
- `run-gpu`: the screenshot start and the scripted session with
  `--gui-renderer GPU` (OpenGL forced).

In the AROS Shell an asterisk inside quotes is an escape character: a `*` in a
command's text is written `**`.

`tests/fixtures/hello.pptx` was written by `deckcraft-cli run` on the host.
