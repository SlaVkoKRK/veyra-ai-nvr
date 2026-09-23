# VEYRA 1.2.81

Dynamic-mask precision / responsiveness update based on real night-camera debug.

## Dynamic Glare Background v2

- Default learning grid increased from 96x54 to 128x72.
- Default learned-mask threshold increased to 0.84 and broad-mask dilation is disabled.
- Saturated components are split into compact glare points vs broad bright scene surfaces.
- Only compact hot points use the fast static learner.
- Broad diffuse glare must show slow temporal luma activity before it can be learned. This keeps bright roofs/walls out while still learning drifting steam, fog and IR veil.
- Old v1 glare-memory scores are demoted on first load so oversized masks from 1.2.80 disappear and relearn with the stricter logic.
- Dynamic glare still affects threat selection only; it never erases detector pixels.

## Dynamic Motion Mask v3

- 64x36 precision is retained, but cells learn faster: threshold 0.46, increment 0.34 and area weighting reaches full strength at 20% cell coverage.
- Repeated detector-confirmed empty motion may learn even before whole-frame Scene Guard reaches rain/wind/busy. Calm scenes require one extra miss and use reduced gain.
- After initial confirmation, consecutive empty scans refine the same cells without restarting the whole confirmation batch.
- GLARE no longer freezes the whole dynamic-motion learner. Only cells around the current local glare component/halo are excluded, so foliage/rain elsewhere continues learning.
- Debug uses the exact runtime threshold, so a learned mask can no longer exist internally while appearing as 0%/invisible in Dynamic Masks.

No extra Coral pass, no second decoder and no protected native Live transport changes.
