# VEYRA 1.2.106 â€” GLARE approach / scene-flash separation

## GLARE Threat Guard
- Uses Illumination Guard directly to veto broad LIGHT_ON/LIGHT_OFF / door-opening scene flashes.
- Already-confirmed glare threats and coherent translating local lights are rescued from the scene-flash veto.
- Tiny distant headlights/headlamps can now establish approach from repeated component-brightness growth when reduced-plane area is quantised and does not grow every frame.
- Existing area/bbox growth, trajectory, MotionFusion support, carrier support, Dynamic Glare learned-background veto, animal veto, repeat alerts and persistent-threat behavior remain active.
- Dynamic Glare Mask stays below Threat Guard: a fresh moving/brightening source may rescue itself from learned background, while persistent static glare remains suppressed.

## Debug
GLARE debug/status now exposes brightness growth/hits and scene-flash veto state in addition to growth, overlap and reason.

## CPU / detector
No additional Coral inference, optical flow, full-frame resize or connected-components pass is added. Brightness history uses scalars already returned by the existing single glare analysis.
