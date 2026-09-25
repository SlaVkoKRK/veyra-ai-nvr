# VEYRA 1.2.100

## Gesture v3 â€” extended hand search envelope

- Fixes the main 1.2.99 failure where a hand raised clearly above the head or stretched beside the body could leave the local `pixel_hand` crop entirely.
- The armed-only local tracker now searches roughly 50% of PERSON height above the detector bbox and 55% of PERSON width beyond each side, while still analysing only a small normalized crop.
- Local analysis size is 192Ă—192 and still runs only after `GESTURE ARMED`; no permanent full-frame hand pipeline, pose model or second Coral inference is added.
- Candidate scoring now rewards coherent motion outside the PERSON bbox, where a real raised/outstretched hand often appears.
- The absdiff tracker now distinguishes the currently occupied hand location from the just-vacated location, reducing one-frame-behind traces during WAVE.
- MotionFusion remains the fallback source when `pixel_hand` cannot produce a usable point.

## Feature Integrations â€” GESTURE / VISION

- `Ustawienia â†’ Integracje` now contains global `GESTURE` and `VISION` ON/OFF feature switches with descriptions.
- GESTURE OFF is a real runtime master gate: the camera gesture recognizer is disabled even if an older per-camera `detect.gestures.enabled` value is still present.
- When GESTURE is OFF, Gesture Trigger controls disappear from the camera `Zaawansowane` panel.
- VISION OFF writes `vision.enabled=false` and hides the Vision settings tab, prompt UI and Vision controls/filters in Gallery.
- When the new feature flags do not exist yet (upgrade from 1.2.99), both default to ON so the update does not silently disable an existing setup.

## Compatibility / Live

- No change to the protected Live transport: `browser â†’ nginx â†’ go2rtc â†’ camera`.
- Existing gesture polygons, MQTT topic/payload, camera configuration and historical Gallery metadata remain compatible.
- No MediaPipe / MoveNet / additional pose model was added in this release. A conditional pose tracker remains a fallback option only if real-camera tests show Gesture v3 is still insufficient.
