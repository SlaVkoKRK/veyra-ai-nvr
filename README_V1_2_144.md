# VEYRA 1.2.144

> **Camera Masks / Zones UI correction over 1.2.143.**

## 1. Camera selector + settings tabs stay full width
The camera selector and the `Obiekty / Maski / Strefy / Ruch / ...` navigation now live in their own full-width header above the editor area.

For normal tabs (`Obiekty`, `Ruch`, etc.) the content remains full width. For polygon editors (`Maski`, `Strefy`) the area below the header switches to a two-column desktop layout again: controls in one column and the live camera/canvas preview in its own adjacent column. Mobile still collapses to one column.

## 2. Masks preview is filtered by the current selection
The Masks canvas no longer draws every polygon family at the same time. It now shows only the currently selected scope:

- `Maska ruchu` â†’ only motion masks,
- `Maska obiektĂłw` + `WSZYSTKIE obiekty` â†’ only ALL object masks,
- `Maska obiektĂłw` + a class such as `car` â†’ only masks for that class,
- `Static Light Mask` â†’ only Static Light masks.

The active polygon still has the brighter outline and draggable points. Filtering is visual/editor-only; it does not disable or change runtime mask behaviour.

## 3. High-risk zones moved out of Masks
Camera settings now have a separate **Strefy** tab. `Strefa wysokiego ryzyka` was removed from the visible Masks type selector and moved to this tab together with its existing metadata:

- zone name,
- object classes,
- repeat cadence including 0.5 / 1 / 1.5 s,
- polygon editing.

The Zones preview shows only high-risk zones. Existing `detect.risk_zones` configuration and runtime logic are unchanged, so existing zones do not need to be redrawn.

Old links using `?tab=masks&kind=risk` are interpreted as the new `Strefy` editor for compatibility.

## 4. Previous fixes retained
1.2.144 keeps the 1.2.143 per-mask bbox overlap threshold fix and the OpenCV DEBUG ASCII normalisation, plus the 1.2.142 candidate runtime version / two-phase updater identity fix.

## Architecture
- No second decode.
- No second Coral inference.
- No full-frame additional resize.
- No optical flow or heavier model.
- Runtime risk-zone detection format is unchanged.
- `core/app/main.py`, `docker-compose.yml` and `live_proxy/default.conf.template` are unchanged from 1.2.143.

## Targeted validation
- full-width camera/settings header,
- desktop two-column Mask / Zone editor,
- filtered Motion / ALL / class / Static Light preview,
- separate High Risk Zones tab,
- existing risk-zone save format and cadence,
- per-mask overlap threshold regression,
- candidate runtime version regression,
- Python compile, JavaScript syntax, shell syntax and YAML parse.
