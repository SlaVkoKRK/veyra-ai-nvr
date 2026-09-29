# VEYRA 1.2.127

## Emitter Map debug always visible

- Debug -> Light Threat / Emitter Map no longer depends on NIGHT runtime context to render a map.
- The authoritative runtime Light Threat map is still preferred whenever available.
- If the runtime map does not exist (for example DAY, quiet cached MotionFusion heartbeat, or before the first production pass), DEBUG renders a diagnostic-only map from the current frame at the normal reduced Light Threat analysis width (<=480 px).
- The diagnostic fallback is isolated from the alarm path: it does not update Light Threat history, Dynamic Static Light Map learning, events, MQTT or notifications.
- The old raw-camera placeholder `OCZEKIWANIE NA PIERWSZA ANALIZE` is removed from the Emitter Map branch. Even with no emitter candidate the view is a dimmed grayscale Light Threat map; core is red and halo is orange.
- Production Light Threat alarms remain NIGHT-only.

## Regression safety

- No second Coral inference.
- No full-resolution 4K float workspace.
- Protected Live/update files remain unchanged.
