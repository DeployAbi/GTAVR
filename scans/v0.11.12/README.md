# GTAVR 0.11.12 verification

`GTAVR-Setup-and-Play.exe` SHA256:

`24563E12D9F7D369E99C171F138601F42D19E04C9068B575030AC2F0CDAA64E7`

Implementation revision: `29a45d288626268f3f678336e1915995a0b3bfde`.
The EXE is unsigned. See [SECURITY.md](../../SECURITY.md) for its behavior.
This repository publishes the EXE and documentation, not implementation source.

## Build and package checks

- Release x64 core and matching ABI12 bridge built successfully.
- Native suite: **693 passed, 0 failed, 1 optional GPU benchmark skipped**
  (694 tests; 31,778 checks, zero failures).
- Tests cover calibrated finite force commands, caps and rejected updates,
  shared profiles, radio native control IDs, vehicle/freshness gates, and one
  press per station with release after blocked input. Real DXGI resize tests
  preserve the next draw and unrelated graphics state.
- Legacy launcher: **296 checks passed**. Both radio rows and centering
  controls were visually checked in the rendered Bindings dialog.
- Shared launcher title-routing/ownership fixtures passed; render-packet
  diagnostics passed 9 tests, with one optional test skipped.
- Proxy loader smoke, packaged self-tests and archive audit passed.
- All six native binaries match build inputs, EXE payloads and installed files.
- The archive audit found 42 checksummed files and zero forbidden third-party
  mod binaries. ScriptHookV is acquired separately from its publisher.
- Installation byte-verified eight owned files; Verify returned PASS.
- Ten existing configuration/mod files, including current wheel/pedal
  calibration, were byte-identical after installation.
- Only the EXE is published as a release asset.

[Release manifest](release-manifest.json) records the package inputs and scope.
Its dirty-worktree flag reflects vendored dependency line endings; the
normalized Git diff is empty. Implementation changes are committed locally.

## Runtime acceptance

The user confirmed menu/gameplay rendering and wheel/pedal input on the prior
selective-resize candidate (EXE SHA256
`EC53B0CD350632D9B903BB3CDC5BBCFD336C42EF6BE7C26649E1572B8AD43532`).
That rendering implementation is retained. Its matching game trace recorded
AER and mode-0 gameplay and separate steering/pedal devices.

The current revision changes centering output and adds radio controls. Actual
centering direction/feel, overlay use in the headset, and station changes on
this exact core/bridge pair remain **UNVERIFIED** pending the rig trial.
Driver acceptance of a force command is not a measurement of motor torque.
Other wheel bases, pedal hubs and headsets remain UNVERIFIED.

## Antivirus results

GitHub runs checksum verification, Microsoft Defender and ClamAV when this
EXE is published. VirusTotal upload depends on a configured repository API key.
No result is claimed until the run for this exact EXE completes; see the
[public workflow](https://github.com/DeployAbi/GTAVR/actions/workflows/security-scan.yml).
