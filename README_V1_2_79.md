# VEYRA 1.2.79 â€” Unified Dynamic Masks Debug

## Debug: one "Maski dynamiczne" switch

Dynamic mask visualization is no longer split between the generic `Maski` overlay and the `Maska glare` image stage.

The camera Debug panel now has a dedicated **Maski dynamiczne** option.

When enabled, the current camera frame shows both adaptive maps at the same time:

- **orange** â€” Dynamic Motion Mask / Scene Guard learned motion suppression,
- **purple** â€” Dynamic Glare Background learned persistent or slowly changing bright sources.

The overlay can be used with RAW, AUTO, IR Assist, White Light and Glare Recovery views. It is diagnostics-only and never changes the detector image.

The exact `Coral 512` detector-input view intentionally stays clean, because that view must continue to represent the real tensor image given to the single Coral inference.

## Separation from permanent masks

The ordinary **Maski** option now shows only configured permanent motion/object masks and mask rules.

Dynamic Motion Mask is no longer rendered under that option.

The `Maska glare Â· Recovery` stage continues to show the real glare-recovery mask and WATCH / LOCK / ALERT threat state, but the learned persistent glare background is displayed only when **Maski dynamiczne** is enabled.

## Performance / safety

- no additional Coral inference,
- no additional decoder,
- no change to native Live transport,
- no change to dynamic-mask learning/filtering logic; 1.2.79 changes only their Debug organization and visualization.

## Validation

- pytest: **156/156 PASS**
- Python compile: PASS
- generated inline JavaScript: **20/20 `node --check` PASS**
- YAML parse: PASS
- shell syntax (`bash -n`): PASS
- protected Live files: byte-identical to 1.2.78
