# VEYRA 1.2.83

Dynamic Glare CPU and animal-notification correction.

## Dynamic Glare Background

- The learner now accepts evidence only from the exact `Glare Recovery` footprint shown in Debug: saturated core plus its Recovery halo.
- The former independent temporal-luma fallback is removed. Low-luma haze that is not visible in `Glare Recovery` cannot silently become a learned glare mask.
- Learning runs only after a full MotionFusion analysis. Tracker heartbeats marked `analysis_skipped` no longer trigger a full-resolution Recovery calculation, so Dynamic Glare respects the configured motion-analysis FPS budget.
- Dynamic Motion Mask behavior is unchanged.

## Glare Threat Guard

- A local tracked animal, including `dog`, is a hard glare-notification veto unless a plausible person or vehicle overlaps the same light source.
- The veto clears accumulated glare hits and the active glare-approach state. A dog appearing after a source was already armed can no longer inherit that state and emit repeated glare notifications.

No extra Coral inference, decoder, or camera stream is added.
