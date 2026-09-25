# VEYRA 1.2.96 â€” multi-zone Gesture editor + PERSON scale preview

VEYRA 1.2.96 expands Gesture Trigger configuration without changing the detector/video hot path.

## Multiple Gesture zones per camera

A camera can now have up to 16 rectangular Gesture areas under one logical `zone_name`. A confirmed PERSON arms Gesture Trigger when the centre of its bbox is inside **any** configured rectangle.

The configuration is backwards-compatible:

- legacy `zone: [x1,y1,x2,y2]` is loaded as one rectangle,
- new configurations use `zones: [[...], [...]]`,
- the first rectangle is mirrored to legacy `zone` when saving so older tools still have a sensible fallback,
- per-camera legacy `zone` continues to override global/default zones during migration.

MQTT stays simple: all rectangles share one logical zone name and events still expose that value as `zone`.

## Visual zone editor in Camera â†’ Advanced

The old four-number-only editor is replaced by a camera-image editor while retaining exact percentage fields for the active rectangle.

Controls:

- **+ Rysuj strefÄ™** â€” drag a new rectangle directly on the camera preview,
- select `Strefa 1 / 2 / 3â€¦`,
- drag a rectangle to move it,
- drag corner handles to resize it,
- **UsuĹ„ strefÄ™**,
- **CaĹ‚y obraz** for the active rectangle,
- exact Left / Top / Right / Bottom percentages.

The editor paints from the existing camera `preview.jpg`; it does not create a second MJPEG Debug stream.

## PERSON height visualization

`PokaĹĽ min. wysokoĹ›Ä‡ PERSON` draws a reference figure/ruler using the real `Min. wysokoĹ›Ä‡ person [px]` value and current source frame height. This makes the distance cutoff visible instead of forcing the operator to guess what e.g. 140 px means in the scene.

`PokaĹĽ wykryte PERSON` overlays current PERSON bboxes from the already existing `/api/live-state` refresh. Each bbox shows confidence, height in source pixels and `IN/OUT` depending on whether its centre is inside any Gesture zone.

No additional live-state polling loop was added.

## CPU / architecture

Gesture recognition remains MotionTrace-only:

- no MediaPipe,
- no OpenVINO pose model,
- no extra Coral inference,
- no additional image resize or optical flow,
- no extra camera/debug stream for the zone editor,
- runtime zone membership is only a short list of rectangle comparisons.

The protected Live path is unchanged.
