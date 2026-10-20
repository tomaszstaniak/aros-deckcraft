---
type: guide
updated: 2026-10-20
---

# Development

## Current capabilities

A pinned DeckCraft source plus an explicit patch series builds the editor
(`DeckCraft`) and the command line (`deckcraft-cli`, including the MCP server
over stdio) for **x86_64 AROS ABIv11** with a private Rust std toolchain. Both
have been run on an ABIv11 guest (older AROS One 1.3 installation); see "What
has been run" below for the exact scope. Mainline ABIv1 is not built.

## Targets

| Architecture / ABI | Priority | Compile | Link | Runtime |
|---|---|---|---|---|
| x86_64 / ABIv11, older SDK (AROS One 1.3) | Primary | yes | yes | editor and CLI checks pass (see below) |
| x86_64 / ABIv11, 2026.09 SDK | Primary, separate profile | not run | not run | not run |
| x86_64 / mainline v1 | Secondary | not run | not run | not run |

Mainline v1 needs mainline's own Rust cross compiler (`configure --enable-rust`
in the AROS tree: rustc 1.98.1, `panic = abort`, `mmap` and sockets as ENOSYS
stubs) and its crate store; the ABIv11 profile below does not build for it. The
port's own changes are written for `target_os = "aros"` without ABIv11-only
assumptions, and the egui window layer (egui-aros) is built on mainline's
`aros-gui-sys`, but nothing has been compiled for v1 yet.

## Requirements

- An x86_64 ABIv11 GCC 10.5 cross toolchain and the matching SDK. The C glue,
  the patched pthread objects and the Rust link must all use the same SDK:
  ABIv1 and ABIv11 pass `SysBase` differently, and a mismatch crashes inside the
  first system call instead of failing to build.
- A private Rust std toolchain for AROS registered with rustup, reporting
  exactly `rustc 1.100.0-nightly (0dfb098f3 2026-08-31)`, built with the
  rust-aros std patch tooling.
- A rust-aros checkout (target template `x86_64-aros.json.in` and
  `std-groundwork/libc-aros`, at the commit named in `scripts/dcconfig.py`).
- A deadwood2/AROS checkout containing the commit named in
  `scripts/dcconfig.py` (its `compiler/pthread` source is read, never changed).
- An egui-aros checkout containing the commit pinned in `upstreams.json`.
- Python 3.11+, Git, `patch`, and network access once for `cargo fetch`.

## Configuration

`scripts/dcconfig.py` is the one place for paths. Each setting comes from an
environment variable, else the ignored `local.json` (copy
`local.example.json`), else a default. `python3 scripts/dc.py config` shows
where each value comes from; a missing value is reported with the variable that
sets it.

## Build

```sh
python3 scripts/dc.py fetch          # clone/verify upstream/deckcraft and upstream/craft-fonts at their pins
python3 scripts/dc.py materialize    # work/deckcraft = pin + patches/deckcraft/series; never overwrites
CARGO_HOME=$PWD/deps/v11/cargo-home cargo fetch --manifest-path work/deckcraft/Cargo.toml   # once, online
python3 scripts/dc.py build --online # once, for the std sources' own dependencies
python3 scripts/dc.py build          # DeckCraft and deckcraft-cli
python3 scripts/dc.py build -p deckcraft-cli
python3 scripts/dc.py host test -p deckcraft-aros   # host cargo with egui-aros patched in
```

Each build writes `build/v11/out/<time>-<id>/` with the binaries and
`manifest.json`: work-copy commit and dirtiness, upstream pins, crate and
libc-aros patch digests, the pthread objects, the egui-aros vendor record, the
embedded fonts, toolchain identity, the cargo command, binary SHA-256 and a
check that no symbol is left undefined. Builds use the committed work-copy tree
unless `--worktree` is given (recorded as such). Ambient `RUSTFLAGS`,
`CARGO_TARGET_DIR` and similar overrides are refused.

