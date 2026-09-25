# VEYRA 1.2.101 â€” semantic Gesture pose + organized Integrations

VEYRA 1.2.101 continues directly from 1.2.100.

## Gesture v4: semantic wrist tracking

Gesture no longer has to infer the hand only from local pixel motion. After an eligible PERSON enters a Gesture polygon and becomes ARMED, VEYRA uses a short-lived semantic pose path focused on that one PERSON crop:

`POSE WRIST -> pixel_hand -> MotionFusion`

- MediaPipe Pose Lite (`model_complexity=0`) supplies shoulder / elbow / wrist landmarks.
- Pose is never run full-frame and never runs continuously for the camera.
- Pose cadence is capped at 8 FPS while Gesture is armed.
- The PERSON crop is padded above and sideways so a raised/outstretched hand remains visible.
- The crop is downscaled to at most 384 px on its longest side before pose processing.
- `pixel_hand` remains the cheap between-frame fallback.
- MotionFusion remains the final zero-extra-model fallback.
- Debug reports `HAND POSE`, `HAND PIXEL`, or `MOTION` so the active source is visible.

The MediaPipe dependency is pinned to `0.10.21` and is installed once into persistent `/data/gesture_pose_py311` by a small background bootstrap when Gesture is enabled. This avoids changing the protected Live/update transport files. Until the first bootstrap finishes, Gesture automatically continues through the existing pixel/MotionFusion fallbacks.

## Integrations UI

The Integrations screen now has a second navigation level:

- **OgĂłlne** â€” GESTURE, VISION, Home Assistant / HACS;
- **Powiadomienia** â€” notification templates and Telegram / ntfy / Pushover / Discord.

Existing feature gating remains unchanged: disabling GESTURE or VISION hides their dependent UI and disables the corresponding runtime path.

## CPU / Live safety

The core rule remains `one analysis -> many consumers`. No second Coral inference, full-frame pose pass, optical flow, or Python Live relay is added.

Protected Live files remain byte-identical to 1.2.100:

- `core/app/main.py`
- `docker-compose.yml`
- `live_proxy/default.conf.template`
- `scripts/apply-update.sh`

## Validation

Release validation is performed on the final manual ZIP and remote TAR.GZ after fresh unpack, including pytest, Python compile, generated inline JavaScript syntax, YAML parse, bash syntax, archive integrity, manifest SHA and junk-file checks.
