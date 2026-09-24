# VEYRA 1.2.90 â€” poprawka testĂłw Integracji / Telegram 404

VEYRA 1.2.90 naprawia bĹ‚Ä…d z 1.2.89, w ktĂłrym przyciski **SprawdĹş poĹ‚Ä…czenie** i **WyĹ›lij test** w Integracjach mogĹ‚y zwracaÄ‡ `Not Found`, mimo ĹĽe panel Integracje byĹ‚ widoczny.

## Przyczyna

1.2.89 rejestrowaĹ‚a diagnostyczne endpointy Integracji z warstwy WebUI, aby nie zmieniaÄ‡ chronionego `core/app/main.py`. Rejestrator szukaĹ‚ uruchomionego Core wyĹ‚Ä…cznie pod nazwÄ… moduĹ‚u `<pakiet>.main`.

Przy uruchomieniu produkcyjnym przez `python -m ...` wykonywany moduĹ‚ jest jednak dostÄ™pny jako `__main__`. W takim wariancie rejestrator nie znajdowaĹ‚ obiektu FastAPI i po cichu koĹ„czyĹ‚ pracÄ™, przez co `/api/integrations/{provider}/check` i `/api/integrations/{provider}/test` nie istniaĹ‚y.

## Poprawka

- rejestrator obsĹ‚uguje oba warianty uruchomienia: nazwany moduĹ‚ `<pakiet>.main` oraz `__main__`;
- sukces rejestracji zapisuje `INTEGRATION ROUTES registered ...` do logu Core;
- bĹ‚Ä…d rejestracji nie jest juĹĽ niemy â€” zapisuje `INTEGRATION ROUTES FAILED ...`;
- dodano test regresji dokĹ‚adnie dla wariantu `python -m ...` / `__main__`;
- kod providerĂłw Telegram / ntfy / Pushover / Discord oraz pipeline kamer nie sÄ… zmieniane.

## CPU / Live

Brak zmian w CameraWorker, GLARE, Vision i Live. Chronione pliki Live pozostajÄ… byte-identyczne z 1.2.89:

- `core/app/main.py`
- `docker-compose.yml`
- `live_proxy/default.conf.template`
- `scripts/apply-update.sh`

## Aktualizacja

```bash
/opt/ainvr/scripts/apply-update.sh /path/veyra_update_v1_2_90.zip
```
