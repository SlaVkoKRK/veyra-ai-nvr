# VEYRA 1.2.130

## Emitter Map â€” visual confidence ladder

- Weak transient optical candidates are retained for analysis but rendered almost invisibly.
- Active, not-yet-qualified candidates use a restrained amber overlay.
- Surface / IR-reflection veto candidates remain neutral grey.
- Red remains reserved for a qualified optical emitter only.
- The change is display-only: it does not lower Light Threat thresholds or alter the 1.2.129 tracker-independent IR-reflection policy.
- IR Transition Guard remains unchanged.

## Per-camera runtime controls in Live / Debug

- The single-camera view now exposes CAM / MOT / DET / SNP using the same runtime API and the same state colors as the home camera cards.
- REC and SCN remain status indicators in the camera view.
- State is refreshed from `/api/status`, so home and camera views cannot drift into separate control states.
- Controls are available on desktop and mobile.

Protected Live architecture and protected deployment files are unchanged.
