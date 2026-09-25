# VEYRA 1.2.99

## Gesture v2 â€” real local motion source

- `GESTURE ARMED` no longer relies only on global MotionFusion rectangles. While an eligible confirmed PERSON is inside a gesture zone, VEYRA lazily reads the current shared BGR frame and analyses only a normalized 160Ă—160 upper-body crop.
- The local pixel tracker follows the currently visible moving hand/forearm component. MotionFusion remains a fallback when the local tracker has no usable component.
- The BGR frame is requested only while an eligible PERSON is actually armed; Gesture disabled / no PERSON / PERSON outside the zone keeps the previous zero-pixel hot path.
- No MediaPipe, pose model, OpenVINO model or second Coral inference was added.
- Wave/circle separation is stricter: circle now requires meaningful motion/reversal in both axes and a bounded X:Y range ratio, reducing `WAVE â†’ CIRCLE` mistakes.
- Debug shows the active point source as `HAND PIXEL` or `MOTION`. MQTT `source` is `pixel_hand` when the local tracker produced the recognized trajectory.

## Gesture Zone editor â€” Debug-style layout

- The camera workspace is now the large left-hand column, matching the Debug workflow.
- All Gesture Trigger controls are in a compact right-hand panel: enable, visualization toggles, confidence/window/cooldown, minimum PERSON height, zone name, polygon tools, point coordinates and allowed gestures.
- The preview is sticky on desktop and the controls panel has its own vertical scroll, so the camera stays visible while editing options.
- On narrower screens the layout stacks vertically: preview first, controls second.
- Existing polygon editing, movable reference PERSON and source-coordinate geometry are unchanged.

## Compatibility / protected Live

- No changes to the protected Live architecture.
- Existing `zone`, `zones` and `zone_polygons` camera configuration remains backward-compatible.
- Gesture MQTT topics and payload fields remain backward-compatible; only `source` can now report `pixel_hand` for the new local tracker path.
