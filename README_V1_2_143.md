# VEYRA 1.2.143

> **Object Mask threshold + camera Mask UI + DEBUG text hotfix over 1.2.142.**

## 1. Object Mask â€” per-mask overlap threshold is now authoritative
The camera UI has long allowed a separate **Reject bbox overlap** percentage for every Object Mask. The saved camera config correctly contained entries such as `reject_overlap_percent: 45`, but the runtime compatibility parser flattened `objects.mask` / `objects.filters.<class>.mask` to polygons and silently replaced the per-mask value with the legacy global 90% default.

That explains production DEBUG such as `Maska ALL 2: 56.48% / 90% â†’ PASS` even though the mask had been configured near 45%.

1.2.143 preserves metadata for each legacy/UI mask entry before compiling mask integrals. Existing saved masks do **not** need to be redrawn or resaved: after the update, a stored 45% threshold is evaluated as 45%. The same fix applies to masks for ALL objects and per-class masks. Plain historical polygon-only syntax continues to inherit the old global/per-class threshold.

## 2. Camera settings â†’ Masks uses the full page width
The Masks tab no longer switches the camera settings grid into the old `390px + preview` two-column layout. The camera preview is stacked above the editor and the settings panel remains full-width, matching the other camera settings tabs. Mobile behaviour is preserved.

## 3. Light Emitter Map DEBUG no longer shows `??` for Unicode text
OpenCV Hershey fonts are ASCII-only. Unicode separators, dashes, arrows, checkmarks and Polish diacritics in DEBUG labels were rendered as literal question marks. All OpenCV camera DEBUG text is now normalised centrally to readable ASCII before measuring/drawing it, so Light Emitter Map descriptions use normal separators instead of `??`. This also fixes the same rendering defect in other camera DEBUG overlays.

## Updater / runtime version
The 1.2.142 candidate-runtime identity fix is retained unchanged. 1.2.143 still reports its baked/candidate runtime version before the two-phase `/opt/ainvr/VERSION` commit, and Coral/iGPU health remain mandatory when present.

## Architecture
- No second decode.
- No second Coral inference.
- No full-frame additional resize.
- No optical flow or heavier model.
- `docker-compose.yml` and `live_proxy/default.conf.template` are unchanged.

## Targeted validation
- Stored per-mask thresholds (ALL + per-class) survive the compatibility parser.
- Masks settings stay one-column/full-width and preview is stacked above controls.
- OpenCV DEBUG strings are ASCII-normalised before `getTextSize` / `putText`.
- Candidate runtime version regression from 1.2.142 remains covered.
- Python compile, shell syntax and YAML parse are checked during release packaging.
