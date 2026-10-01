# VEYRA 1.2.141

> **Light Threat / Static Light hotfix over 1.2.140.** This release addresses the entrance-camera false positives where a motion-sensor porch light creates a genuine broad scene illumination step and a coincident local reflection (for example on a dog, insect or wet/IR-reflective surface) incorrectly borrows that remote-light evidence.

## Root cause
The 1.2.140 physical-emitter gate correctly rejected local spider/web reflections that had no meaningful 2-D remote consequence. A motion-sensor house light is different: it really illuminates a large part of the scene, so the remote/washout measurements are physically valid. The remaining bug was **causal attribution** â€” the broad illumination from a known fixed lamp could validate an unrelated moving local GLARE candidate.

The manual **Static Light Mask** also remained primarily a bbox-overlap prior, so a small mask around the fixed lamp could not suppress a very large GLARE bbox covering most of the illuminated scene.

## Static Light Mask becomes a trusted scene anchor
- Existing `manual_glare_masks` storage remains compatible.
- Local bbox overlap remains available as the existing stationary-light prior.
- In 1.2.141 the polygon additionally marks a **trusted fixed light / illuminated scene anchor**.
- When the existing Illumination Guard sees a broad exposure/illumination step and the masked region changes coherently, VEYRA marks it as an expected fixed-light transition.
- The transition is vetoed **before approach/growth/history can qualify it** with reason:
  - `trusted_static_scene_light:<mask name>`
- This veto is scene-anchor based and does **not** depend on the GLARE bbox covering 70% of the manual polygon, so a huge scene-wide bbox cannot bypass it.

## Fixed-light scene compensation
After a short stable dwell (default about 0.55 s), VEYRA captures the signed effect of the fixed lamp on the existing tiny Light Threat luma/contrast grid:
- the current local source/exclusion zone is not absorbed into the template,
- the fixed lamp contribution is subtracted from the effective NIGHT_IR luma baseline,
- the corresponding contrast change is also compensated,
- remote illumination and Camera Washout are then measured only on the **residual** scene change.

Result: once the porch light is ON, a dog eye/fur reflection, insect or other local bright object cannot use the porch lamp's broad illumination as its own physical-emitter proof. It must create additional remote illumination/washout itself.

When the masked fixed-light area returns close to the canonical NIGHT_IR state, the temporary compensation is released automatically. This works for both OFFâ†’ON and ONâ†’OFF direction because the captured scene delta is signed.

## Real Light Threat remains enabled
This is not a permanent exclusion of the masked scene:
- an additional flashlight/headlamp/headlight above the compensated porch-light state can still produce a new qualified remote field and alarm normally,
- a previously confirmed Light Threat is not cancelled merely because the fixed-light transition is detected,
- no dog/animal tracker is used by Light Threat.

## DEBUG / Gallery
Light Threat diagnostics now expose:
- Static Light transition YES/NO,
- compensation active YES/NO,
- mask/anchor name,
- masked luma delta,
- masked hot fraction,
- existing raw/qualified remote field, sector, washout and nuisance metrics.

Live DEBUG also shows `STATIC STEP` while a trusted fixed-light transition is being blocked and `STATIC COMP` while its learned scene contribution is being subtracted.

## Performance / architecture
- Reuses the existing reduced Light Threat luma/contrast grid.
- No second decode.
- No second Coral inference.
- No new full-frame resize.
- No optical flow.
- No heavy model.
- Existing one-analysis â†’ many-consumers architecture is preserved.
- `core/app/main.py`, `docker-compose.yml` and `live_proxy/default.conf.template` remain unchanged.

## Targeted validation
- Motion-sensor fixed lamp: broad remote/washout is immediately marked as trusted Static Light transition.
- Stable fixed lamp: scene contribution is learned on the tiny grid and remote/washout falls back to residual-only evidence.
- Local dog/reflection candidate during the lamp step cannot alarm even with a huge GLARE bbox.
- Returning to the original lighting state releases the temporary compensation.
- A real additional multi-sector emitter above the compensated fixed lamp still qualifies.
- Existing spider/web nuisance gate, Remote Illumination, Camera Washout, HA notification/event separation, risk-zone/updater and static-light tests remain green.
- **51 targeted tests passed.**
- Python compile/compileall, shell `bash -n`, YAML parsing and changed Gallery JavaScript `node --check` passed.
- Release package manifests and archive integrity are verified during packaging.
