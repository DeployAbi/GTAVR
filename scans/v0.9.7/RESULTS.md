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

## VirusTotal (75 engines)

Report: <https://www.virustotal.com/gui/file/8e911581f18e5d85ab7111e35081d8a7d4edac225d60fbdd3b2ff5f948473194>
(GitHub run with the VirusTotal upload: <https://github.com/DeployAbi/GTAVR/actions/runs/33888062420>)

| | |
|---|---|
| Undetected | **62** - including Kaspersky, ESET, Bitdefender, Avast, AVG, Avira, Sophos, Trend Micro, F-Secure, G Data, Emsisoft, Malwarebytes, CrowdStrike, SentinelOne, Cylance, Deep Instinct, Palo Alto, Fortinet, Dr.Web, Trellix ENS |
| Flagged | **9** - APEX (Malicious), Bkav (W32.Malware.178ECB18), Elastic (malicious, high confidence), Google (Detected), Ikarus (Trojan.Win64.Krypt), McAfee cloud (ti!8E911581F18E), Microsoft (Trojan:Win32/Wacatac.B!ml), Symantec (ML.Attribute.HighConfidence), Trapmine (suspicious.low.ml.score) |
| Not applicable | 4 mobile engines |

What the nine have in common: every label is a machine-learning or
hash-keyed heuristic (`!ml`, `ML.Attribute`, `low.ml.score`, `ti!<hash>`,
the generic `Krypt`/`W32.Malware.<id>` buckets). None names a malware family,
and the signature-based engines - including Trellix's own signature engine
next to McAfee's cloud ML - pass it. Microsoft Defender on the GitHub runner
(no cloud lookup) also passes it; `Wacatac.B!ml` is Microsoft's cloud model,
the best-known generic verdict on unsigned self-contained installers.

Why the models score it: the launcher is an unsigned .NET executable that
carries ~4 MB of embedded binaries, requests administrator rights, writes
DLLs into another program's folder and, until 0.9.7, had **no version
resource at all** (file version 0.0.0.0, no publisher, no description).
0.9.8 stamps a proper identity; a code-signing certificate is what removes
the rest.

If you want a second opinion without trusting this page: upload the file at
virustotal.com yourself and compare the SHA-256 shown there with the one at
the top.

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
