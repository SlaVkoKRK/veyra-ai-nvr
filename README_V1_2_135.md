# VEYRA 1.2.135

## Light Threat â€” optical approach gate

- Motion overlap remains a localisation prerequisite only; it no longer contributes independent confidence to the Light Threat alarm score.
- Brightness/scale growth is no longer accepted as an approach fact by itself for an unclassified no-carrier source.
- No-carrier growth/brightness now requires current local environmental-light evidence from the same reduced Light Threat analysis.
- Coherent translation keeps its fast path, but in the production optical schema it must also have current local light impact/bloom evidence.
- Person/car carrier support retains the low-latency path so real security events are not delayed unnecessarily.

## Edge / corner reflection veto

- Added a small-source edge/corner veto for IR/lens/insect reflections that touch the frame boundary.
- The veto is not a blanket border mask: a real clipped headlamp/headlight can pass immediately when it produces strong local light impact/bloom.
- Diagnostic reason: `edge_reflection_veto`.
- The exact reported false-positive geometry `[40,992,92,1080]` at a 1920x1080 bottom edge is covered by regression tests.

## Adaptive Static Light

- A stale/false Decision-Gate notification no longer prevents a fixed Emitter Map hotspot from learning forever.
- Stationary learning is blocked by actual geometric movement / carrier support rather than merely by the fact that an alarm was emitted.
- Default stationary Emitter Map learning time is 6 s, allowing several fast repeat alarms before a truly fixed hotspot is absorbed.

## Alarm cadence preserved

- No one-event latch, long cooldown, or re-arm-after-clear policy was added.
- The native coherent Light Threat repeat remains approximately every 1.2 s while the current source continues to pass the Decision Gate.

## Preserved safeguards

- IR Transition Guard remains unchanged (~8 s NIGHT_IR settle).
- Decision Gate remains mandatory for every Light Threat alarm.
- GLARE Recovery / NIGHT_GLARE history cannot independently emit an alarm.
- Animal classes / animal tracking are not used by Light Threat decisions.
- One image analysis feeds Light Threat, Adaptive Static Light, Debug and the other consumers; no extra Coral pass, decoder, optical flow or full-frame image pass was added.
- Protected Live/deployment files remain unchanged.
