# VEYRA 1.2.88 â€” Approach Signature, aktualizacje na dashboardzie i Integracje

VEYRA 1.2.88 rozwija alarm GLARE bez dokĹ‚adania nowego obrazu do hot-path oraz dodaje niezaleĹĽne kanaĹ‚y powiadomieĹ„ i informacjÄ™ o dostÄ™pnej aktualizacji na stronie gĹ‚Ăłwnej.

## Glare Threat Guard â€” Approach Signature

Alarm GLARE nie utoĹĽsamia juĹĽ pojedynczego skoku jasnoĹ›ci z kimĹ› zbliĹĽajÄ…cym siÄ™ z latarkÄ…/reflektorem.

- `RECENT` porĂłwnuje bieĹĽÄ…cÄ… prĂłbkÄ™ jasnego ĹşrĂłdĹ‚a z poprzedniÄ… prĂłbkÄ….
- `TREND` zachowuje informacjÄ™ o caĹ‚ym epizodzie.
- lampka przechodzÄ…ca `OFF -> ON -> ON` przestaje zbieraÄ‡ potwierdzenia po pierwszym skoku;
- duĹĽa, prawie nieruchoma pĹ‚aszczyzna Ĺ›wiatĹ‚a (np. otwierane drzwi / Ĺ›wiatĹ‚o z pomieszczenia) dostaje `scene_illumination_change` zamiast alarmu;
- maĹ‚y, szybki lub chaotyczny punkt bez wykrytego noĹ›nika Ĺ›wiatĹ‚a moĹĽe dostaÄ‡ `erratic_light_veto` (ochrona przed owadami);
- bez `person/car/truck/bus/motorcycle/bicycle/train` potrzebna jest krĂłtka historia, kolejne realne zmiany oraz spĂłjna trajektoria;
- wykryty czĹ‚owiek/pojazd zachowuje szybszÄ… Ĺ›cieĹĽkÄ™ powiadomienia;
- Glare Recovery pozostaje niezaleĹĽne od decyzji o alarmie, ale nie jest juĹĽ niepotrzebnie podtrzymywane przez pojedynczy skok lampy/drzwi.

Do metadanych GLARE trafiajÄ… `recent_growth`, `recent_shift`, `trajectory_coherence`, `episode_age` i `recent_hits`, co uĹ‚atwia pĂłĹşniejsze strojenie FP.

### CPU

Approach Signature korzysta wyĹ‚Ä…cznie z istniejÄ…cych wynikĂłw jednego passa GLARE i historii kilku wartoĹ›ci liczbowych. Nie dodaje drugiego `resize`, `connectedComponents`, optical flow, detektora ani inferencji Coral. Odrzucanie lamp/drzwi wczeĹ›niej ogranicza teĹĽ niepotrzebne wejĹ›cia w `NIGHT_GLARE`.

## Aktualizacja dostÄ™pna na dashboardzie

Strona gĹ‚Ăłwna korzysta z istniejÄ…cego `/api/system/update/check` i pokazuje popup **Aktualizacja dostÄ™pna**, gdy kanaĹ‚ GitHub udostÄ™pnia nowszÄ… wersjÄ™.

- `PĂłĹşniej` wycisza popup dla tej wersji w bieĹĽÄ…cej sesji;
- `PrzejdĹş do aktualizacji` otwiera `Ustawienia -> Aktualizacje`;
- wynik sprawdzenia jest cache'owany w `sessionStorage` przez 15 minut, wiÄ™c F5 nie odpytuje GitHuba ponownie za kaĹĽdym razem;
- nie dodano nowego endpointu aktualizatora.

## Ustawienia -> Integracje

Nowa zakĹ‚adka **Integracje** dodaje niezaleĹĽne kanaĹ‚y wysyĹ‚ania zdarzeĹ„ VEYRA:

- **Telegram** â€” bot + Chat ID, tekst lub snapshot;
- **ntfy** â€” publiczny albo wĹ‚asny serwer, opcjonalny token i attachment;
- **Pushover** â€” push z opcjonalnym obrazem;
- **Discord webhook** â€” wiadomoĹ›Ä‡ lub snapshot.

Dla kaĹĽdego providera moĹĽna osobno wĹ‚Ä…czyÄ‡: zdjÄ™cia, prealert, confirmed, repeat i GLARE. Home Assistant / HACS pozostaje natywnÄ…, niezaleĹĽnÄ… Ĺ›cieĹĽkÄ… MQTT.

Internetowe I/O oraz odczyt snapshotu dziaĹ‚ajÄ… w jednej ograniczonej kolejce i osobnym **lazy daemon-thread**. Gdy ĹĽaden provider nie jest aktywny, dodatkowy worker nawet siÄ™ nie uruchamia. CameraWorker tylko wrzuca kwalifikujÄ…cy siÄ™ event do kolejki po przejĹ›ciu istniejÄ…cych bramek powiadomieĹ„ VEYRA.

## Live

Chronione pliki Live pozostajÄ… byte-identyczne z 1.2.87:

- `core/app/main.py`
- `docker-compose.yml`
- `live_proxy/default.conf.template`
- `scripts/apply-update.sh`

## Aktualizacja

```bash
/opt/ainvr/scripts/apply-update.sh /path/veyra_update_v1_2_88.zip
```
