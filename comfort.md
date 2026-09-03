# GTAVR — Comfort & Safety Guide

VR motion sickness is a real physiological response, not weakness. In this
project **comfort is a safety requirement**, and several features exist
specifically to protect you. Use them — especially at the start.

## The 5 rules for your first sessions

1. **First week: 15-minute sessions**, seated, fan on your face if possible.
   Stop *before* you feel bad, not after — sickness lags exposure.
2. **Never push through symptoms.** Dizziness, warmth, sweating, eye strain,
   or stomach awareness = take the headset off. "Getting your VR legs" is
   gradual exposure over days, not endurance.
3. **Snap turn on** (default). Smooth turning is the most common trigger.
4. **Vignette on** while walking/driving (default). It restricts peripheral
   optic flow — the main sickness driver — during locomotion.
5. **Cutscenes play in the theater** (automatic). The game taking your camera
   in VR without mediation is the worst trigger there is; we never let it
   move your head directly. If you ever experience forced head motion, that
   is a P0 bug — please report it with the log.

## What the mod does for you (defaults)

| Feature | Default | Why |
|---|---|---|
| Snap turn (configurable angle) | ON | Optic-flow reduction |
| Locomotion vignette | ON | Peripheral-flow reduction |
| Vehicle horizon lock | ON | Decouples horizon from car pitch/roll |
| Cutscene theater mode | ON, always | Forced camera motion is P0 nausea |
| World scale / IPD from runtime | locked to runtime | Wrong IPD = eye strain + wrong world scale |
| Quick recentre bind | set | Drift happens; recentre often |
| Seated/standing calibration | on first run | Floor height correctness |

## Vehicles — the hardest context

Driving and flying are the highest-sickness situations in these games. The
vehicle profile (horizon lock, cockpit-relative reference, stronger vignette)
engages automatically. If you are sensitive: drive first-person with the
cockpit visible (a fixed reference frame helps enormously), avoid aircraft
until you are comfortable in cars.

## If something looks wrong

- **Double vision / world feels giant or tiny** → IPD/scale problem. Recalibrate
  in the overlay; if it persists, that is a defect — report it.
- **Judder/stutter** → your PC is below the framerate floor. Lower resolution
  scaling before anything else; check `docs/perf-report.md` guidance.
  Sustained judder is a sickness trigger — do not tolerate it.
- **Head-locked image during cutscenes** → that is the theater working as
  designed, not a freeze.

## The settings overlay (in-headset)

The settings menu is a head-locked panel — it follows your gaze.

- **Open/close:** `Delete` or `Insert` (keyboard, always works — even before
  controllers connect), `F10`, or the controller **menu button**
  (both-grips / both-face-buttons combos also toggle).
- **Point & click (controllers):** right thumbstick moves the cursor,
  trigger = left click, grip = right click; left thumbstick scrolls.
- **Mouse fallback (dead controllers):** the physical mouse drives the
  overlay cursor (the ImGui cursor is visible in-headset even though the
  desktop cursor is not); left/right click work. Requires the game window
  focused; `GTAVR_MOUSE_CAPTURE=0` disables this.
- **Keyboard fallback (dead controllers):** arrow keys navigate,
  Left/Right adjust the focused slider, `Enter` activates, `Esc` cancels.

Honest limits: the mod installs no keyboard hook, so nav keys also reach
the game (arrows can move in-game menus/phone while the overlay is open —
prefer adjusting while stationary). Text entry is not supported in-headset;
numeric fields use sliders and +/- nudge buttons.

## Health notice

Not for users under 13 (headset manufacturer guidance). Photosensitive
epilepsy warning applies as with any rendered content. Take 10–15 min breaks
hourly even when comfortable. By using this software you accept the health
notice in `docs/LEGAL.md`.
