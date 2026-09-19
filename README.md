# Veyra AI-NVR

Veyra is a local AI video monitoring/NVR system focused on efficient motion-driven object detection, Coral EdgeTPU, Intel VAAPI and Home Assistant integration.

Current CORE version: **0.8.8**.

## 0.8.8 — Intel VAAPI driver parity + GitHub update channel

- Keeps the proven Frigate 0.17.2 FFmpeg 7.0.2 runtime from 0.8.7.
- Aligns Intel VAAPI userspace with Frigate 0.17.2 on amd64: `intel-media-va-driver-non-free`, `i965-va-driver-shaders`, `libmfx1`, `libmfxgen1`, `libvpl2`.
- `iHD` remains the default driver; `video.libva_driver: i965` stays available for A/B tests on Skylake.
- 0.8.8 is published through the in-panel GitHub updater as an atomic release payload.

## 0.8.7 — FFmpeg parity A/B

- CORE now prefers the exact static FFmpeg 7.0.2 runtime shipped in Frigate 0.17.2 (`/usr/lib/ffmpeg/7.0/bin`).
- Debian Bookworm FFmpeg 5.1 remains installed only as a fallback.
- No motion/night/Coral/tracker cadence or thresholds were changed in this version.
- Camera decoder status exposes `ffmpeg_version` so the active runtime can be verified from the API/UI diagnostics.
- Purpose: isolate the persistent idle CPU gap versus Frigate without sacrificing night motion quality.

## Update channel

Veyra 0.8.5 introduces updates directly from the web panel:

**Ustawienia systemu → Sprawdź aktualizacje / Aktualizuj**

0.8.5 is the bootstrap version: install it once from the local installer ZIP. Future releases use the public GitHub `VERSION` plus a chunked differential package from `dist/manifest.txt`. The updater joins the chunks, decodes the archive, verifies SHA256 and package VERSION, then uses the existing backup / migration / health-check / rollback flow.

Local `.env`, `/config`, `/models` and `/data` are preserved.

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

## 0.8.7 — CPU parity

- ograniczone kodowanie `notification.jpg`: pierwszy kadr natychmiast, następne domyślnie co najmniej 1.25 s;
- wspólne kodowanie JPEG dla current notification i thumbnail, gdy oba aktualizują się na tej samej klatce;
- detector IPC nie przenosi pełnego health dict przy każdym ROI;
- szybka ścieżka dla kwadratowego ROI 512 bez zbędnego letterbox canvas/copy;
- cache maski bool w motion hot-path.

Optymalizacje nie zmniejszają detect FPS, motion resolution ani czułości detekcji.
