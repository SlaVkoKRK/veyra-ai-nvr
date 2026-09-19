# Veyra AI-NVR

Current CORE version: **0.8.9**.

## 0.8.9 emergency VAAPI rollback

- reverts only the Intel VAAPI userspace override introduced in 0.8.8;
- keeps the proven Frigate 0.17.2 FFmpeg 7.0.2 runtime from 0.8.7;
- restores Debian Bookworm `intel-media-va-driver` + `i965-va-driver`, which was stable on the target Skylake host;
- no changes to motion, night assist, Coral, tracking, notifications or detection cadence;
- updater diagnostics now preserve the Docker build log for future failures.

Use the in-panel updater from **Ustawienia systemu → Sprawdź aktualizacje / Aktualizuj**.

Production config, camera credentials, models, event databases and snapshots are intentionally excluded from this repository.
