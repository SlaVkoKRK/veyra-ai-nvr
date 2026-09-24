# VEYRA 1.2.89 â€” testy integracji, edytowalne powiadomienia i logi powiadomieĹ„

VEYRA 1.2.89 rozwija Integracje dodane w 1.2.88. KaĹĽdy zewnÄ™trzny kanaĹ‚ moĹĽna teraz sprawdziÄ‡ i przetestowaÄ‡ bez czekania na prawdziwe zdarzenie, a wyglÄ…d wiadomoĹ›ci jest wspĂłlnie konfigurowalny z podglÄ…dem na ĹĽywo. Aktualizacja poprawia teĹĽ wyrĂłwnanie dolnego paska interfejsu oraz dodaje osobny widok powiadomieĹ„ w Logach.

## Integracje â€” diagnostyka i test wysyĹ‚ki

Dla **Telegram**, **ntfy**, **Pushover** i **Discord webhook** sÄ… dwa osobne przyciski:

- **SprawdĹş poĹ‚Ä…czenie** â€” weryfikuje dane providera bez wysyĹ‚ania alarmu;
- **WyĹ›lij test** â€” wysyĹ‚a rzeczywiste testowe powiadomienie z aktualnie wpisanÄ… konfiguracjÄ… i, jeĹ›li wĹ‚Ä…czono zdjÄ™cia, z przygotowanym obrazem testowym VEYRA.

Sprawdzenie poĹ‚Ä…czenia korzysta z natywnych mechanizmĂłw providera:

- Telegram: `getMe` + opcjonalnie `getChat`; przy pustym Chat ID VEYRA prĂłbuje znaleĹşÄ‡ ostatni chat z `getUpdates`;
- ntfy: endpoint zdrowia `/v1/health`;
- Pushover: walidacja uĹĽytkownika i tokena aplikacji;
- Discord: odczyt metadanych webhooka.

KaĹĽda karta zawiera opis pĂłl oraz instrukcjÄ™ skÄ…d wziÄ…Ä‡ token / Chat ID / Topic / User key / Webhook URL wraz z linkiem do oficjalnej dokumentacji.

## Co oznaczajÄ… opcje wysyĹ‚ki

Panel Integracje opisuje wspĂłlnie:

- **ZdjÄ™cia** â€” doĹ‚Ä…czenie snapshotu do powiadomienia;
- **Prealert** â€” najszybszy alert z wczesnej, mocnej detekcji przed peĹ‚nÄ… promocjÄ… eventu;
- **Potwierdzone** â€” zdarzenie po peĹ‚nej bramce TP VEYRY;
- **PowtĂłrzenia** â€” kolejne alarmy aktywnego obiektu;
- **GLARE** â€” niezaleĹĽny alarm Glare Threat Guard.

Testy providerĂłw oraz normalna wysyĹ‚ka internetowa nie wykonujÄ… I/O w CameraWorkerze. ZwykĹ‚a wysyĹ‚ka nadal korzysta z ograniczonej, lazy kolejki i osobnego wÄ…tku.

## Home Assistant / HACS

Karta Home Assistant / HACS ma bezpoĹ›rednie linki do:

- repozytorium `SlaVkoKRK/veyra-home-assistant`;
- instrukcji dodawania wĹ‚asnego repozytorium w HACS.

Integracja HACS pozostaje niezaleĹĽnÄ… natywnÄ… Ĺ›cieĹĽkÄ… MQTT i nie korzysta z szablonĂłw providerĂłw internetowych.

## Szablony powiadomieĹ„

WspĂłlny edytor wyglÄ…du pozwala zmieniÄ‡:

- tytuĹ‚ zwykĹ‚ego zdarzenia;
- opis zwykĹ‚ego zdarzenia;
- tytuĹ‚ GLARE;
- opis GLARE.

DostÄ™pne zmienne obejmujÄ… m.in.:

`{ikona}`, `{nazwa_kamery}`, `{kamera}`, `{klasa}`, `{klasa_id}`, `{confidence}`, `{score}`, `{typ}`, `{repeat}`, `{event_id}`, `{czas}`, `{growth}`, `{motion_overlap}`, `{reason}`.

VEYRA dostarcza gotowe domyĹ›lne szablony. Panel ma podglÄ…d tytuĹ‚u, treĹ›ci i zdjÄ™cia; jeĹĽeli Galeria zawiera zdarzenie, podglÄ…d uĹĽywa ostatniego eventu, a w przeciwnym razie danych testowych.

## Logi -> POWIADOMIENIA

W Logach jest nowy filtr **POWIADOMIENIA** oparty na istniejÄ…cym `core.log`. Pokazuje wpisy:

- `NOTIFY` â€” przekazanie eventu do warstwy zewnÄ™trznych powiadomieĹ„;
- `INTEGRATION CHECK` â€” rÄ™czna diagnostyka poĹ‚Ä…czenia;
- `INTEGRATION TEST` â€” rÄ™czna wysyĹ‚ka testowa;
- `INTEGRATION SENT` â€” udana normalna wysyĹ‚ka;
- `INTEGRATION FAILED` â€” bĹ‚Ä…d wysyĹ‚ki.

Nie powstaje nowy plik logu ani dodatkowy polling backendu.

## UI

Dolny pasek statystyk/systemu na desktopie ma tÄ™ samÄ… wysokoĹ›Ä‡ co stopka bocznego menu. UsuniÄ™to widoczne przesuniÄ™cie o okoĹ‚o 1â€“2 px.

## CPU / Live

1.2.89 nie zmienia detekcji, GLARE ani hot-path obrazu z 1.2.88. Testy providerĂłw wykonujÄ… siÄ™ wyĹ‚Ä…cznie po klikniÄ™ciu uĹĽytkownika, a logowanie powiadomieĹ„ jest event-level.

Chronione pliki Live pozostajÄ… byte-identyczne z 1.2.88:

- `core/app/main.py`
- `docker-compose.yml`
- `live_proxy/default.conf.template`
- `scripts/apply-update.sh`

## Aktualizacja

```bash
/opt/ainvr/scripts/apply-update.sh /path/veyra_update_v1_2_89.zip
```
