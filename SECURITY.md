# Security: what this exe does, why antivirus tools react to it, how to check

GTAVR is a VR injector for a single-player game. By construction it does the
things antivirus heuristics are written to notice. This page says exactly what
it does, what it never does, and how to verify it yourself instead of taking
anyone's word for it.

## 0.11.19 setup-checker change

**Scan status (2026-09-22):** local and GitHub Microsoft Defender scans
reported no threats; ClamAV reported zero infected files. The
[VirusTotal report](https://www.virustotal.com/gui/file/6c13f5b99024dde2fca448bec3d9723c4784a11b7a4b48f57bc7a493222b0295)
for the same EXE reports **9 detections, 62 undetected and 4 unsupported**,
including a Microsoft `Trojan:Win32/Wacatac.C!ml` verdict. These mixed results
are not established false positives. See the
[recorded results](scans/0.11.19-ffb-test.json) and
[completed CI run](https://github.com/DeployAbi/GTAVR/actions/runs/35774673517).
A successful CI run means the scans/report lookup completed, not that every
VirusTotal engine returned no detection.

On 2026-09-22 Microsoft Defender quarantined an older installed `GTAVOVR.exe`
helper as `Trojan:Win32/Wacatac.C!ml`. This is a recorded detection, not a
proven false positive. The GUI needed only its setup checks, but that older
binary also contained the developer launch/injection path.

The 0.11.19 distributed helper is compiled as a dedicated read-only checker:
manual process injection, process launch, file staging and settings writes
are excluded. It retains the installed filename for normal hash-owned updates.
Verify checks package hashes before executing it. The separate developer
injector is not shipped. The core still loads through `version.dll` in-game.

Local diagnostic Defender scans of the exact release EXE, ZIP and extracted
package reported no threats, with real-time protection active and no
exclusion changes or quarantine restoration. The [dated receipts](scans/0.11.19-ffb-test.json)
record hashes and engine/signature versions. Public CI scans are independent;
consult their actual results. No result guarantees future classifications.

## What the exe does, technically

`GTAVR-Setup-and-Play.exe` is a setup-and-play launcher. On **Install** it:

1. Asks for administrator rights, because the game folder is normally under
   `Program Files`.
2. Writes these files into the GTA V Legacy folder: `version.dll` (a proxy
   loader that starts the mod when the game starts), `OVRInject.dll` (the VR
   renderer), `GTAVRBridge.asi` (a ScriptHookV script that drives the camera),
   the OpenVR and OpenXR loader DLLs, a build manifest and config templates.
   Every file is hash-checked after writing. A foreign file it would have to
   overwrite is backed up first, or the install refuses.
3. Downloads the pinned ScriptHookV from its publisher,
   `https://www.dev-c.com/files/ScriptHookV_3889.0_1158.13.zip`, and verifies
   its SHA-256 before using it. ScriptHookV is never redistributed; a copy you
   downloaded yourself and placed next to the exe is accepted and verified the
   same way.

On **Play**, the game starts; `version.dll` loads `OVRInject.dll` into
`GTA5.exe`, which hooks the game's Direct3D 11 `Present` and a few D3D calls
(MinHook, in-process), reads the game's rendered frame, and submits it to your
headset through OpenXR or OpenVR. The bridge script talks to the game through
ScriptHookV's public native API.

That is the complete list of things it touches: your game folder, your
headset runtime, and `%LOCALAPPDATA%\GTAVR` for settings and a log.

## What it never does

- **No network use** other than the one ScriptHookV download from dev-c.com.
  There is no update check, no telemetry, no crash reporting, no analytics.
  The only other URL in the program is the Ko-fi page the "Support" button
  opens in your browser.
- **No persistence.** No service, no driver, no scheduled task, no autostart,
  no registry run keys. Nothing runs when the game is not running.
- **No files outside** the game folder and `%LOCALAPPDATA%\GTAVR`, plus a
  temporary extraction folder under `%TEMP%` during install.
- **No game file modification.** It adds files; it never patches `GTA5.exe`
  or any Rockstar file.
- **No DRM or anti-cheat circumvention.** GTA Online and BattlEye presence
  hard-disable the mod, with no setting to change it.
- **No credential, browser, wallet or keylogging code.** It reads keyboard
  state only for its own hotkeys (overlay toggle, freeze-frame, dump) while
  the game window is in the foreground.

## Why antivirus and SmartScreen react anyway

Heuristic engines score behaviour, not intent, and this exe scores on
several classic points at once:

- It is **not code-signed** (see below), so SmartScreen shows "Windows
  protected your PC" for every unsigned installer that asks for admin rights.
- It **drops DLLs into another program's folder**, including a `version.dll`
  proxy - the same technique some malware uses to get loaded, and the same one
  ReShade, ENB and most single-player mod loaders use.
- Its DLL **hooks Direct3D inside the game process** (API hooking), which is
  what every overlay from Steam to Discord to GeForce Experience does, and
  what a lot of game cheats do too. Engine labels differ; the older helper received a
  Trojan detection, as documented above.
- It **downloads and verifies a third-party archive** during install.

These characteristics do not establish the cause or correctness of any
particular detection. The checks below provide evidence for each exact file.

For the record, 0.9.7 on VirusTotal: 62 engines clean (Kaspersky, ESET,
Bitdefender, Avast, Sophos, CrowdStrike, SentinelOne, Trend Micro,
Malwarebytes and the rest), 9 flagged, all nine with machine-learning or
hash-keyed generic labels and no malware family name. Every release's
numbers are in [scans/](scans/).

## How to check for yourself

**1. Checksum.** Compare the file you downloaded with `SHA256SUMS.txt` in this
repository and with the hash printed in the release notes:

```powershell
Get-FileHash .\GTAVR-Setup-and-Play.exe -Algorithm SHA256
```

**2. Public, automated scans.** Every release is scanned on GitHub's own
runners by the [`security-scan`](../../actions/workflows/security-scan.yml)
workflow: Microsoft Defender with freshly updated signatures, ClamAV, a
checksum verification, and VirusTotal when an API key is configured. The run
logs are public and are produced by infrastructure the author does not
control.

**3. Your own engines.** Upload the exe to <https://www.virustotal.com/> and
compare the SHA-256 shown there with `SHA256SUMS.txt`. Report detections with the exact file hash and engine/signature version
so they can be investigated. A generic label does not establish that a
detection is false.

**4. Watch it run.** Sysinternals Process Monitor on the install shows the
file writes (game folder, `%LOCALAPPDATA%\GTAVR`, `%TEMP%`) and a single
TLS connection to dev-c.com. Nothing else.

## Code signing

The exe is unsigned. An OV/EV code-signing certificate costs a few hundred
dollars a year and requires a verified legal identity; the project has neither
yet. Signing would remove the SmartScreen warning; it would not change what
the program does. If it happens, the release notes will say so and the
`Authenticode status` step in the scan workflow will show `Valid`.

Measured on 2026-09-04, for anyone wondering how much the metadata matters:
the 0.9.7 launcher carried no version resource at all. Rebuilding the
identical mod with a full identity (publisher, product, description,
version) moved VirusTotal from 9 to 8 flagged engines - Bkav and Ikarus
dropped off, Elastic went from high to moderate confidence, one generic
`susgen` entry appeared. The remaining eight are machine-learning models
that weight the missing signature above everything else, which is why the
next step is a certificate, not more metadata. Every release from the next
one on carries the identity.

## Reporting

If you believe a release is compromised or you found a vulnerability, open a
GitHub issue with the SHA-256 of the file you have and the engine or evidence
that flagged it. Please do not post exploit details for a genuine
vulnerability publicly before there is a fix.
