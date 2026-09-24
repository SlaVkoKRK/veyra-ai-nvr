# VEYRA 1.2.85

GLARE false-positive hardening, diagnostic snapshots and per-camera control on top of 1.2.84.

## Lower false-positive risk

- Glare Threat Guard now advances only on a real MotionFusion analysis frame; cached `analysis_skipped` heartbeats cannot count the same motion/light evidence again.
- A coherent glare threat requires two confirmation hits instead of one.
- Local light-source change is slightly stricter (`source_min_growth` 1.04, centroid shift 0.004).
- The old 8-second generic night grace is reduced to a short 0.8-second bridge intended only for the moment when a real headlight/headlamp pushes the global luma classifier toward DAY.
- Existing animal veto, Dynamic Glare Background veto and one-Coral-per-ROI rule remain intact.

## NIGHT-only Dynamic Glare

- Dynamic Glare Background learns only in trusted NIGHT_IR / NIGHT_WHITE_COLOR context.
- It does not learn on DAY frames and its purple debug overlay is hidden during DAY.
- Learned night memory is retained across the day so it does not need to relearn fixed lamps/fog landmarks every evening.
- Normal Dynamic Motion Mask behavior is unchanged.

## GLARE snapshots in Gallery

- A confirmed Glare Threat Guard alert creates one normal Gallery event with label `glare`.
- The event stores a clean frame, annotated review image and notification thumbnail with glare bbox and diagnostics.
- Repeated ~1.2 s alerts in the same coherent glare episode reuse the same Gallery event instead of generating duplicate snapshots.
- GLARE events are explicitly marked `Vision: skipped / glare_event`; they have no Coral-512 verifier source.
- Automatic Vision enqueue and manual `PonĂłw Vision` retry both hard-block the `glare` label.
- Gallery hides Vision TP/FP/AUTO/retry controls for GLARE and shows the glare diagnostics instead.

## Per-camera switch

Camera settings -> **Zaawansowane -> Glare Guard dla tej kamery**.

Disabling it for one camera blocks:

- automatic NIGHT_GLARE,
- Glare Recovery modifier,
- Dynamic Glare Background learning/use,
- Glare Threat Guard alarms,
- GLARE snapshots.

It does not change Live or ordinary object detection.

## Live / detector safety

- No additional Coral inference is introduced.
- No second decoder or stream is introduced.
- Live transport is untouched.
- Protected Live files remain byte-identical to 1.2.84.

HACS 0.3.3 remains protocol-compatible.
