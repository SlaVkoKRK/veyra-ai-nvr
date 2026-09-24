# VEYRA 1.2.84

Dynamic Glare CPU consolidation on top of 1.2.83.

## One analysis -> many consumers

- Dynamic Glare Background no longer creates a full-resolution float32 Recovery halo and then immediately shrinks it to the 128x72 learner map.
- A new grid-native Recovery projection uses the same saturated-core threshold, quarter-resolution AREA plane, Gaussian halo and normalization as NIGHT_GLARE Recovery, but projects occupancy directly to the Dynamic Glare grid.
- Real Glare Recovery and Debug remain unchanged and still use the full-resolution Recovery helper when they actually need a full image.
- The 1.2.83 rule is preserved: Dynamic Glare learning runs only after a full MotionFusion analysis and never on `analysis_skipped` tracker heartbeats.

## Low-luma fog / steam / IR haze

- The low-luma temporal fallback is restored in a cheap grid-native form so fog, steam and IR haze without clipped white pixels can still become background evidence.
- Y is resized while still `uint8`; only the tiny 128x72 grid is converted to float32.
- Median and temporal activity are calculated on that tiny grid instead of the full 1920x1080 luma plane.
- Minimum-component filtering also runs only on the tiny grid.

## Safety

- Dynamic Motion Mask behavior is unchanged.
- Glare Threat Guard sensitivity and the 1.2.83 hard animal-notification veto are unchanged.
- No extra Coral inference, decoder or camera stream is added.
- Live transport files are unchanged byte-for-byte from 1.2.82/1.2.83.

HACS 0.3.3 remains protocol-compatible.
