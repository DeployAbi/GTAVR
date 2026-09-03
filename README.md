# GTAVR — VR for GTA V Legacy (Story Mode)

Six-degree-of-freedom VR for **Grand Theft Auto V Legacy**, delivered as a single
setup-and-play executable. No files to copy, no folders to find.

> ### Story Mode only
> GTA Online is **blocked by design**. The mod detects online sessions and
> BattlEye and disables itself. There is no setting to change this, and none
> will be added. Do not attempt to use it online.

**Download → [`GTAVR-Setup-and-Play.exe`](GTAVR-Setup-and-Play.exe)** · version 0.9.1

---

## Before you start — the one hard requirement

**Your game must be GTA V Legacy build `1.0.3889.0`.**

This is not a preference. The pinned ScriptHookV loads on exactly that
`GTA5.exe` file version and hard-refuses every other one, so on any other build
the game simply starts without VR. The launcher checks your build on the first
screen and tells you plainly:

```
GTAVR 0.9.1 | requires GTA V Legacy build 1.0.3889.0
| ScriptHookV v3889.0 / 1158.13 | your build: 1.0.3889.0  MATCH
```

You also need:

- **Windows 10/11 x64** and administrator rights (the game folder is usually
  under Program Files).
- An **OpenXR runtime** for your headset — Pimax Play, the Meta app, SteamVR.
  OpenXR is recommended; OpenVR also works.
- **BattlEye disabled** in the Rockstar Games Launcher (Settings → General).
  That is Rockstar's own switch for story-mode modding. The mod never touches
  BattlEye itself.
- **An internet connection for the first install** — or see *Installing
  offline* below.
- A GPU that holds a stable framerate. VR cares about p99 frametime, not
  averages.

## Install

1. Disable BattlEye, and launch Story Mode once, vanilla, to confirm it works.
2. Run `GTAVR-Setup-and-Play.exe` **as administrator**.
3. Pick your GTA V Legacy folder. Check the build line says **MATCH**.
4. **Install / Update Everything**, then **Verify**.
5. **Play Story Mode**.

VR engages roughly a minute after the world loads — you will see a flat
theater screen until then. That is normal.

### Installing offline

ScriptHookV is **not** bundled: its publisher does not permit redistribution,
so the installer downloads it from dev-c.com and checks its SHA-256 before
using it. If the machine has no internet, download
`ScriptHookV_3889.0_1158.13.zip` once from
<https://www.dev-c.com/gtav/scripthookv/> and put it **next to the exe**. The
installer finds it, verifies it against the same pinned hashes, and installs
without touching the network.

A copy already sitting in the game folder works too.

## The two settings that matter

Sharpness comes from **one** of them, and it is not the one people expect.

| Setting | What it does |
|---|---|
| **Game resolution** | Resizes the game window; GTA re-renders at that size. **This is the sharpness lever** — every VR pixel is sampled from this frame. |
| **Headset resolution** | Multiplier on what your headset software *already* asks for. Above `1.00` it only interpolates. |

Set headset resolution in **Pimax Play / Meta app / SteamVR**, not here — that
number reaches the mod verbatim, so `1.00` means "exactly what you configured
there". Leave it at `1.00` and spend your GPU on **Game resolution**.

The in-headset overlay shows the PPD (pixels per degree) each one delivers and
tells you which is the bottleneck.

### Stereo modes

- **Smooth** — the game renders the left eye every frame and the right eye is
  synthesized from that frame's depth. Both eyes come from the same instant, so
  there is no left/right time offset. Needs MSAA off.
- **Alternate-eye (AER)** — each eye is a real render, on alternate frames. Both
  eyes are perfectly correct, but they are one frame apart in time, which shows
  up as doubling on near objects when moving fast.

## Known issues

Honest list for 0.9.1:

- **No aim reticle.** The mod drives a scripted camera, and GTA suppresses its
  own reticle while one is active. Not yet replaced.
- **Smooth has spatial artifacts.** Only one viewpoint is really rendered, so
  edges of near objects (your own character, the car interior) can smear or
  ghost in the right eye, and transparents such as windscreens and rain sit at
  the wrong depth.
- **AER doubles near objects at speed.** Structural: at 45 fps the eyes are one
  game frame apart, which at 120 km/h is ~73 cm of camera travel. More frames
  is the only cure.
- **Oversized game window limits the mouse.** If Game resolution makes the
  window taller than your monitor, Windows will not let the cursor reach the
  off-screen part. A gamepad avoids it.
- **Unsigned.** Windows SmartScreen will warn. See below.

Headset-specific behaviour beyond the author's own hardware is **unverified**.

## SmartScreen

The executable is not code-signed and requests administrator rights, so Windows
will show a warning. If that is not acceptable to you, do not run it. You can
verify what you downloaded:

```powershell
Get-FileHash .\GTAVR-Setup-and-Play.exe -Algorithm SHA256
```

Expected: see [`SHA256SUMS.txt`](SHA256SUMS.txt).

## Uninstall

**Uninstall** in the launcher removes only files it installed and hash-proved
it owns, and restores any `version.dll` that was there before. Your settings
and logs are deliberately left behind. **Disable GTAVR** gives a vanilla next
launch without uninstalling anything.

## What gets installed

The exe embeds one matched set: the `version.dll` early loader, `OVRInject.dll`,
`GTAVRBridge.asi`, the OpenVR/OpenXR loaders, the build manifest and the config
templates. Every byte is verified on write, user settings are preserved across
updates, and any foreign file it would have to overwrite is backed up first —
or refused. No game files are modified.

Settings and the session log live in `%LOCALAPPDATA%\GTAVR`.

## Support the development

If you want this to keep going and getting better — **<https://ko-fi.com/deployabi>**

## Legal

Story Mode only. No DRM or anti-tamper circumvention. No game assets and no
third-party mod binaries are redistributed here. See [`LEGAL.md`](LEGAL.md) and
[`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md).

Not affiliated with, endorsed by, or associated with Rockstar Games or
Take-Two Interactive.
