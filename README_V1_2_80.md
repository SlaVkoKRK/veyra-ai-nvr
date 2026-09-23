# VEYRA 1.2.80 â€” Adaptive mask release + fast glare repeats

## Dynamic Motion Mask now actually releases

The old adaptive motion score decayed only at `0.025/min`. A fully learned cell at `1.0` could therefore remain above the active `0.50` threshold for roughly 20 minutes after motion stopped, which looked like a permanent mask in Debug.

1.2.80 adds per-cell local activity ageing:

- local motion refreshes only the cells it really touches,
- after about **8 s** without local motion the active score starts a fast release,
- the active mask normally falls below threshold within a few additional seconds,
- a soft sub-threshold memory remains for quick relearning if foliage/rain returns,
- threshold crossings are persisted so a restart cannot resurrect an already released active mask,
- Debug `Maski dynamiczne` shows `fade N` while cells are being released.

## GLARE can never teach Dynamic Motion Mask

Illumination bloom is not background motion. While local glare threat or `NIGHT_GLARE` recovery is active, Dynamic Motion Mask learning is frozen. A flashlight/headlight halo can therefore no longer paint a large orange adaptive-motion area.

This does not disable MotionFusion or Coral discovery. It only blocks learning of the adaptive suppression map during glare.

## Dynamic Glare Background learns steam / fog / broad IR bloom

The dynamic glare background no longer relies only on clipped white pixels. A second slower diffuse learner now handles broad elevated-luma regions such as:

- steam above a jacuzzi,
- fog / mist,
- broad IR bloom,
- diffuse static or slowly drifting glare.

Compact moving bright cores are still excluded from fast background learning. Diffuse moving haze needs sustained persistence, so a passing headlamp/headlight alerts long before it could become background.

## Faster glare security notifications

The detector-free Glare Threat Guard is more aggressive while keeping the bright-source requirement:

- confirm hits: **1** after coherent growth is measured,
- minimum growth: **1.06Ă—**,
- growth window: **1.4 s**,
- notification cadence while the threat remains coherent: about **1.2 s**,
- a notification is no longer one-shot for the whole glare episode,
- a dog/person/car without a bright glare component still cannot trigger a glare alert.

Every repeat uses the existing native `glare_approach` MQTT protocol, so HACS 0.3.3 remains compatible and produces a fresh Companion App alert.

## Animal / dog GLARE veto

A moving animal cannot manufacture a GLARE event from an unrelated static lamp or reflection:

- MotionFusion bbox growth alone is no longer accepted as glare growth; the bright source itself must change in size or position first,
- a tracked `dog` (and other animal classes) containing the bright component vetoes `NIGHT_GLARE` wake-up and `glare_approach` unless a plausible person/vehicle light carrier is present at the same location,
- Debug exposes the reason as e.g. `animal_veto:dog` or `static_bright_source_motion_only`,
- the veto runs before `force_glare()`, so a dog cannot switch detector preprocessing into GLARE just by moving through a bright patch.

This keeps the fast optical path independent from Coral when no object has been classified yet, while using already-existing tracks as a semantic safety veto as soon as they are available.

## Invariants

- exactly one scheduled Coral inference per ROI,
- no second detector pass,
- no new decoder,
- Dynamic Glare Background is threat-selection only and never masks detector pixels,
- protected Live transport files are unchanged.
