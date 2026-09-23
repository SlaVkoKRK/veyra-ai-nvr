# VEYRA 1.2.73 — Settings cleanup + Vision state + GitHub remote updates

VEYRA 1.2.73 starts the new official remote-update channel and completes the current Settings / Live / Gallery UI cleanup.

## UI

- Statistics no longer leak underneath the normal **Ustawienia** page. They remain a separate main-menu view only.
- **Veyra / Vision / Aktualizacje** tabs are vertically centered inside the settings header.
- The main YAML editor card and editor surface now use the full available settings width.
- **Wygląd** theme controls are placed below the appearance description instead of beside it.
- Single-camera Live class badges are pinned to the top-left edge of the camera and use the same class accent as the detection state (`person` red, `car` blue, etc.).
- Gallery **Vision Verify** now shows both pieces of state when automatic classification is active: **AUTO** stays highlighted and the actual computed `TP / FP / ?` result is highlighted at the same time. A manual override highlights the selected manual verdict instead.

## Remote updates from GitHub

Starting with this release the installed VEYRA can use the official GitHub repository as its update source. No local Git checkout and no Git credentials are required.

Flow:

```text
Ustawienia -> Aktualizacje -> Sprawdź aktualizacje
                              |
                              v
GitHub VERSION + dist/channel.json
                              |
                              v
SHA256-verified update package
                              |
                              v
restricted host control bridge
                              |
                              v
scripts/apply-update.sh -> backup -> update -> health checks / rollback
```

The updater supports the GitHub text-chunk transport (`manifest.txt` + base64 parts) as well as a direct package URL. It verifies SHA256 and refuses an accidental downgrade. Existing `/opt/ainvr/.env`, `config/`, `models/` and runtime `data/` remain outside the update payload.

For this first remote-channel test the supported starting point is **1.2.72**. Older releases do not need to be supported by this new updater.

## Live safety

The media transport remains unchanged:

```text
browser -> nginx -> go2rtc -> camera
```

No Python video relay, custom SourceBuffer, playback-rate control or second Coral pass is introduced.
