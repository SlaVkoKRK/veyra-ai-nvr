# VEYRA 1.2.108 â€” GLARE Recovery snapshot in Gallery

## Zmiany

- Event GLARE zapisuje teraz `glare_recovery.jpg` dokĹ‚adnie z tej samej klatki, ktĂłra utworzyĹ‚a zdarzenie.
- Obraz jest generowany przez ten sam runtime stage `glare_mask`, ktĂłry zasila Debug â†’ **Maska glare Â· Recovery** (saturated core + halo NIGHT_GLARE Recovery).
- Brak dodatkowego Coral inference i brak dodatkowego RTSP/decode pass.
- Galeria pokazuje przycisk **Glare Recovery** wyĹ‚Ä…cznie dla eventĂłw GLARE posiadajÄ…cych zapisany artefakt.
- Powtarzane alerty tego samego epizodu GLARE nadal uĹĽywajÄ… jednego eventu i jednego zamroĹĽonego obrazu Recovery z chwili pierwszego eventu.
- Chroniona Ĺ›cieĹĽka Live pozostaje byte-identyczna; wykorzystany zostaĹ‚ istniejÄ…cy uwierzytelniony endpoint obrazu diagnostycznego zamiast dodawania nowej trasy do `main.py`.

## KompatybilnoĹ›Ä‡

- Bez migracji konfiguracji.
- Starsze eventy GLARE bez zapisanego obrazu Recovery po prostu nie pokazujÄ… przycisku **Glare Recovery**.
- Minimalna wspierana wersja aktualizatora: 1.2.31.
