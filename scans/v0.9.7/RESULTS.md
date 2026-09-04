# Security scan results: GTAVR 0.9.7

File: `GTAVR-Setup-and-Play.exe`
SHA-256: `8E911581F18E5D85AB7111E35081D8A7D4EDAC225D60FBDD3B2FF5F948473194`
Release: <https://github.com/DeployAbi/GTAVR/releases/tag/v0.9.7>

All scans below ran on GitHub-hosted runners, not on the author's machine.
Full log (public): <https://github.com/DeployAbi/GTAVR/actions/runs/33886946940>

## Microsoft Defender

| | |
|---|---|
| Platform | 4.18.26070.9 |
| Engine | 1.1.26080.3 |
| Signatures (updated at scan time) | 1.459.49.0 (2026-09-04) |
| Command | `MpCmdRun.exe -Scan -ScanType 3 -File GTAVR-Setup-and-Play.exe -DisableRemediation` |
| Result | **found no threats** (exit code 0) |

## ClamAV

| | |
|---|---|
| Engine | 1.5.3 |
| Signature database | 28113 (2026-09-04), 3,628,042 known signatures |
| Command | `clamscan --infected GTAVR-Setup-and-Play.exe` |
| Result | **Infected files: 0** |

## Checksum

`sha256sum -c SHA256SUMS.txt` on the runner: `GTAVR-Setup-and-Play.exe: OK`

## VirusTotal

Not run automatically for this release (no API key configured in the
repository yet). Upload the file at <https://www.virustotal.com/> yourself and
compare the SHA-256 shown there with the one above; expect the usual
"HackTool"/"Injector" heuristics from engines that flag every game mod loader
(see [SECURITY.md](../../SECURITY.md)).

## Build attestation

From `release-manifest.json` (embedded in the package, copied here):

| | |
|---|---|
| Source commit | `fb2d71a433a3104a42c36ac93e0d52b283176664` |
| Built from a clean worktree | yes (`dirty_worktree: false`) |
| Code-signed | no |
| Game assets included | **none** |
| ScriptHookV included | **none** (downloaded from dev-c.com at install and verified against SHA-256 `B64C97C3353906F14621E7E9511E4AEC2A7D436ECC21ED124D3816585E2E6188`) |

Release-script audit at packaging time:

```
PROXY_LOADER_SMOKE=PASS
PACKAGE_AUDIT=PASS
FORBIDDEN_THIRD_PARTY_COUNT=0
CHECKSUM_FILES=13
SELF_TEST=PASS
SIGNATURE_STATUS=NotSigned
```

The package audit refuses to build if any third-party mod binary
(ScriptHookV, its ASI loader, any game file) is present in the payload; the
self-test installs the package into a throwaway folder and verifies every
written file against its recorded hash.

## Unit tests of the mod's own logic

```
197 test(s): 197 passed, 0 failed, 0 skipped (2357 checks, 0 check failures)
```

These cover the stereo delivery, frame identification, image mapping, HUD
routing policy, settings parsing and bridge queue logic. They are evidence
that the code does what its author intended, not evidence about malware; the
scans above are for that.

## What the exe embeds

The single-file launcher carries one matched set and nothing else:
`version.dll` (proxy loader), `OVRInject.dll`, `GTAVRBridge.asi`,
`openvr_api.dll`, the OpenXR loader, `manifests/gtav_legacy.ini`, the config
templates, `README.txt`, `SETUP.md`, `LEGAL.md`, `THIRD_PARTY_NOTICES.md` and
the third-party licence texts (Apache-2.0, MIT, BSD-2 MinHook and
readerwriterqueue, BSD-3 OpenVR). Every embedded file is hash-checked after
it is written to the game folder.
