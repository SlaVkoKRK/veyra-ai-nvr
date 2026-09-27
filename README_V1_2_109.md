# VEYRA 1.2.109

Static FP hardening for vegetation-like PERSON detections and Gallery metadata cleanup.

## Static FP: internal motion vs whole-object translation

A fixed object such as grass, leaves or a plant can generate substantial MotionFusion activity inside a stable YOLO PERSON box. Earlier releases could treat that local deformation as enough evidence that the detected object was moving.

1.2.109 adds an `internal_motion_only` classifier for initializing PERSON tracks. It uses the existing detector-box history only (no extra inference or image pass):

- median IoU of consecutive detector boxes,
- normalized center spread,
- bbox area stability,
- coherent net bbox travel.

If the bbox remains spatially anchored while local motion occurs inside it, the candidate remains in `stationary_hold_internal_motion` instead of becoming a true-positive event. A genuinely walking person still releases the hold immediately through coherent bbox displacement. Startup detection and already-confirmed stationary persons remain unchanged.

Default internal-motion release is conservative: unusually broad local motion can still confirm a fixed box, protecting a real person who appears nearly stationary.

## Gallery debug

- Event details now report `internal-motion`, `moving`, `active` or `stationary` instead of calling every non-stationary tracker state `moving`.
- `Maski: undefined` is fixed: mask checks display the configured mask name/id and their real PASS/CLOSE/REJECT status.
- Event metadata includes `coherent_displacement` and `internal_motion_only` for later diagnosis.

## Regression coverage

Added a regression case modeled on a NIGHT_IR plant/grass false-positive: PERSON confidence around 78%, repeated position-change hints and local motion, but no real bbox translation. The candidate must remain blocked. A coherent walking-person trajectory remains allowed.
