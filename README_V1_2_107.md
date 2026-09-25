# VEYRA 1.2.107 â€” GLARE emitter guard + master switch parity

## GLARE false-positive guard
- Adds a local emitter signature using the already-computed bright core and local bloom/halo.
- Large moving reflective objects (for example reflective dog fur/eyes, clothing or wet vegetation) cannot become GLARE only because the bright component grows rapidly.
- Existing tracked animal veto now associates glare with the animal using both glare size and animal bbox size.
- Real headlights/headlamps are rescued by local halo/bloom, coherent brightness approach, or an actual person/vehicle carrier.
- Tiny distant headlamp/headlight behavior from 1.2.106 remains unchanged.

## Main switches
- GLARE now obeys the main AI Detection state. With detection or motion OFF it cannot arm the GLARE sensor, persist an event, or send a notification.
- Snapshot OFF prevents GLARE event/snapshot persistence in Gallery. Notifications may still be text-only when AI Detection and Notifications are ON.
- Notification master and per-camera notification switches continue to gate GLARE through the normal notification configuration.

## Debug / Gallery
- GLARE event metadata now includes local halo ratio, core fill, emitter signature and reflection-veto state.
- Event Debug exposes these values so rejected/accepted sources can be diagnosed from the saved event.

## CPU
No extra Coral inference, optical flow, full-frame resize or connected-components pass is added. The emitter signature reuses masks from the existing single GLARE analysis and only inspects a small local slice around the selected component.
