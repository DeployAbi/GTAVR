# Legal Posture

Last updated: 2026-07-24. This document is information, not legal advice.

## What this project is

A runtime mod that converts the **single-player** mode of legally owned
RAGE-engine games into a VR experience, via DLL injection and graphics-API
hooking. It ships only its own code plus a loader that operates on the user's
own installation. It contains **no game assets, no decrypted binaries, and no
code from Rockstar/Take-Two or third-party mod authors**.

## Hard rules the project enforces in code

1. **Single-player only.** The mod detects online sessions (GTA Online / Red
   Dead Online) and BattlEye presence, and hard-disables itself — no injection
   into online play, ever. There is no configuration to bypass this. Modding
   online modes violates the publisher's terms and harms other players.
2. **No DRM or anti-tamper circumvention.** The mod does not patch, bypass, or
   interfere with Denuvo, BattlEye, Arxan, Rockstar launcher protections, or
   integrity checks. Story-mode modding of GTA V requires the user to disable
   BattlEye themselves via **Rockstar's own launcher option** — the mod never
   touches it. If a title's protection blocks legitimate injection, that title
   is documented as unsupported.
3. **No redistribution.** Installers never copy game files; uninstall is
   complete. Users must own the game.

## Publisher policy risk (user-assumed)

Take-Two/Rockstar's enforcement posture on single-player mods has varied over
time (including DMCA action against a prior VR mod in 2022, later resolved).
**Publisher policy can change at any time.** Using this mod may violate the
game's EULA/ToS even though it is single-player only. By using the software,
**the user assumes that risk**, including the risk of account action. The
authors recommend: story mode only, offline where possible, and never with
modified game files that persist into online play.

## Prior-art respect

Design knowledge was informed by public analysis of LukeRoss's R.E.A.L. mods
and Praydog's UEVR. No code, shaders, binaries, or assets from those works are
copied or distributed. Pattern/offset knowledge used for interoperability is
re-derived and documented per build in `manifests/`.

## Interoperability basis

Hooking and signature-scanning for interoperability with a lawfully acquired
program is pursued under interoperability principles (e.g. EU Directive
2009/24/EC Art. 6; US fair-use-for-interoperability case law such as Sega v.
Accolade / Sony v. Connectix). This is a good-faith engineering posture, not a
guarantee of outcome in any jurisdiction.

## Third-party components

Vendored under `ThirdParty/` with their own licenses: OpenVR (BSD-3), OpenXR
loader/headers (Apache-2.0), Dear ImGui (MIT), MinHook (BSD-2), DirectXMath
(MIT), readerwriterqueue (BSD-2). Their notices govern their use.

## Health notice

VR motion sickness is a real physiological risk. Comfort features are part of
the safety design (see `docs/user/comfort.md`), but no software can guarantee
individual tolerance. Stop immediately on discomfort.

## Warranty

None. Provided "as is", without warranty of any kind. The distribution license
for the project's own code is a maintainer decision pending before any public
release (open-source recommended; see `docs/00-feasibility.md` §0).