What the build adds around the pinned source:

- egui-aros at its pinned commit, prepared by its own `scripts/ea.py vendor`:
  the window, input, CPU renderer and ASL layer, and its crate patches (winit,
  egui-winit, eframe, glutin, raw-window-handle, aros-gui-sys) as `[patch]`
  entries.
- `patches/crates/`: rayon-core 1.13.0 (`terminate_global_thread_pool`) and
  parking_lot_core 0.9.12 (realtime condvar deadlines on AROS).
- `patches/libc-aros/`: a `memchr` declaration for rust-aros libc-aros.
- `patches/aros-pthread/`: the pthread link library's detach fix. The unpatched
  `compiler/pthread` is first rebuilt and compared with the SDK's
  `libpthread.a`, object for object; then the two patched objects go first on
  the link line.
- The Latin-script fonts of craft-fonts at the commit DeckCraft 0.8.0's release
  workflow embeds (`CRAFT_FONTS_DIR`; Adobe-designed families left out as
  `crates/fonts/build.rs` does). The CJK and Arabic faces (about 80 MB) are not
  embedded, because an AROS executable is loaded into memory whole.
- The unwind panic profile (`-Cpanic=unwind -Cforce-unwind-tables=yes`) by
  default; `DECKCRAFT_PANIC=abort` selects abort.

Never strip the binaries fully; `x86_64-aros-strip --strip-debug` is safe.

## The editor on AROS

`apps/deckcraft-aros` replaces the desktop app's eframe/wgpu window with
egui-aros: an Intuition window on the default public screen, drawn in software.
Open, Save As, Export and Insert Picture/Audio/Video use ASL requesters with the
desktop app's filters; the chosen path is absolute. Text copy and paste use the
system clipboard. Settings and AutoRecover live in `PROGDIR:DeckCraftData` (or
`DECKCRAFT_CONFIG_DIR`). The loop sleeps until input, a repaint the app asks for,
or a wake-up from another thread; there is no idle poll.

Not available, and reported as such: audio output (media plays silently, the
app's behaviour without an audio device), printing, pictures and HTML on the
clipboard, file drops, the control server (`--control` is refused).

The editor needs a large stack: from a Shell, `Stack 4000000` first.
`deckcraft-cli` runs its command on a thread with an 8 MiB stack, and ends
rayon's global pool before it exits.

`DeckCraft --screenshot FILE.png [--screenshot-frames N]` writes the window's
pixels after N frames and quits. `DECKCRAFT_LOG=file` appends the log to a file;
`RUST_LOG=info` or `debug` raises the level (`debug` logs every frame: what woke
the loop and what the frame cost).

## What has been run

On 2026-10-20, ABIv11 guest with the older AROS One 1.3 installation, QEMU TCG,
1024x768 Workbench screen. Guest scripts are in `tests/guest/`.

- CLI: `--version`, `info`, `render` (PNG), `convert` to PDF and Markdown,
  `run` with a new slide saved to PPTX, a second `run` saving over the same
  file, MCP `initialize` and `tools/list` over stdio. Every command returned 0.
- Editor: opening a PPTX (path with a space), typing a slide title, Ctrl+S over
  the opened file, reopening it with the change, Export › Create PDF through
  the ASL requester, the unsaved-changes prompt (Cancel keeps the window, Don't
  Save leaves the file unchanged), resizing, three unattended starts in a row
  with identical screenshots, exit code 0 each time, and the desktop usable
  after closing.
- Idle: with the window untouched, QEMU used 0.37 CPU-seconds per 30 seconds
  (the bare desktop 0.73).

Not run: sound and video, the slide show, transitions, large presentations,
the 2026.09 ABIv11 base, real hardware, mainline v1.

Known platform limits met: `File::try_lock` fails on this profile, so
AutoRecover cannot tell another running DeckCraft's entries from stale ones.
