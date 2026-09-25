# VEYRA 1.2.102 â€” strict Vision gating + Gesture health logs

VEYRA 1.2.102 continues directly from 1.2.101 and fixes feature-gating regressions introduced with the new Integration switches.

## Vision OFF is now UI-consistent

When **Integracje â†’ OgĂłlne â†’ Vision** is OFF:

- Gallery no longer references an undefined `visionFeatureEnabled` variable;
- Vision TP / FP / ? badges disappear from the main page and Gallery thumbnails;
- Vision filters and the Vision Verify side panel are hidden;
- the **Vision** settings tab and prompt editor are hidden before tab initialization, including direct `?tab=vision` navigation;
- the **VISION AI** log tab is hidden;
- existing historical Vision metadata remains stored and becomes visible again if Vision is enabled later.

The runtime remains disabled through the existing `vision.enabled=false` written by the Integration control. The `state=disabled` diagnostic means the Vision worker thread is not armed/running.

## Gesture operational log

When **Gesture** is enabled, **Logi** gets a dedicated **GESTURE** tab. It filters the existing Core log, so no protected Live route or updater file is changed.

Gesture now writes explicit lifecycle messages:

- `GESTURE INIT` with pose/pixel state;
- `GESTURE POSE bootstrap installed` after MediaPipe dependency installation;
- `GESTURE POSE ready backend=mediapipe model=pose-lite` when semantic wrist tracking is ready;
- `GESTURE POSE bootstrap failed ...` with the error when the pose dependency cannot start.

The pose bootstrap now immediately initializes the semantic tracker after a successful install instead of waiting for the first gesture frame.

## Live safety

The protected files are byte-identical to 1.2.101:

- `core/app/main.py`
- `docker-compose.yml`
- `live_proxy/default.conf.template`
- `scripts/apply-update.sh`

Native Live remains `browser -> nginx -> go2rtc -> camera`.
