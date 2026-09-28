# VEYRA 1.2.115

## Updater runtime hotfix

- Naprawiono `homeCurrentVersion is not defined` w Ustawienia â†’ Aktualizacje.
- Updater w Ustawieniach jest samodzielny i nie zaleĹĽy od zmiennych strony gĹ‚Ăłwnej.
- Stan UPDATER pokazuje teraz: Sprawdzanie, Brak aktualizacji, DostÄ™pna aktualizacja, Instalowanie aktualizacji lub BĹ‚Ä…d.
- Podczas instalacji wyĹ›wietlany jest spinner, pasek procentowy i krĂłtki log kolejnych etapĂłw.
- `control-worker.sh` i `auto-update.sh` publikujÄ… bezpieczne etapy postÄ™pu do istniejÄ…cego `status.json`.
- GitHub API pozostaje ĹşrĂłdĹ‚em pierwszego wyboru, a raw jest niewidocznym fallbackiem.
