# Veyra AI-NVR

## 0.9.6 — TFLite embedded metadata hotfix

- reads Ultralytics class names directly from `metadata.json` embedded in the `.tflite`;
- supports `TFLITE_ULTRALYTICS_METADATA.json` and legacy onnx2tf literal metadata;
- keeps associated `labels.txt` / `.names` support as an additional path;
- no detector, tracker, score, motion or notification thresholds changed.

Current CORE version: **0.9.6**.

## 0.9.5 — model-defined classes

Class names no longer need to be duplicated in `ainvr.yaml`. Veyra reads them from the active model (embedded TFLite labels or adjacent metadata/label files). The old top-level `classes:` list is accepted only as a backwards-compatible fallback.

## 0.9.4 — night FP gate without losing distant-person acquisition

- keeps class `min_score` as the single-frame tracker admission filter and `threshold` as the Frigate-style median/computed-score TP gate;
- keeps `night_weak_motion` and selective CLAHE/gamma only for weak-only night ROIs;
- a weak-night track can still alert after two hits when the second tracker pass confirms it on RAW;
- enhancement-only weak-night tracks require three coherent hits before fast prealert / TP, reducing short static-texture false positives without raising global thresholds;
- adds `raw_hits`, `night_assist_hits`, `region_source` and `origin_region_source` diagnostics;
- retains the 8x8 TP-only region grid, MQTT QoS1 reconnect outbox, black-frame watchdog, FFmpeg 7.0.2 and Debian VAAPI stack.

## 0.9.1 — updater hotfix + black-frame recovery

- fixes false `Aktualizacja nie powiodła się (kod 0)` reporting by preserving the real `apply-update.sh` exit code;
- differential GitHub payloads include `docker-compose.yml`, which is required by the update-package validator;
- includes the per-camera black-frame watchdog from 0.9.0;
- leaves FFmpeg 7.0.2 and the stable Debian VAAPI driver stack unchanged.


Veyra is a local AI video monitoring/NVR system focused on efficient motion-driven object detection, Coral EdgeTPU, Intel VAAPI and Home Assistant integration.

## 0.9.2 — Frigate FP parity + updater Docker HOME fix

- keeps night weak-motion and motion contrast, but sends the raw crop to Coral in Frigate-parity mode;
- forces Frigate 0.17.2 YOLO-generic logit score semantics;
- removes the blind fixed 3x3 startup detector sweep;
- adds a persistent 8x8 learned detector-region grid trained only from confirmed true positives;
- fast security prealerts remain immediate but are not persisted to the gallery until the normal true-positive confidence gate;
- keeps `ProtectHome=true` while moving Docker CLI HOME/DOCKER_CONFIG to `/opt/ainvr/data/control/docker-home`;
- keeps FFmpeg 7.0.2 and the stable Debian VAAPI stack.


## 0.8.9 — emergency VAAPI rollback

- Reverts only the Intel VAAPI userspace override introduced in 0.8.8.
- Keeps the proven Frigate 0.17.2 FFmpeg 7.0.2 runtime from 0.8.7.
- Restores Debian Bookworm `intel-media-va-driver` + `i965-va-driver`, which was stable on the target Skylake host.
- No changes to motion, night assist, Coral, tracking, notifications or detection cadence.
- Updater diagnostics now keep the Docker build log for future failures.

## 0.8.8 — Intel VAAPI driver parity + GitHub update channel

- Keeps the proven Frigate 0.17.2 FFmpeg 7.0.2 runtime from 0.8.7.
- Aligns Intel VAAPI userspace with Frigate 0.17.2 on amd64 by copying the pinned Frigate iHD driver and libigdgmm into the Veyra image. Debian Intel VAAPI packages remain as stable dependencies/fallback; no live Intel repository is required during updates.
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
