# VEYRA 1.2.95 â€” Gesture Trigger trace continuity + visible confirmation

VEYRA 1.2.95 fixes the first real-world Gesture Trigger regression reported after 1.2.91: the turquoise trace could stop while the hand was still moving, Debug looked choppy, and a successfully recognized gesture was mainly visible only through MQTT/logs rather than as an explicit confirmation on the camera image.

## Why the trace stopped

The MotionTrace recognizer intentionally reused existing MotionFusion components to avoid another model/inference. In 1.2.94 it accepted only a relatively small, separate motion component inside the upper part of a confirmed `person`. A waving forearm often merges with torso motion, and that merged component could be rejected by the `max_motion_box_person_fraction` guard. The recognizer therefore received only some hand positions and the visible path ended early.

1.2.95 keeps the zero-extra-image-pass architecture but adds a second cheap geometry path:

- small standalone upper-body motion still uses its centroid;
- moderately merged arm+torso motion generates left/right peripheral edge hypotheses;
- temporal continuity chooses the most plausible hand-side hypothesis;
- a coherent small component is allowed through the centre of the upper body once a trace already exists, because a real wave/circle necessarily crosses the middle.

No pixels, resize, optical flow, MediaPipe, OpenVINO or extra Coral call are added.

## Gesture sampling FPS

Adaptive MotionFusion normally drops a confirmed stationary track to about 2 fps. That is useful for normal object tracking, but too sparse for a 2â€“3 second hand trajectory.

From 1.2.95, only while a real confirmed person is armed in the configured Gesture Zone, MotionFusion temporarily returns to the camera detector FPS (normally 5 fps). When no gesture is armed, the previous adaptive FPS behavior is unchanged. Debug already forced detector FPS, so this mainly fixes real background operation with no Debug client open.

## Debug lifecycle

Debug now makes the Gesture Trigger state explicit:

- `GESTURE ARMED Â· czekam na ruch rÄ™ki`
- `GESTURE TRACKING Â· pts N`
- `GESTURE CANDIDATE Â· WAVE h1/2 82% Â· pts N`
- `GESTURE âś“ WAVE 86%`

After a successful gesture the final trajectory is retained and drawn in green for about 1.8 s instead of being cleared immediately. MQTT publication and the existing `GESTURE ...` log entry remain the actual automation output.

The MJPEG Debug endpoint still intentionally defaults to 4 fps, so the video preview itself can look less smooth than Live. This does not reduce the internal gesture sampling, which can run at detector FPS while armed.

## CPU

Baseline cost remains unchanged when gestures are disabled or no person is armed. During an armed gesture, MotionFusion may temporarily run at detector FPS and the recognizer performs only bbox geometry/math on existing motion components. No additional image inference is introduced.

## Live

Protected Live files remain byte-identical to 1.2.94:

- `core/app/main.py`
- `docker-compose.yml`
- `live_proxy/default.conf.template`
- `scripts/apply-update.sh`

## Update

```bash
/opt/ainvr/scripts/apply-update.sh /path/veyra_update_v1_2_95.zip
```
