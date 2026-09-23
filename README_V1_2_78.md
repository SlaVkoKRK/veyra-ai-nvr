# VEYRA 1.2.78 â€” Dynamic Glare Background + Dynamic Motion Mask v2

## Dynamic Glare Background

VEYRA learns persistent or very slowly moving bright points in the night scene and excludes them from Glare Threat Guard discovery.

Typical examples:

- fixed porch / garden lamps,
- IR reflections,
- distant fixed lights,
- moon / other very slowly changing bright points.

The background map is learned only while a bright point is not supported by MotionFusion. A person with a headlamp or an approaching vehicle therefore does not teach the glare background while it is moving.

The map is stored as a small normalized grid and decays when a learned light disappears. It is persisted under `/data/cache/glare-guard/` so a CORE restart does not immediately forget the scene.

Important: the dynamic glare mask affects only **Glare Threat Guard candidate selection**. It never removes or paints over pixels sent to the detector. The single Coral pass still receives the real NIGHT_GLARE preprocessing result.

### Debug

`Debug -> Maska glare` now shows the learned background in **purple** with:

- `DYNAMIC GLARE MASK x.x%`,
- current WATCH / LOCK / ALERT state,
- threat bbox, growth and MotionFusion overlap.

This makes it possible to verify that a fixed lamp is learned while a moving flashlight/headlight is still treated as a threat.

## Dynamic Motion Mask v2

The old adaptive Scene Guard map used a 32x18 grid. A motion box touching a grid cell caused the whole cell to learn at full strength, and filtering counted touched cells instead of the real covered area. On 1920x1080 this could produce visibly large square blind areas.

1.2.78 changes that behavior:

- default grid: **64x36** instead of 32x18;
- old learned 32x18 memory is migrated by interpolation instead of discarded;
- learning is **area weighted** â€” a box barely touching a cell only adds a tiny amount to that cell;
- filtering uses exact box/cell intersection area instead of counting whole cells;
- default reject overlap rises to **72%**, so a partially unmasked object is much less likely to be suppressed;
- foreground rescue is more permissive for small / narrow person or vehicle motion;
- even a fully learned area gets one bounded **rescue detector ROI every ~1.5 s** so the dynamic map can never become a permanent blind spot;
- the rescue is part of the normal scheduled detector work and does not create a second Coral pass.

### Debug

Dynamic motion memory is now drawn as one contiguous outlined field instead of large individual rectangles. The label shows:

- coverage,
- active lighting profile,
- actual grid size,
- number of rescue probes.

## Performance / safety

- no extra Coral inference,
- no additional decoder,
- no change to native Live transport,
- dynamic glare learning runs on a tiny normalized hot-pixel map,
- dynamic motion remains active only for new motion discovery; existing tracker ROIs are never masked.
