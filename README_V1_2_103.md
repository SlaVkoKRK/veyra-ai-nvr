# VEYRA 1.2.103 â€” corrected Vision boundaries + cleaner Integrations UI

VEYRA 1.2.103 continues directly from 1.2.102 and fixes the remaining UI boundary regressions around the Vision feature flag.

## Vision OFF hides only Vision

When **Integracje â†’ OgĂłlne â†’ Vision** is OFF, VEYRA now hides only Vision-owned UI:

- the **Vision** tab and prompt editor in System Settings;
- the **VISION AI** tab in Logs;
- Vision TP / FP / ? badges and Vision filters;
- Vision Verify, Vision details, retry controls and Vision Debug.

The normal event diagnostic tools remain available regardless of Vision state:

- **Coral / maski / scena**;
- **Widok zdarzenia**;
- **Czysty kadr**;
- **Coral 512**.

These controls belong to the detector/event pipeline, not to the optional Vision verifier. The Gallery feature gate is therefore applied only to explicit `vision-only` elements instead of the whole event side panel.

## Strict settings and log tabs

The Vision tab in Settings and **VISION AI** in Logs now use both the HTML `hidden` state and explicit display gating. Direct navigation to `?tab=vision` while Vision is disabled falls back to the Veyra settings tab.

Historical Vision metadata remains stored and becomes visible again if Vision is re-enabled.

## Integrations information architecture

The main Integrations description is now provider-neutral and describes the page as a place for optional VEYRA functions and external services.

The second-level tabs provide their own context:

- **OgĂłlne** â€” Gesture, Vision and Home Assistant / HACS system integrations;
- **Powiadomienia** â€” alert delivery providers, event selection, message templates and on-demand connection tests.

## Live safety

The protected Live transport files remain unchanged from 1.2.102:

- `core/app/main.py`
- `docker-compose.yml`
- `live_proxy/default.conf.template`
- `scripts/apply-update.sh`

Native Live remains `browser -> nginx -> go2rtc -> camera`.
