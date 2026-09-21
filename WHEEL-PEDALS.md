# Wheel and pedals: DirectInput controls

**This 0.11.12 build is withdrawn after a reported launch error 17. Do not
install it. Use [0.11.10](https://github.com/DeployAbi/GTAVR/releases/tag/v0.11.10).
The feature instructions below are retained for the withdrawn build only.**

GTAVR 0.11.12 reads separate steering bases, USB wheels and pedal hubs
through Windows DirectInput. It includes the eight axes and extra buttons
that the older Windows joystick interface can miss. It adds comfortable
steering limits, a response curve and an optional gentle centering spring.
It does **not** add road feedback, ActivePedal vibration/ABS effects or a gearbox.
SimHub is not required for these driving inputs.

1. Finish the current game session and close GTA. Run the new EXE and
   use **Install / Update Everything** to install its matching native core.
2. Open **Bindings**. The device list should include **Simucube 2 Pro**,
   **GSI Steering Wheel** (30 buttons, four axes) and **SC-Link Hub** (128
   buttons, eight axes) on the inspected rig. Press **Refresh** after changing
   connected devices. Let any `[waiting]` indication clear.
3. Enable **Use steering wheel / pedals to drive cars**. Choose **Calibrate
   steering range**, select the Simucube **base**, capture center, then your
   comfortable left and right limits (for example 90 degrees each way).
   Each chosen limit now means full in-game steering. **Steering response**
   100 is linear; lower values turn faster, higher values soften the center.
4. Click **Accelerate**, then press only the throttle. Release it. Click
   **Brake**, then press only the ActivePedal. The captured name should refer
   to **SC-Link Hub**; axis names depend on the device configuration. Keep
   **more or less (analog)** enabled for steering, throttle and brake.
5. Watch **Live driving test**: each pedal should return to 0% when released
   and approach 100% when fully pressed; left and right should respond in the
   intended directions and return to zero at center. A disconnected device
   is reported explicitly. Rebind if rest position or direction is wrong.
6. **Save bindings**, then **Play Story Mode**. Gamepad input mode can use
   these device bindings while VR supplies head tracking. If you edit
   bindings with the new game/core already running, use **Reload shared
   bindings** in the headset panel, close it and return focus to the game.

Bind **Previous radio station** and **Next radio station** in the same EXE
Bindings dialog to two wheel buttons of your choice. These are car-only actions
while Driving is enabled. Each press changes one station; release before the
next press. The headset **Bindings** tab displays these bindings and lets you
remove or reload them. Existing steering and pedal bindings are preserved.

For resistance, enable **Wheel force (centering)** after calibration. Change it
in the EXE or live in the overlay's **Controls** or **Bindings** tab. It starts
at **2%** of base force, with a hard maximum of **5%**. Close the overlay to
resume force in a car. The setting saves to the shared binding profile.

The implementation applies a restoring constant force, scaled independently
by your calibrated left/right travel, with a small neutral band. This replaces
the earlier native Spring effect, which the driver accepted on the inspected
rig but the user could not feel. Simucube profiles have a separate Spring
effect multiplier; the new implementation uses the ordinary constant force
channel. The cause of the previous lack of physical force is not proven.

Output is limited to Simucube 2 wheel bases while driving with game/VR focus.
It stops in menus, on foot and when focus is lost, and expires after 150 ms
without updates. No motor runs during calibration; no force goes to the
ActivePedal hub. The base's force settings and hardware stop remain in control.
The overlay reports disabled motor output/zero gain when the driver reports
that state. `[wheel-force]` logs commands, cap and status; `command-sent`
records API acceptance, not measured torque. Road/tire feedback is not included.
Physical force and radio behavior remain **UNVERIFIED** pending the rig trial.

This build accepts simple menu frames while retaining the previous startup
resolution timing and preserves GTA's cached rendering state during resizing.
It removes the gameplay-delayed resize from the withdrawn 0.11.12-wheel-test
build, which caused a reported black gameplay image.
Physical menu and gameplay acceptance is recorded separately from unit tests.

If the brake cannot be captured, check that its brake output moves in
Simucube Tuner. Being an active pedal does not require the game to control its
motor to read its braking input. No guessed brake axis is installed by this
build. The user confirmed steering/pedal and menu/gameplay behavior on the
preceding selective-resize candidate. Other hardware remains **UNVERIFIED**.

The first DirectInput save backs up the previous shared bindings to
`%LOCALAPPDATA%\GTAVR\input-bindings.ini.before-directinput.bak`.
Adding radio bindings uses version 5 and also preserves the previous profile
as `input-bindings.ini.before-radio.bak`. Files without radio bindings retain
version 3 or 4.
To return to 0.11.10, close the game, reinstall the previous EXE, and restore
that backup as `input-bindings.ini`; older cores cannot read version-4/5
DirectInput bindings. User graphics/camera settings are retained.

The linked public Manual Transmission 5.6.1 explicitly excludes GTA builds
3095 and newer, including this rig's 3889. Its 5.6.2 announcement moved newer
versions to licensed Patreon distribution. It is not bundled here. A future
integration would need a licensed compatible build, redistribution permission
where applicable, and VR coexistence/force-feedback testing.

Sources: [Manual Transmission compatibility](https://www.gta5-mods.com/scripts/manual-transmission-ikt),
[author's release announcement](https://github.com/ikt32/GTAVManualTransmission/releases/tag/v5.6.2),
[Simucube input interface](https://docs.simucube.com/Simucube%202/Developers.html),
[Simucube game effect settings](https://docs.simucube.com/TunerSoftware/wheelbases/wheelbaseeffects.html).
