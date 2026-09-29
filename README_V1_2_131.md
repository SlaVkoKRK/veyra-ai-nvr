# VEYRA 1.2.131

## Equal per-camera control geometry

- Home camera cards now keep CAM / MOT / DET / SNP / REC / SCN in one row.
- All six controls use the same fixed width and height, including REC and SCN status badges.
- The single-camera Live / Debug view uses the exact same control geometry.
- Mobile uses a smaller but still identical six-control row.

## Mobile Live zoom buttons

- The `+`, `-` and `100%` camera zoom controls now isolate touch gestures from the browser viewport.
- Repeated taps no longer trigger browser double-tap/page zoom.
- Button clicks explicitly suppress their default/double-click gesture while the existing camera zoom/pan implementation remains unchanged.
- Pinch zoom directly on the camera image remains available.

No Live transport, detector, Light Threat, IR Transition Guard or protected deployment-file behavior changed.
