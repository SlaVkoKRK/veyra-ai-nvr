# VEYRA 1.2.113

## Debug Â· Manual GLARE + Risk Zones
- Debug kamery pokazuje teraz nowe polygony per kamera: **Manual GLARE Mask** i **Risk Zone**.
- PrzeĹ‚Ä…cznik **Maski** rysuje stare maski, rÄ™czne maski GLARE oraz Risk Zones; **Strefy** pokazujÄ… rĂłwnieĹĽ Risk Zones obok klasycznych zones.
- Etykieta Manual GLARE pokazuje nazwÄ™ i prĂłg overlap, a Risk Zone nazwÄ™, `co X s` i aktywne klasy.
- Pasek statusu Debug pokazuje liczbÄ™ masek `GLARE` i `RISK`.

## GLARE Â· dalekie reflektory / samochĂłd
- naprawiono bĹ‚Ä…d konfiguracji, przez ktĂłry Threat Guard dziedziczyĹ‚ prĂłg Recovery `night_glare_highlight_threshold=242`, a jego wĹ‚asny bardziej czuĹ‚y prĂłg dla dalekiego ruchomego Ĺ›wiatĹ‚a nie byĹ‚ faktycznie uĹĽywany,
- Threat Guard ma teraz niezaleĹĽny prĂłg jasnego punktu oraz pojedynczÄ… analizÄ™ 480 px; Recovery zachowuje swĂłj osobny, bardziej konserwatywny prĂłg,
- maĹ‚y emitter moĹĽe zostaÄ‡ potwierdzony przez **halo + spĂłjnÄ… translacjÄ™ w czasie**, nawet gdy nasycony reflektor nie roĹ›nie juĹĽ mierzalnie ani nie zwiÄ™ksza Ĺ›redniej jasnoĹ›ci,
- maĹ‚y prawdziwy emitter moĹĽe korzystaÄ‡ z wczeĹ›niejszego force-area floor; zwykĹ‚y nie-emitter nadal musi przejĹ›Ä‡ ostrzejszy gate,
- bez dodatkowego Coral inference, optical flow ani drugiego przebiegu analizy GLARE.

Zabezpieczenia FP pozostajÄ… aktywne: Illumination Guard dla otwierania drzwi / LIGHT_ON, animal veto, object-reflection veto, Dynamic Glare background oraz rÄ™czne GLARE masks z Threat Guard override.

Debug GLARE pokazuje teraz dodatkowo `move`, `halo` i `emitter`, ĹĽeby byĹ‚o widaÄ‡, na ktĂłrym gate zatrzymaĹ‚ siÄ™ konkretny reflektor.
