# VEYRA 1.2.87 â€” trwaĹ‚e â€žZdarzenia od ostatniej wizytyâ€ť

VEYRA 1.2.87 jest maĹ‚ym wydaniem UI nad 1.2.86. Nie zmienia pipeline kamer, GLARE, detekcji, Vision ani Live.

## Zdarzenia od ostatniej wizyty

Dashboard nie traktuje juĹĽ odĹ›wieĹĽenia strony jako nowej wizyty.

- `localStorage` przechowuje trwaĹ‚y znacznik **przejrzane do** (`veyraHomeReviewedMs`).
- `sessionStorage` przechowuje cutoff bieĹĽÄ…cej karty/sesji (`veyraHomeVisitCutoffMs`).
- F5 / reload odtwarza listÄ™ z `/api/events`, ale zachowuje ten sam cutoff.
- Nowy przycisk **Oznacz jako przejrzane** jawnie przesuwa cutoff na bieĹĽÄ…cy czas i czyĹ›ci listÄ™.
- Licznik jest widoczny bezpoĹ›rednio w nagĹ‚Ăłwku sekcji.
- Stary klucz z 1.2.86 (`veyraHomeLastVisitMs`) jest jednorazowo migrowany, wiÄ™c aktualizacja nie gubi dotychczasowego punktu odniesienia.

To nie jest cache obrazĂłw ani nowy backend. Miniatury i status Vision sÄ… po reloadzie pobierane ponownie z bieĹĽÄ…cego stanu, dziÄ™ki czemu nie zostajÄ… stare dane.

## CPU / Live

Zmiana dziaĹ‚a wyĹ‚Ä…cznie w JavaScript dashboardu i nie dodaje ĹĽadnego pollingu ani pracy do camera hot-path. InterwaĹ‚ `/api/events` pozostaje bez zmian.

Chronione pliki Live pozostajÄ… byte-identyczne z 1.2.86:

- `core/app/main.py`
- `docker-compose.yml`
- `live_proxy/default.conf.template`
- `scripts/apply-update.sh`

## Aktualizacja

```bash
/opt/ainvr/scripts/apply-update.sh /path/veyra_update_v1_2_87.zip
```
