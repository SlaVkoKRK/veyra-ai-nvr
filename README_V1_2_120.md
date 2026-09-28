# VEYRA 1.2.120

Light Threat / IR transition hotfix.

- Dodano **IR Transition Guard** dla stabilnego wejĹ›cia kamery w `NIGHT_IR`.
- Po przeĹ‚Ä…czeniu kamery na IR nowe alerty Light Threat sÄ… wyciszone przez krĂłtki okres stabilizacji (domyĹ›lnie 8 s).
- Guard dotyczy wyĹ‚Ä…cznie Light Threat; zwykĹ‚e Motion i Coral pozostajÄ… aktywne.
- Przy wejĹ›ciu w IR czyszczona jest historia emitera, aby klatki kolorowe i IR nie tworzyĹ‚y faĹ‚szywej trajektorii/growth.
- Adaptive Static Light Map nie uczy siÄ™ z klatek przejĹ›ciowych; resetowany jest tylko jej temporalny baseline, a zapamiÄ™tana mapa statycznych Ĺ›wiateĹ‚ pozostaje zachowana.
- Guard nie jest uzbrajany przy kaĹĽdym chwilowym NIGHTâ†’DAY, poniewaĹĽ prawdziwy reflektor/czoĹ‚Ăłwka moĹĽe chwilowo podnieĹ›Ä‡ jasnoĹ›Ä‡ sceny i nadal musi byÄ‡ wykrywalny.
- Dodano diagnostykÄ™ stanu IR Guard do statusu Debug.

Nagrywanie/Review zaplanowane po tym hotfixie jako osobna zmiana, aby nie mieszaÄ‡ walidacji Light Threat z duĹĽym moduĹ‚em NVR.
