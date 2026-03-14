# Hand Gun Tracker

Browser-based hand gesture gun tracker using computer vision. Detects a "finger gun" hand pose via webcam and renders a cyberpunk-styled shooting experience with muzzle flash, particles, shell casings, and sound effects.

## Tech Stack

- **Single-file app**: Everything in `index.html` (HTML + CSS + JS)
- **Hand tracking**: MediaPipe Hands (CDN) — 21 landmark detection, up to 2 hands
- **Audio**: Web Audio API (procedurally generated gunshot, reload, empty click sounds)
- **Rendering**: Canvas 2D with custom HUD, particle system, and gun renderer
- **No build step**: Serve with `python3 -m http.server 8080`

## Architecture

All code lives in `index.html` with these main classes:

| Class | Purpose |
|-------|---------|
| `GestureDetector` | Detects finger gun pose + thumb hammer state machine (idle → cocked → fired) |
| `GunRenderer` | Draws a detailed pistol silhouette aligned to the hand |
| `HUD` | Ammo counter, FPS, crosshair, reload bar, hit markers, BANG text |
| `MuzzleFlash` | Additive-blend flash effect with directional spikes |
| `Particles` | Spark + smoke particle system |
| `Shell` | Ejected shell casing physics |
| `Shake` | Screen shake on fire |
| `Audio` | Web Audio synth for shot/reload/empty sounds |
| `App` | Main loop — camera init, MediaPipe setup, frame processing |

## Gesture Controls

- **Finger gun** (index + middle extended, ring + pinky curled): Activates weapon
- **Thumb down** (while in gun pose): Fires — thumb must first be "cocked" (raised) then brought down
- **Open hand** (all fingers extended): Reloads ammo (12 rounds max)

## Key Constants

- `AMMO_MAX = 12`, `RELOAD_FRAMES = 55`, `FIRE_COOLDOWN = 14`
- Thumb thresholds: `COCK_THRESH = 0.55`, `FIRE_THRESH = 0.42` (normalized by hand size)

## Running Locally

```bash
cd ~/hand_gun && python3 -m http.server 8080
# Open http://localhost:8080 — allow camera access when prompted
```

## Notes

- Camera feed is mirrored (selfie mode) — landmark x-coordinates are flipped accordingly
- All audio is procedurally generated via Web Audio API (no audio files needed)
- Video element is hidden (1x1px) — only the canvas is visible
