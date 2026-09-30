# VEYRA 1.2.134

## Light Threat â€” tiny FP / insects / IR reflections

- Tiny no-carrier Light Threat candidates can no longer qualify from brightness or scale growth alone.
- Production Light Threat now requires measurable environmental-light evidence for tiny sources; coherent translation may use the ordinary impact floor so distant real headlights/headlamps remain detectable.
- Added an observed-bloom metric from the already reduced luma plane. No extra full-frame resize, detector inference, optical flow or connected-components pass was added.
- Local light impact now compares the near ring around the source with a surrounding local baseline annulus instead of the global scene median. This rejects bright local surfaces, dog-eye IR reflections and insects that are bright themselves but do not illuminate their surroundings.
- Animal classes / animal tracking are still not used by Light Threat decisions.

## Adaptive Static Light â€” persistent Emitter Map learning

- Adaptive Static Light now learns persistent stationary Emitter Map footprints as well as the legacy Glare Recovery footprint.
- A fixed hotspot that remains in the same place for a few seconds can therefore become learned even if it never appears as a useful Recovery footprint.
- MotionFusion nuisance motion over a confirmed stationary hotspot no longer prevents that hotspot from learning.
- A current Decision-Gate threat, carrier-supported source or coherent translating emitter is protected from learning.
- Learning uses the same reduced Light Threat core/halo planes and preserves the one-analysis principle.

## Static Light Mask / learned hold

- Manual Static Light Mask is now an anchored hold once its configured overlap threshold is met.
- Strongly learned Adaptive Static Light regions are also held when the current source is mostly known and has little/no novel footprint.
- Brightness growth, scale growth or a white-LED/AE step cannot by themselves escape the hold.
- Escape requires coherent geometric translation plus current optical evidence, so a real moving headlight/headlamp can still cross a known static-light area.
- Diagnostic reasons now distinguish `manual_static_light_hold`, `adaptive_static_light_hold`, `tiny_reflection_veto` and existing reflection/scene vetoes.

## Preserved safeguards

- IR Transition Guard remains unchanged (~8 s NIGHT_IR settle; Light Threat history cleared; Adaptive Static Light learning paused; Motion/Coral continue).
- Decision Gate remains mandatory for every Light Threat alarm.
- GLARE Recovery / NIGHT_GLARE history cannot independently emit an alarm.
- Native Live transport and protected deployment files are unchanged.
