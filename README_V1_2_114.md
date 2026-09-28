# VEYRA 1.2.114

## Aktualizacje Â· spĂłjna wersja w caĹ‚ym UI

Naprawiono rozjazd pomiÄ™dzy komunikatem o aktualizacji na stronie gĹ‚Ăłwnej a zakĹ‚adkÄ… **Ustawienia â†’ Aktualizacje**.

### Co byĹ‚o nie tak
- strona gĹ‚Ăłwna potrafiĹ‚a poprawnie pobraÄ‡ Ĺ›wieĹĽÄ… wersjÄ™ z GitHub Contents API,
- okno aktualizacji wykonywaĹ‚o osobno odczyt lokalnej wersji,
- bĹ‚Ä…d lokalnego odczytu powodowaĹ‚ wejĹ›cie caĹ‚ego bloku w fallback raw i nadpisanie poprawnej wersji z GitHub API starszym wynikiem z raw/CDN,
- UI pokazywaĹ‚ techniczny dopisek `Â· fallback raw`.

### Co zmieniono
- **latest** i **current** sÄ… rozwiÄ…zywane niezaleĹĽnie â€” problem z odczytem wersji lokalnej nie moĹĽe juĹĽ cofnÄ…Ä‡ wersji zdalnej,
- zakĹ‚adka Aktualizacje uĹĽywa tego samego cache najnowszej wersji co popup na stronie gĹ‚Ăłwnej,
- kolejnoĹ›Ä‡ ĹşrĂłdeĹ‚ najnowszej wersji: Ĺ›wieĹĽy cache â†’ GitHub Contents API â†’ backend raw fallback,
- jeĹĽeli raw zwrĂłci starszÄ… wersjÄ™ niĹĽ wczeĹ›niej znana z GitHub API, nowsza znana wersja ma pierwszeĹ„stwo,
- odczyt wersji zainstalowanej ma osobne fallbacki i nie wpĹ‚ywa na wynik wersji zdalnej,
- techniczne etykiety `GitHub API` / `fallback raw` zostaĹ‚y usuniÄ™te z interfejsu; ĹşrĂłdĹ‚o pozostaje tylko w `console.debug`.

Nie zmieniono transportu Live ani chronionych plikĂłw Live.
