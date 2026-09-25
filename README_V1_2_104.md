# VEYRA 1.2.104 â€” server-side Vision feature gating

VEYRA 1.2.104 fixes the remaining Vision OFF navigation regression.

## Fixed
- Ustawienia: when Vision is disabled, the Vision tab and its settings pane are not rendered into the HTML at all.
- Logi: when Vision is disabled, the `VISION AI` source tab is not rendered into the HTML at all.
- Statystyki: the Vision statistics tab is also omitted when Vision is disabled.
- Gesture logging remains independent and stays visible when Gesture is enabled.
- Browser-side hiding remains only as a secondary safeguard; the backend feature state is now authoritative for these navigation elements.

## Live safety
The protected Live files remain byte-identical to 1.2.103.
