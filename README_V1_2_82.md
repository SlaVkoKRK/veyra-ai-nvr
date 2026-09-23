# VEYRA 1.2.82

Glare Recovery / Dynamic Glare Background synchronization update based on real fog/steam night-camera tests.

## Dynamic Glare Background v3

- Dynamic Glare Background now learns from the **same saturated-core + halo footprint used by the real NIGHT_GLARE Recovery stage**.
- If fog/steam/IR haze is visibly present in `Glare Recovery`, the background learner can now accumulate that same footprint instead of waiting only for the older temporal-luma heuristic.
- The previous temporal diffuse learner remains as a fallback for low-luma IR haze without clipped white pixels.
- Default commit threshold is reduced to `0.55` and Recovery-backed learning is intentionally fast: roughly 1.3â€“1.8 s for persistent background evidence at the default cadence.
- Compact moving saturated sources are locally excluded from learning so an approaching headlamp/headlight does not get painted into the purple background map.
- Dynamic-mask Debug now renders the in-progress glare score with a faint purple heatmap before the hard learned threshold is reached. The footer exposes `learned`, `learning`, and current `Recovery` coverage separately.

## Fog/background notification spam guard

- A soft background-evidence guard uses the Dynamic Glare score before the hard mask is committed.
- Persistent fog/steam can therefore stop repeated `glare_approach` pushes after it demonstrates stable background behaviour instead of waiting for many 1.2-second repeats.
- A genuinely approaching source bypasses the soft guard when the bright source itself shows sufficient growth or translation.

## Safety / recall

- A learned fog/background mask suppresses the persistent Recovery halo, but a **fresh saturated core with MotionFusion support is preserved**. Headlights/headlamps entering an already learned fog area therefore remain eligible for Glare Threat Guard.
- Dynamic glare still affects threat selection only. It never erases pixels from the Coral detector input.
- No extra Coral inference, no second decoder and no native Live transport changes.

HACS 0.3.3 remains protocol-compatible.
