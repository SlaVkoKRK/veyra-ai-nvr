# Veyra AI-NVR

Veyra is a local AI video monitoring/NVR system focused on efficient motion-driven object detection, Coral EdgeTPU, Intel VAAPI and Home Assistant integration.

Current CORE version: **0.8.5**.

## Update channel

Veyra 0.8.5 can check and install updates directly from the web panel:

**Ustawienia systemu → Sprawdź aktualizacje / Aktualizuj**

The panel checks the public `VERSION` file. The host-side updater downloads `dist/veyra_update.tar.gz.b64`, decodes it locally, verifies the published SHA256 and applies it only when the package `VERSION` matches the repository version and is newer than the running version.

Before installation Veyra creates backups, migrates configuration, rebuilds the CORE image, performs a health/version check and rolls back on failure.

The updater preserves local `.env`, `/config`, `/models` and `/data`.

## Web restart controls

The web panel provides:
- **Restart Core** — restarts only `ainvr-core`, leaving go2rtc running,
- **Restart Veyra** — restarts the full Veyra stack (CORE + go2rtc).

The web container does **not** receive Docker socket or root access. It writes an allowlisted request into `/data/control`, and a restricted host-side systemd bridge executes only `update`, `restart-core` and `restart-all`.

## Wind / global-motion guard

0.8.5 adds a low-cost wind guard using motion metadata that is already calculated on the 400/512 motion frame. No extra decode or optical-flow pass is added.

After sustained broad/fragmented motion (for example moving trees in wind), Veyra throttles only **new motion-discovery ROI**. Existing tracked people/cars keep their normal adaptive track cadence.

Default behavior:
- enter after ~1.5 s of sustained global/fragmented motion,
- leave after ~2 s of calmer motion,
- new motion discovery: max 2 regions per cycle, rotated,
- throttled discovery interval: ~0.6 s,
- initial motion before guard activation keeps the fast path.

Per-camera switch: `motion.wind_guard_enabled` (default `true`).

## Repository safety

This repository must never contain production configuration or runtime data. See `SECURITY.md`.

Only `config/ainvr.example.yaml` is published. It contains placeholders only. Camera credentials, MQTT credentials, models, event databases and snapshots are intentionally excluded.

## Home Assistant

The Home Assistant integration is maintained separately in:
`SlaVkoKRK/veyra-home-assistant`.
