# VEYRA 1.2.126

## Emitter Map / Light Threat DEBUG bootstrap

- DEBUG no longer waits forever for the first Emitter Map on quiet night scenes.
- The normal MotionFusion scheduler and one-analysis-many-consumers alarm path remain unchanged.
- When a DEBUG client is connected and the authoritative cache is still empty, VEYRA performs exactly one reduced Y-plane Light Threat bootstrap (<= configured analysis width, max 480 px).
- The bootstrap is diagnostic only: it does not update alarm history, Dynamic Static Light learning, Coral, notifications, or the Decision Gate.
- After the first map exists, normal runtime Light Threat passes remain authoritative.

## Review theme paint

- Review timeline canvas now has an explicit theme background before its first JavaScript repaint.
- Initial paint is repeated after layout/theme initialization so dark mode no longer flashes or remains white until the first click.
- Theme changes force a timeline redraw.
- Existing mouse/touch drag, time label and pinch zoom behavior is preserved.

## Recording file browser

System Settings -> Recording now includes a recording browser:

- list MP4 files from the configured VEYRA recording store only;
- filter by camera and type (continuous / event ring / event clips);
- show relative path, timestamp and size;
- open a recording in a new tab;
- delete one recording safely; the owning camera recorder is stopped briefly and restarted;
- deleting an event MP4 also removes its matching JSON sidecar;
- "Delete all recordings" stops recorders briefly, removes continuous/ring/event data and starts required recorders again;
- arbitrary filesystem paths cannot be browsed or deleted through this API.

## Compatibility / protected paths

- No changes to the protected Live architecture.
- No changes to `core/app/main.py`, `docker-compose.yml`, `live_proxy/default.conf.template`, or `scripts/apply-update.sh`.
