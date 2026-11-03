---
name: reinstall-fxsound
description: Uninstall the installed FxSound AppImage and install a fresh one built from the current repo HEAD. Use when the user says "reinstall", "install the real version", "install with the latest fix", or "uninstall fxsound".
---

# Uninstall / install the real FxSound (AppImage)

The installed version is `~/Applications/FxSound.AppImage`, launched from
`~/.local/share/applications/fxsound.desktop`. User settings in
`~/.config/fxsound` are never touched.

## Uninstall
1. `pkill -TERM -x fxsound`, then wait until `pgrep -x fxsound` is empty (SIGTERM restores the default sink).
2. `mv ~/Applications/FxSound.AppImage ~/Applications/FxSound.AppImage.old` (keeps one backup, replacing the previous `.old`).
3. To uninstall without reinstalling, also remove `~/.local/share/applications/fxsound.desktop`.

## Install from the repo
1. Check `git status` is clean and note `git log --oneline -1`: the AppImage is built from the working tree.
2. Build: `distrobox enter fxsound-dev -- ./packaging/build-appimage.sh --build-dir build-appimage`
   (it downloads linuxdeploy/appimagetool to `/tmp/appimage-tools` the first time).
3. Install: `install -m755 FxSound-*-x86_64.AppImage ~/Applications/FxSound.AppImage`, then delete the built file from the repo root.
4. Make sure `~/.local/share/applications/fxsound.desktop` exists with `Exec=$HOME/Applications/FxSound.AppImage` (write the absolute path), `Icon=fxsound`, `StartupWMClass=FxSound`.
5. Start it detached: `setsid -f ~/Applications/FxSound.AppImage >/dev/null 2>&1`, then check `wpctl status` shows `* FxSound`.

To roll back: stop it and move `FxSound.AppImage.old` back.
