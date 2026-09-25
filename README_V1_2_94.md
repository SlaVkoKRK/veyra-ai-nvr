# VEYRA 1.2.94 â€” GLARE mask regression fix + earlier no-carrier alert/repeats

VEYRA 1.2.94 fixes two regressions reported after 1.2.93: the Dynamic Glare learned mask could become visibly smaller than in 1.2.92, and a real car/headlight without a visible `person/car` carrier could alert too late and only once near the end of the approach.

## Dynamic Glare â€” whole current static Recovery footprint

The 1.2.93 persisted reconstruction required a continuous `score >= fringe_threshold` path. A single lower-score grid cell could disconnect the outer halo from the learned core and shrink the purple mask.

1.2.94 uses two layers:

- persisted memory still starts from hard learned core and low-score persistent fringe;
- a short bounded bridge restores the 1.2.92 tolerance for tiny score gaps;
- the learner reuses the Recovery connected-component labels it already computes and attaches the **whole current static Recovery component** that touches learned core;
- the current Threat Guard runs first, so a moving headlamp/headlight is removed by the existing protection mask before that attachment step;
- remote/new Recovery islands remain unmasked and discoverable.

No extra full-frame resize, detector inference or second connected-components image pass is added. The attachment decision runs only on the existing 128Ă—72 learner grid and reuses labels from the already-existing Recovery CC pass.

## GLARE â€” earlier alert when Coral cannot see the carrier

The primary GLARE case is allowed to have no visible `person/car`. In 1.2.93 that path was accidentally double-confirmed:

1. collect coherent no-carrier/tiny-light history;
2. then wait for another `unclassified_confirm_hits` sequence.

At 4â€“5 MotionFusion analyses/s this added roughly another 0.6â€“0.8 s after the history gate itself, and could push the first alert close to the end of a short vehicle approach.

From 1.2.94:

- once `unclassified_ready` / `tiny_ready` has already proved the required coherent history, the same frame is considered confirmed for notification;
- carrier-present fast path keeps its existing confirmation behavior;
- lamp `OFF -> ON -> ON`, opening-door scene illumination and erratic insect/orb vetoes remain active.

## GLARE â€” repeat while the confirmed source remains blinding

A close saturated headlight can stop growing numerically even though it is still moving and blinding the camera. 1.2.93 tied the persistent-repeat fallback back to `approach_signature`, so the episode could lose readiness immediately after the first notification.

1.2.94 keeps a **confirmed continuing threat** alive while the same local motion-supported glare remains coherent. This allows the configured GLARE cooldown/repeat cadence to continue after the first alert even if area growth saturates. The continuation path is available only after the full approach-history gate has already produced the first real notification.

NIGHT_GLARE Recovery is held by the same confirmed continuing threat, so detector preprocessing does not drop out only because a saturated source stopped getting numerically larger.

## Tracking / Stationary

The 1.2.93 Stationary hardening is unchanged: a motion-origin static lookalike cannot become TP from detector confidence alone, while real local motion or tracker `position_changes` immediately releases the hold.

## CPU

The 1.2.94 GLARE fix remains scalar/grid-only:

- one existing `glare_motion_metrics()` image pass;
- no extra Coral inference;
- no optical flow;
- no second full-resolution glare analysis;
- Recovery attachment reuses the CC labels already calculated by Dynamic Glare.

On a synthetic 128Ă—72 grid in the build environment, mask refresh + attachment measured about 0.35 ms/update versus about 0.17 ms for the 1.2.93 refresh alone. This is a microbenchmark, not a direct i5-6500T CPU percentage measurement.

## Live

Protected Live files remain byte-identical to 1.2.93:

- `core/app/main.py`
- `docker-compose.yml`
- `live_proxy/default.conf.template`
- `scripts/apply-update.sh`

## Update

```bash
/opt/ainvr/scripts/apply-update.sh /path/veyra_update_v1_2_94.zip
```
