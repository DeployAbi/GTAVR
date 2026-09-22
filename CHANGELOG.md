# Changelog

## 0.11.19-ffb-test - 2026-09-22 (prerelease)

### Wheel and pedal input

- Fix controls that capture correctly in the EXE but do nothing in the car.
  An optional VR body update could erase accepted wheel/pedal input when VR
  hands were inactive, including Gamepad mode. Physical steering, throttle
  and brake now remain active independently of VR hand tracking.
- Read separate DirectInput steering bases and pedal hubs, including axes
  unavailable through the older joystick interface. Calibrate steering
  center/limits and each pedal's rest/full-press travel, including inverted
  axes. Existing calibrated bindings are preserved.
- Include vehicle-only radio bindings and optional Simucube 2 centering and
  telemetry-based road/engine force. Force-direction inversion, a 300 ms
  engagement ramp and an always-on runaway watchdog are included. Start low;
  the configurable maximum is 100% of DirectInput nominal, subject to the
  base's own gain. ActivePedal motor effects and a gearbox are not included.
- Include the launcher force test and the ZIP's `wheelprobe --axes` diagnostic
  for pedal travel. Physical force feel and other hardware remain UNVERIFIED.

### Startup and rendering

- Decode UTF-16 Social Club logs so the startup observer can recognize the
  current session's UI readiness. Online/BattlEye protection remains unchanged.
- Preserve cached game rendering state during source-image resize and accept
  sparse menu frames. Retain the vehicle-attached VR camera.
- Hold VR camera ownership through brief transient vehicle/startup blockers;
  protection, pause, cutscene and foreign-camera blockers still release it
  immediately. Add AER cadence and device-input diagnostics.

### Installer and verification

- Replace the distributed developer injector with a dedicated read-only
  setup checker. Manual process injection, process launch, file staging and
  settings writes are excluded from that binary. Normal Story Mode loading
  continues through the proxy loader.
- Stop Verify/Check Setup from executing an old helper after installed
  payload hashes fail verification. Use Install / Update Everything first.
- This follows a Defender quarantine of the older `GTAVOVR.exe` helper as
  `Trojan:Win32/Wacatac.C!ml`. That older detection is not established to be a
  false positive. No quarantined file was restored or antivirus setting changed.

### Validation and limits

- The core/bridge are the tested 0.11.18 pair: 722 native tests passed, zero
  failed, one optional GPU benchmark skipped; 224147 checks. Camera/input
  integration: 10 tests / 2478 checks; camera client: 4 tests / 68 checks.
- Read-only checker import/profile audit and 30 preflight cases passed;
  recompiled launcher binding/UI fixtures passed 205 checks.
- Proxy-loader smoke, installer/runtime/title/scope self-tests and the
  46-file package audit passed. All six embedded native payloads were verified.
- Local Microsoft Defender scans found no threats in the exact EXE, ZIP and
  extracted package on 2026-09-22, engine 1.1.26080.3, signatures 1.459.330.0.
  See [scan receipts](scans/0.11.19-ffb-test.json); public CI results are separate.
- In-game/headset acceptance of this exact candidate remains UNVERIFIED.
  This is a test release for GTA V Legacy **1.0.3889.0**, Story Mode only.

Close GTA, run **Install / Update Everything**, then **Verify**, then
**Play Story Mode**. See [wheel setup](WHEEL-PEDALS.md). The withdrawn 0.11.12
release remains withdrawn.

## 0.11.12 - WITHDRAWN - 2026-09-21

Withdrawn after a launch failure reported as error 17. Do not install this
version. The public EXE and latest release have been restored to
[0.11.10](https://github.com/DeployAbi/GTAVR/releases/tag/v0.11.10).
The cause is under investigation; passing unit tests and scans did not
establish successful game startup on the final core/bridge pair.

### Wheel and pedals

- Capture controls from separate DirectInput devices, with eight axes, 128 buttons and four hats per device. This covers pedal axes missing from the older joystick interface.
- Bind steering, accelerator, brake and handbrake in the EXE. Existing keyboard, gamepad and joystick mappings remain readable.
- Calibrate steering center and comfortable left/right limits. Each captured limit maps to full steering; the steering-only response curve ranges from 0.5 to 2.0, with 1.0 linear.
- Replace the ineffective native Spring effect with calibrated constant-force centering on supported Simucube 2 bases. It is off by default, initially 2% when enabled, capped at 5%, restricted to focused vehicle gameplay, and expires after 150 ms without updates. It provides basic resistance; road/tire effects and ActivePedal motor output are not included. Physical force feel/direction remains UNVERIFIED on this revision.
- Add Wheel force (centering) enable and strength controls to the in-game Controls and Bindings tabs. Changes save to the shared profile and take effect after closing the overlay while driving. Report driver-reported motor-off/zero-gain states without changing the wheel's profile or hardware stop.
- Add Previous radio station and Next radio station device bindings in the EXE. They operate only in a vehicle with Driving enabled, once per press. Held or conflicting buttons do not repeat; pause/focus changes require release before the next station change. Actual in-car station response remains UNVERIFIED on this revision.
- Retain version-4 steering/pedal calibration; adding radio buttons uses version 5 and backs up the previous profile. Existing version 1-4 files remain readable.
- Package a matching core and bridge for the new car-radio controls. Install/Update Everything updates them together.

### Menu and gameplay

- Preserve GTA's cached rendering state when resizing the game image. Release only views of the old backbuffers instead of clearing shaders, samplers and other unrelated state.
- Present sparse menu frames, including draws that reuse the current shader. Continue filtering unchanged keep-alives and preserve the gameplay threshold.
- Keep menu/loading HUD content in the game image.
- Remove the deferred gameplay resize from the unpublished `0.11.12-wheel-test` package. Restore the established startup resolution behavior after that test caused black gameplay in both AER and depth stereo.
- Retain the vehicle camera attachment delivered in 0.11.10.

### Installation

- Report which program holds an installed mod file, including a leftover PlayGTAV process, and offer to close it before installing or uninstalling.
- Keep the single EXE and Legacy-only package. Manual Transmission, handling mods and telemetry mods are not bundled.

The user confirmed menu/gameplay rendering and steering/pedal behavior on the preceding selective-resize candidate. That rendering implementation is retained. Automated checks on this revision passed: 693 native tests (one optional skip), 296 Legacy launcher checks and shared-launcher/diagnostic fixtures. Physical centering and radio changes require a separate rig trial. Other wheel bases, pedal hubs and headset combinations remain UNVERIFIED.

## 0.11.10 - 2026-09-21

- Added click-to-bind controls for gamepads, steering wheels and pedals.
- Attached the VR camera to the vehicle to address the reported alternating-position flicker.
- Improved uninstall failure reporting and persisted launcher logs.

## 0.11.9 - 2026-09-21

- Added capture-based controller bindings, per-binding click/analog selection and a driving control set.

## 0.11.8 - 2026-09-21

- Expanded gameplay HUD capture and independent controller arming.
- Prepared depth-stereo resources before the first frame and improved camera lease handling during stalls.
- Added driving and comfort defaults. Earlier history is retained in the README.
