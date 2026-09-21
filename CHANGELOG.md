# Changelog

## 0.11.12 - 2026-09-21

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
