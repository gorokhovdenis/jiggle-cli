# jiggle-cli

Step away from the desk for ten minutes and something, somewhere, notes that you
were idle. This nudges the cursor now and then so that never happens — a small
random move, a smooth glide, back to the exact pixel it started from.

One bash script. Nothing to build, nothing to install.

![macOS](https://img.shields.io/badge/macOS-any-blue)
![bash](https://img.shields.io/badge/bash-3.2%2B-orange)
![License](https://img.shields.io/badge/license-MIT-green)

> Prefer a menu bar app? [**jiggle**](https://github.com/gorokhovdenis/jiggle) is
> the same idea in Swift, with an icon, a menu and no dependencies.

## Install

```sh
curl -O https://raw.githubusercontent.com/gorokhovdenis/jiggle-cli/main/jiggle.sh
chmod +x jiggle.sh
brew install cliclick
```

[`cliclick`](https://github.com/BlueM/cliclick) is the only dependency — it is
what actually moves the cursor.

The terminal you run this from needs Accessibility: **System Settings → Privacy
& Security → Accessibility**, and enable Terminal, iTerm, or whichever you use.
The permission belongs to the terminal, not to the script. Without it the script
says so on startup instead of pretending to work.

## Usage

```sh
./jiggle.sh
```

Every 30–90 seconds it moves the cursor a random distance, smoothly, then
returns it to the exact pixel it started from. `Ctrl-C` stops it.

Configured entirely through the environment:

| Variable | Default | Meaning |
|---|---|---|
| `JIGGLE_MIN` | 30 | minimum pause between jiggles, seconds |
| `JIGGLE_MAX` | 90 | maximum pause, seconds |
| `JIGGLE_DELTA` | 150 | maximum cursor displacement, pixels |
| `JIGGLE_EASE` | 300 | glide smoothness; 0 is an instant jump, higher is slower and more human |
| `JIGGLE_SMART` | 1 | skip a cycle if the mouse was moved by hand |

```sh
JIGGLE_MIN=5 JIGGLE_MAX=10 JIGGLE_DELTA=400 ./jiggle.sh   # frequent and sweeping
JIGGLE_DELTA=4 JIGGLE_EASE=0 ./jiggle.sh                  # barely perceptible
```

## How it behaves

- **Randomized intervals.** A fixed heartbeat every 60 seconds is itself a
  machine-shaped pattern; the pause is random within the range you pick.
- **Glides, and returns exactly.** Not a one-pixel teleport — a smooth arc out
  and back to the starting pixel, so nothing drifts over a long session.
- **Gets out of your way.** With `JIGGLE_SMART=1` (the default), if the cursor
  is not where the script left it, someone is using the Mac and that cycle is
  skipped.
- **Resets `HIDIdleTime`** — the counter the OS and idle-aware software read.
  Measured, not assumed: `8749 ms` before a jiggle, `101 ms` after.

## Limitations

- **This only affects the idle timer.** Software that tracks the foreground
  window, takes screenshots or logs keystrokes is unaffected by a moving cursor.
  If that is what you are up against, this will not help you.
- **Accessibility is per-machine.** Granted once per Mac, by hand, to the
  terminal you run this from.
- **The keyboard is not watched.** The skip heuristic compares cursor position
  only; the [app](https://github.com/gorokhovdenis/jiggle) also skips when a key
  was pressed in the last minute.

## License

[MIT](LICENSE)
