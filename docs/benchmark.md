# Performance: CPU and memory

[🇻🇳 Vietnamese](benchmark.vi.md) · [← Back to the README](../README.md)

AI Over9 runs all day next to your agents, so it has to stay out of their way. These are measured numbers, not
estimates — with the method, so you can measure your own machine.

## Results

Measured on a MacBook Pro (Apple M3 Max, 14 cores, 36 GB), macOS 26.2, AI Over9 0.9.1, while a Claude Code session
was working (the app was receiving its hook events). One sample every 2 seconds, 3 minutes per state (90 samples each).

**CPU** — percent of **one** core (100% = one core fully busy; this Mac has 14):

| State | Mean | Median | 95th percentile | Max |
|---|---|---|---|---|
| **In the background** — menu bar only, window closed | **1.4%** | 0.0% | 6.4% | 15.6% |
| **Main window open** on the Dashboard | **5.0%** | 0.0% | 32.5% | 43.2% |
| Fan helper (root daemon), either state | 0.1% | 0.1% | 0.2% | 0.2% |

**Memory:**

| State | Resident memory (`top`) | Physical footprint (`footprint`) |
|---|---|---|
| In the background | 29–52 MB | 37 MB |
| Main window open | 61–79 MB | 72 MB |
| Fan helper | 4 MB | — |

The background figure was taken right after the app started, while it was loading the usage history; after that
it settles at about 25–30 MB.

**Energy impact** (`top` power column) follows the CPU figures: about 1.4 in the background, 5 with the window open.

### Before and after the 0.9.1 optimisation

The plan card on the Dashboard used to recompute the whole usage history (with calendar math for every record) on
each redraw. It now computes it once per new record or new day.

| Main window open | Before | After |
|---|---|---|
| Mean CPU | 11.0% | **5.0%** (−55%) |
| Median CPU | 8.1% | **0.0%** |
| 95th percentile | 49.5% | 32.5% |
| Physical footprint | 76 MB | 72 MB |

**0.9.3** reads only the temperature sensors that give a value: in the background the mean CPU went from 1.4% to
**1.1%** (median 0%, 95th percentile 5.1%). With the window open it stayed within the run-to-run variation (5–6%).

### Lock screen and AI screen (0.9.14)

Measured on an M-series MacBook (14-inch screen, Retina), drawing one frame off screen with the release build
(`MG_UI_LOCKCARD_BENCH=1`, sample data, two runs with the same result):

| What is drawn | Time per frame |
|---|---|
| The whole full-screen view (backdrop, neural layer, sessions, widgets) | **~24 ms** |
| One clock text | **~0.13 ms** |

**0.9.13** drew the whole view again every second on every display: about 24 ms per second for the MacBook screen,
and roughly 2.5 times that for a 4K display, before the system's own blur and compositing. **0.9.14** redraws only
the clock texts every second (about 1 ms), the neural layer every 6 seconds and the readings every 5 seconds — about
15 to 20 times less drawing. The optional motion (a slow turn of the already drawn layer, a 2-point light under busy
sessions at 12 frames per second) can be turned off in Settings › Lock screen.

With the display off, nothing is drawn at all: the installed app measured 3.1% mean CPU over a minute while locked
(display asleep).

## What this means

- **Most of the time AI Over9 is in the background**, and there it costs about what a clock in the menu bar costs:
  around 1% of one core on average, about 25 MB of memory. Short spikes come from processing agent events, usage
  refreshes and the Telegram poll.
- **The main window costs more,** because live tiles, the usage card and charts redraw while it is open: about 5% of
  one core on average, idle between updates (median 0%), with short peaks around 30–40%. Closing the window when you
  are not looking at it brings it back to the background figure — the app keeps working from the menu bar.
- **The fan helper is negligible**: a few tenths of a percent, 4 MB.
- **Agent hooks never wait for the app:** `aio-hook` hands each event over a local socket and exits; if the app does
  not answer within about 1.5 s it writes the event to a file and the app reads it later.

## Measure it yourself

Find the process ids, then sample every 2 seconds for 3 minutes:

```bash
APP=$(pgrep -x -U "$(id -u)" AIOver9)
HELPER=$(pgrep -f dev.huynt.aiover9.fanhelper)        # only if the fan helper is installed
top -l 91 -s 2 -pid $APP ${HELPER:+-pid $HELPER} -stats pid,cpu,mem,power,threads -o pid
footprint -p $APP | head -2                            # physical memory footprint
```

Ignore the first sample of each `top` run (it has no interval yet). Measure once with the window closed and once with
it open — they differ a lot. Activity Monitor (*CPU* and *Memory* tabs, filter "AIOver9") shows the same values.

**Windows / Linux** (not measured here yet): Task Manager › Details (`AIOver9.exe`), or
`top -p $(pgrep -f AIOver9)` on Linux.

## Limits of these numbers

- One machine, a few minutes per state, measured twice (before and after the optimisation). Slower Macs show higher
  percentages for the same work.
- More agents and sessions mean more events; processing an event takes milliseconds, so the background figure grows
  slowly with activity.
- The Windows and Linux app (a separate code base) has not been measured on real PCs yet.
