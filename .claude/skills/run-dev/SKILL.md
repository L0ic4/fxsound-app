---
name: run-dev
description: Build and run the FxSound dev version from this repo (build-rel) in the fxsound-dev distrobox, stopping the installed AppImage first. Use when the user says "run dev", "try the dev build", or wants to test a fix live.
---

# Run the dev build

1. Stop any running FxSound with SIGTERM, so each one hands the default sink back:
   `pkill -TERM -x fxsound; pkill -TERM -x FxSound` (the installed AppImage runs `fxsound`, the dev build `FxSound`), then wait until `pgrep -x 'fxsound|FxSound'` is empty.
2. Build: `distrobox enter fxsound-dev -- cmake --build build-rel`
   (if `build-rel` is missing: `cmake -S . -B build-rel -G Ninja -DCMAKE_BUILD_TYPE=Release` first).
3. Run it in the background with the user's real config (that's what they want to test):
   `distrobox enter fxsound-dev -- ./build-rel/FxSound` with `run_in_background`.
4. Confirm: `wpctl status | grep -A4 Sinks` shows `* FxSound`.

To go back to the installed version: stop the dev build with `pkill -TERM -x FxSound`, then start `~/Applications/FxSound.AppImage`.
