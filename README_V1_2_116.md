# VEYRA 1.2.116 â€” Light Threat

Light Threat zastÄ™puje GLARE jako gĹ‚Ăłwny mechanizm wykrywania ruchomych ĹşrĂłdeĹ‚ Ĺ›wiatĹ‚a.

NajwaĹĽniejsze zmiany:
- adaptacyjny emitter core wzglÄ™dem lokalnego tĹ‚a zamiast samego progu jasnoĹ›ci,
- halo/bloom oraz wpĹ‚yw Ĺ›wiatĹ‚a na otoczenie (`light impact`),
- `surface score` odrzucajÄ…cy jasne powierzchnie i odbicia IR bez potrzeby rozpoznawania psa/zwierzÄ™cia,
- ta sama Ĺ›cieĹĽka obsĹ‚uguje dalekie reflektory aut, czoĹ‚Ăłwki i latarki,
- spĂłjna trajektoria nadal korzysta z istniejÄ…cej historii MotionFusion,
- GLARE pozostaje tylko jako Recovery/fallback i zgodnoĹ›Ä‡ ze starszymi eventami/HACS,
- nowy widok Debug: `Light Threat Â· emitter map` (czerwony core, pomaraĹ„czowy halo),
- rÄ™czne dawne maski GLARE sÄ… prezentowane jako maski Light Threat bez migracji konfiguracji,
- brak drugiego Coral inference, brak dodatkowego streamu i brak optical flow.

KompatybilnoĹ›Ä‡: minimum 1.2.31. Techniczne pola `glare` / `glare_approach` pozostajÄ… zachowane, aby nie zrywaÄ‡ istniejÄ…cych automatyzacji i historii Gallery.
