# Guest scripts

AmigaDOS scripts for one run on an ABIv11 guest. Put the binaries from a
build output, `tests/fixtures/hello.pptx` and `mcp-in.txt` into one directory,
replace `RUN` with a new run name, unpack the directory to `RAM:dc` in the
guest and `Execute` the script. Results are new `RESULTS:<run>-*` files; the
`.rc` files hold each command's return code.

- `run-cli`: `deckcraft-cli` version, information, conversions, a scripted edit
  saved twice to the same file, and the MCP handshake over stdio.
- `run-shot`: three unattended starts of DeckCraft with `--screenshot` in a row,
  with `Avail FLUSH` before and after each.

In the AROS Shell an asterisk inside quotes is an escape character: a `*` in a
command's text is written `**`.

`tests/fixtures/hello.pptx` was written by `deckcraft-cli run` on the host.
