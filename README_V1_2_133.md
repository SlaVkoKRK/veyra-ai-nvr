# VEYRA 1.2.133

## DEBUG isolation / camera Broken pipe fix

- Fixed `name 'now' is not defined` in the DEBUG state renderer.
- Light Threat analysis age now uses a renderer-local timestamp.
- Added a hard DEBUG isolation wrapper: **no DEBUG rendering exception can terminate the camera worker or close the SHM/rawvideo consumer**. A renderer failure is logged while capture, Motion and Coral continue.
- This prevents the downstream FFmpeg `Broken pipe` sequence caused by the 1.2.132 DEBUG exception.

## Control queue recovery

- The control path now uses `PathExists` so a request left on disk is retried after service/daemon recovery instead of remaining permanently queued.
- `ainvr-control.service` no longer has a hard `Requires=docker.service` dependency. The bridge can start, report a Docker-down error and clear its work item.
- Docker-only actions fail fast with an explicit status and release the queue.
- `restart-all` remains available to the control bridge independently of a direct Docker dependency.

No Light Threat thresholds, IR Transition Guard, Coral path, VAAPI graph or native Live transport are changed.
