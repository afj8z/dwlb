<h1>dwlb</h1>

My fork of [dwlb](https://github.com/kolunmi/dwlb), a fast, feature-complete bar for [dwl](https://github.com/djpohly/dwl).

<div align="center">
![screenshot 1](./docs/screenshot1.png "screenshot1")
</div>

## Fork Features

This fork includes minor changes and personalisations:

- Properly renders multi-color glyphs instead of treating them as monochrome masks.
- Replaced and updated deprecated `fcft` scaling filter for high-DPI displays.
- Personalised Defaults:
  - Vacant tags are hidden by default.
  - Adjusted padding and color scheme.
  - Default font set to FiraMono Nerd Font.

## Dependencies

- libwayland-client
- libwayland-cursor
- pixman
- fcft (>= 3.0.0)

## Installation

```bash
git clone [https://github.com/afj8z/dwlb](https://github.com/afj8z/dwlb)
cd dwlb
make
make install
```

## Usage

You can launch `dwlb` directly within the `dwl` command or as a standalone process.

### Basic

Pass `dwlb` as an argument to dwl's `-s` flag.

```bash
dwl -s 'dwlb -font "FiraMono Nerd Font Mono:size=16"'
```

### With Status Monitor & Startup Script

1.  **Launch `dwlb`** in the background with IPC enabled and desired scaling.
2.  **Pipe status info** into `dwlb` using `-status-stdin`.

```bash
#!/bin/sh

# Cleanup previous instances
killall -9 dwlb aslstatus 2> /dev/null

# Processes you want to launch alongside dwl
hypridle &
swww-daemon &
mako &

# Launch dwlb (IPC enabled, scaled for HiDPI)
# Sleep ensures dwlb is ready before accepting status input
(dwlb -ipc -scale 2; sleep 0.2) &

# Pipe aslstatus output to dwlb
aslstatus -s | dwlb -status-stdin all &
```

## IPC

If `dwl` is [patched with IPC](https://codeberg.org/dwl/dwl-patches/src/branch/main/patches/ipc), `dwlb` can communicate directly with it.

- Enable with `-ipc`.
- Clicking tags/layouts works instantly.
- Disable with `-no-ipc`.

## Commands & Status Syntax

Control `dwlb` instances via command line options. Target specific outputs with a name, `all`, or `selected`.

### Inline Formatting

Status text sent to stdin can contain formatting commands: `^cmd(argument)`.

| Command    | Description                                 |
| :--------- | :------------------------------------------ |
| `^fg(HEX)` | Set foreground color (e.g., `^fg(ff0000)`). |
| `^bg(HEX)` | Set background color.                       |
| `^lm(CMD)` | Left click action.                          |
| `^mm(CMD)` | Middle click action.                        |
| `^rm(CMD)` | Right click action.                         |
| `^us(CMD)` | Scroll up action.                           |
| `^ds(CMD)` | Scroll down action.                         |

**Example:**

```bash
dwlb -status all 'text ^bg(ff0000)^lm(foot)Click Me^bg()^lm() text'
```

## Configuration

Edit `config.h` to change compile-time defaults (colors, fonts, tags).

- **Runtime Options:** Run `dwlb -h` for a full list of flags (e.g., `-bottom`, `-hide-vacant-tags`).

## Acknowledgements

- [kolunmi](https://github.com/kolunmi) (Original Author)
- [dtao](https://github.com/djpohly/dtao)
- [somebar](https://sr.ht/~raphi/somebar/)
