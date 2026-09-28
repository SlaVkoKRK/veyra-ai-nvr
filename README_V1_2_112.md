# VEYRA 1.2.112

## Mobile Gallery
- naprawiono rozwijane sekcje **SzczegĂłĹ‚y Vision** oraz **Coral / maski / scena** na telefonach; zawartoĹ›Ä‡ nie jest juĹĽ Ĺ›ciskana przez flex layout do zerowej wysokoĹ›ci,
- panel informacji eventu przewija siÄ™ jako jedna kolumna, a otwarty accordion zachowuje peĹ‚nÄ… wysokoĹ›Ä‡ treĹ›ci,
- przyciski Widok zdarzenia / Czysty kadr / Coral 512 / Glare Recovery / Vision Debug pozostajÄ… dostÄ™pne na mobile.

## Live Â· odtwarzanie w tle jak Frigate
- `TĹ‚o ON`: aktywny MSE nie jest rozĹ‚Ä…czany przy `visibilitychange`; VEYRA nie przeĹ‚Ä…cza go na statyczny fallback tylko dlatego, ĹĽe ekran/karta zostaĹ‚a ukryta,
- wewnÄ™trzny `<video>` ma ochronÄ™ `pause -> play()` dla Live, analogicznie do zachowania MSE playera Frigate,
- `TĹ‚o OFF`: ukrycie strony zatrzymuje MSE, a powrĂłt tworzy Ĺ›wieĹĽe poĹ‚Ä…czenie,
- po powrocie VEYRA reconnectuje tylko jeĹ›li transport faktycznie jest martwy; dziaĹ‚ajÄ…cego MSE nie restartuje.

Transport pozostaje `browser -> nginx -> go2rtc -> kamera`; bez Python relay, bez WebRTC auto-fallbacku i bez zmian w chronionym playerze/proxy.
