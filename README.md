# NEO VELOCITY

An F-Zero style anti-gravity racer in a single HTML file, built on [three.js](https://threejs.org/).
No build step, no dependencies to install — open `index.html` and race.

**▶ Play: https://madd1in.github.io/neo-velocity/**

*(The in-game UI is German.)*

---

## Features

**Circuits** — three procedurally generated closed circuits, each built from a harmonic
radius/elevation profile, arc-length sampled into 1600 frames with automatically derived
banking, variable track width, boost pads and a pit-lane recharge strip:

| Track | Character | Length |
|---|---|---|
| MUTE LOOP | flowing, wide | ~7.2 km |
| SERPENT VALLEY | technical, narrow, hilly | ~6.9 km |
| AURORA RUN | high speed, long sweepers | ~8.4 km |

**Machines** — three craft with distinct top speed / acceleration / grip / boost stats.

**Driving** — the physics runs in track space (distance along the spline + lateral offset),
so the model stays stable at 600 km/h: centrifugal load scaled by curvature and speed²,
banking that pushes you back into the corner, grip damping, an air brake for tight turns,
and time-based wall drag with impact-scaled damage.

**Race** — 3 laps, 5 AI rivals that pick a racing line, brake for curvature, avoid each other
and rubber-band slightly; grid start with countdown, lap times, best lap, live position,
minimap, and a results table.

**F-Zero mechanics** — a power bar doubles as health and boost fuel: scraping the rails drains
it, hitting one hard costs a chunk, zero power destroys the machine (explosion, respawn at 42 %).
The pit strip recharges you, dash plates fire you forward, and boost costs 20 power.
Holding the air brake while sliding into a rival performs a side attack.

**Presentation** — synthwave sky shader, neon rails, ring gates, procedural road texture,
speed-scaled FOV, camera shake, particle sparks, chase and cockpit cameras, and a fully
procedural WebAudio engine (no audio assets) whose pitch tracks your speed.

**Soundtrack** — six streamed mp3 tracks in `music/` play as a shuffled, self-advancing
playlist; the procedural SFX sit under them through a master gain. Swap the files and edit the
`MUSIC` array to use your own. If the folder is missing the game simply stays on engine sound.

**Records** are stored per track in `localStorage`.

## Controls

| Key | Action |
|---|---|
| `W` / `↑` | Throttle |
| `S` / `↓` | Brake |
| `A` `D` / `←` `→` | Steer |
| `Shift` | Air brake (tightens turns, side attack) |
| `Space` | Boost (costs power) |
| `C` | Chase / cockpit camera |
| `P` / `Esc` | Pause |
| `R` | Restart |
| `Q` | Quality toggle (device pixel ratio) |
| `N` | Next music track |
| `-` / `+` | Music volume |
| `M` | Mute everything |

On touch devices: tap the left or right half to steer (throttle is automatic), both sides at
once to boost.

## Running it

Open `index.html` in any WebGL-capable browser. three.js r128 is loaded from a CDN, so an
internet connection is needed on first load.

## Development notes

Everything lives in `index.html` — markup, CSS and about 1100 lines of JavaScript in one IIFE.
Worth knowing if you want to poke at it:

- `TRACKS` — the circuit catalogue. Add an entry (harmonics, width profile, dash-plate
  positions, colours) and it shows up in the menu; `buildTrack(i)` rebuilds and disposes
  the previous geometry.
- `CFG` — the handling constants (centrifugal scale, banking assist, steering, boost cost).
- `MUSIC` / `SFX_MASTER` — playlist entries and the sound-effect master gain.
- `MACHINES` / `RIVALS` — vehicle and AI stats.
- `window.NV` — a small debug hook left in on purpose:
  `NV.state()` dumps the player/camera state, `NV.step(n)` advances the simulation
  deterministically without waiting for frames, `NV.track(i)` switches circuit,
  and `NV.probe()` renders the framebuffer as ASCII. Handy for automated checks.
