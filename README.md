# Veyra AI-NVR

Current CORE version: **0.9.1**.

## 0.9.1 — updater hotfix + black-frame watchdog

- fixes the updater bug that reported a failed update as `kod 0` because Bash `!` inverted the real exit status before it was captured;
- differential GitHub update packages now always include `docker-compose.yml`, which is required by `apply-update.sh` package validation;
- includes the per-camera NV12 black-frame watchdog prepared in 0.9.0;
- keeps the stable media stack from 0.8.9: Frigate 0.17.2 FFmpeg 7.0.2 with Debian VAAPI drivers;
- future updater failures preserve the real exit code and build log tail.

Use the in-panel updater from **Ustawienia systemu → Sprawdź aktualizacje / Aktualizuj**.

Production config, camera credentials, models, event databases and snapshots are intentionally excluded from this repository.
