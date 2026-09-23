# VEYRA 1.2.75 â€” UI consistency, Vision light theme, track reacquire hold and release bundle

## UI

- The live **Vision WORK** preview follows the light theme instead of keeping the old dark preview card.
- The primary **Veyra / Vision / Aktualizacje** strip fills the same available width as Gallery/Statistics strips and keeps horizontal scrolling behaviour on narrow screens.
- Existing unified strip styling from 1.2.74 is preserved across Settings, Statistics, Gallery, Logs and camera settings.

## Main camera dashboard

The dashboard now keeps a short **20 second track reacquire hold** after a confirmed object disappears:

- Smart Live is not torn down immediately when one detector sample loses the track;
- the card stays in detector/track state instead of immediately falling back to MOTION;
- the status shows `DETECT Â· Ns` while VEYRA waits for a reacquire;
- stale bounding boxes are not drawn during the hold;
- after the hold expires, normal MOTION/IDLE behaviour resumes.

This is a UI/live-session hold only. It does not add another Coral inference or change detector thresholds.

## Vision cleanup

VEYRA remains Ollama-only for Vision. The 1.2.74 cleanup is retained:

- retired `clip_backend.py` and `ov_florence2_helper.py` are removed;
- retired Florence OpenVINO model data is removed;
- retired `person-vehicle-bike-detection-2002` model data is removed;
- `vision-worker` does not mount the legacy `/models/vision` tree and does not carry Torch/Transformers/OpenVINO.

## Release workflow

Starting with this release, one **release bundle ZIP** contains everything needed to publish both update paths:

- manual VEYRA update ZIP;
- remote-update `tar.gz` used by the installed WWW updater;
- SHA256 files;
- release notes;
- manifest and `channel.json` metadata.

The reusable PowerShell publisher accepts this single release bundle, validates hashes, creates/updates the `vX.Y.Z` GitHub tag + Release and switches the production `channel.json` only after the Release assets are available.

## Live safety

The media transport remains unchanged:

```text
browser -> nginx -> go2rtc -> camera
```

No Python video relay, custom SourceBuffer, playback-rate controller or additional Coral pass is introduced.
