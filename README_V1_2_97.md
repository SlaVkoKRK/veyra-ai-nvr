# VEYRA 1.2.97

## Gesture polygon editor

- Gesture zones are now editable multi-point polygons, matching the mask editor model instead of fixed rectangles.
- Multiple polygons per camera remain supported under one logical `zone_name`.
- Existing 1.2.96 `zone` / `zones` rectangle configs migrate losslessly to four-point polygons.
- New config field `zone_polygons` is authoritative; legacy `zones` / `zone` bounding rectangles are still written for rollback compatibility.
- LMB drags a point, inserts a point on an edge, or moves the whole active polygon. RMB removes a polygon point while keeping at least three.
- The initial full-frame zone is immediately editable; no create/delete workaround is required.

## Stable draggable PERSON height reference

- The reference PERSON can be dragged anywhere over the camera image.
- Its position is UI-only and stored locally in the browser per camera.
- Height is computed only from the real detector frame height (`frame_height`, with detect config fallback).
- Refreshing the 1280 px preview JPEG no longer changes the PERSON scale, fixing the periodic height jump from 1080 â†” 720 source-height switching.
- Existing live PERSON overlays still show `height px` and `IN/OUT` against the new polygon zones.

## Performance / compatibility

- No new camera stream, polling loop, inference, OpenCV pass or image resize.
- Gesture runtime uses point-in-polygon checks only on confirmed PERSON track centers.
- Protected Live files remain unchanged.
