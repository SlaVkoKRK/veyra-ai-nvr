# VEYRA 1.2.98

## Mask editor UI

- Fixed the Mask editor background in the light theme. The canvas surround and image-editor surfaces now follow VEYRA light-theme surfaces instead of remaining black.
- Gesture zone editor surfaces use the same light-theme treatment for consistency.
- The Mask preview is now a compact sticky panel on desktop. The camera frame is rendered with `contain` and capped to 58vh / 620 px so the full frame and toolbar remain visible without repeated vertical scrolling.
- On mobile the preview is non-sticky and uses a lower viewport-height cap.
- This is presentation-only: the canvas still keeps the source image pixel dimensions, so mask coordinates, hit-testing, manual bbox overlap and saved polygons retain their existing precision.

## Compatibility / protected Live

- No changes to Live architecture.
- No changes to mask semantics or camera processing.
- Upgrade remains backward-compatible with existing camera configs.
