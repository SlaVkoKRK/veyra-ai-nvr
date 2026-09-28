# VEYRA 1.2.117

- Fixed Light Threat / Emitter Map debug routing: the selected stage now renders the real adaptive core + halo diagnostic instead of falling back to RAW.
- Emitter Map displays whether Light Threat runtime is NIGHT ACTIVE or DAY / INACTIVE.
- Risk Zones are no longer drawn by the Debug "Maski" switch; they are visible only under "Strefy".
- Risk Zone OpenCV labels use ASCII separators, avoiding `??` from unsupported Unicode glyphs, and show the repeat interval directly.
- Live pan/zoom geometry now allows reaching the full frame at zoom, supports up to 600%, and adds visible minus / percent-reset / plus controls while preserving wheel, drag and pinch gestures.
- Live transport remains unchanged.
