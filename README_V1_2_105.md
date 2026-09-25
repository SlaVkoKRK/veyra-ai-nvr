# VEYRA 1.2.105 â€” Stationary jitter hardening

This release continues directly from VEYRA 1.2.104 and hardens the existing Stationary / Static FP guard without changing detector confidence thresholds, Coral inference cadence, Live transport, or confirmed-stationary rechecks.

## Static FP regression analysis

Comparison of 1.2.93 and 1.2.104 confirmed that the core `stationary_hold_no_local_motion` and `stationary_hold_sparse_motion` logic remained unchanged. The weak point was the release condition: a single tracker `position_changes > 0` or `init_moved=True` was accepted as movement evidence. Small YOLO bbox jitter on vegetation can therefore release a static false positive even though the candidate never actually travels through the scene.

## 1.2.105 movement evidence

Before a motion-origin candidate becomes a TP, tracker movement flags are now hints only. VEYRA verifies recent detector-box history and requires coherent normalized displacement:

- meaningful net centre displacement relative to bbox size;
- sufficient path length;
- directional coherence, so back-and-forth 1â€“2 px jitter does not count as real travel.

Defaults are intentionally conservative and configurable internally:

- `static_fp_real_motion_min_net_ratio`: 0.18
- `static_fp_real_motion_min_coherence`: 0.55
- `static_fp_real_motion_min_path_ratio`: 1.2 Ă— net threshold

Real coherent local MotionFusion can still confirm a candidate even if its detector bbox remains nearly stationary. A person that has already been confirmed and then stops remains handled by the existing Stationary/recheck lifecycle; this change only affects pre-TP promotion.

## Regression coverage

Added focused tests for:

- vegetation-like bbox jitter staying in `stationary_hold_no_local_motion`;
- coherent walking-person bbox travel releasing the hold;
- sparse grass motion plus jitter staying in `stationary_hold_sparse_motion`;
- coherent local motion confirming a nearly stationary bbox.

## Live safety

The protected Live files are unchanged from 1.2.104:

- `core/app/main.py`
- `docker-compose.yml`
- `live_proxy/default.conf.template`
- `scripts/apply-update.sh`
