# Veyra AI-NVR

Current CORE version: **0.8.8**.

## 0.8.8 updater hotfix

- keeps the proven Frigate 0.17.2 FFmpeg 7.0.2 runtime;
- copies only the pinned Frigate 0.17.2 `iHD_drv_video.so` + `libigdgmm.so.12` into CORE;
- keeps Debian `i965-va-driver` as fallback instead of copying an optional i965 blob from the Frigate image;
- fixes the GitHub updater build failure that returned code 2 when the optional i965 export file was absent;
- update payload remains differential and SHA256 verified.

Use the in-panel updater from **Ustawienia systemu → Sprawdź aktualizacje / Aktualizuj**.

Production config, camera credentials, models, event databases and snapshots are intentionally excluded from this repository.
