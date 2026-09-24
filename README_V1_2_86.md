# VEYRA 1.2.86

Lower-CPU GLARE background rejection plus review/dataset workflow and Frigate configuration migration on top of 1.2.85.

## GLARE: persistent background vs fresh light

- Glare Threat Guard no longer lets generic MotionFusion support punch through an already learned bright background.
- A persistent learned lamp / IR bloom / fog / steam region is treated as background unless the current bright component contains meaningful **fresh/novel** light or a nearby person/vehicle carrier provides a valid rescue signal.
- Real headlamps and vehicle headlights can still alert when they expand outside the learned background or move enough to create new saturated structure.
- Existing animal veto and rescue behavior remain; a dog/motion alone cannot promote GLARE.
- GLARE event metadata now stores `learned_overlap` and `novel_fraction` so false alarms can be audited directly from Gallery diagnostics.

## One glare image analysis -> multiple consumers

- The reduced night luminance/core/halo analysis is computed once on a real MotionFusion analysis frame.
- The same result is shared by Glare Threat Guard and Dynamic Glare Background.
- Dynamic Glare no longer performs its own second image pass in the production camera hot path.
- `analysis_skipped` frames still perform no glare image analysis.
- DAY still performs no Dynamic Glare learning/overlay work.
- No additional Coral inference, decoder or video stream is introduced.

### Local synthetic microbenchmark

1920x1080 synthetic night luminance, OpenCV single-threaded, 60 timed iterations. These numbers compare the glare path itself and are **not** a measurement of the production i5-6500T host CPU percentage.

| Path | 1.2.83 | 1.2.85 | 1.2.86 |
|---|---:|---:|---:|
| motion-supported Threat + learner, median | 7.992 ms | 5.125 ms | **1.952 ms** |
| no-motion full MotionFusion frame / learner, median | 7.965 ms | 5.233 ms | **1.972 ms** |

The 1.2.86 implementation therefore removes image work compared with both 1.2.85 and the Codex 1.2.83 glare path rather than adding another per-frame pass.

## Home: events since the previous visit

- The dashboard section is now **Zdarzenia od ostatniej wizyty**.
- The browser remembers the start of the previous Home visit and initially loads enough recent events to recover that interval.
- Subsequent polling is small and incremental; this is UI-only and adds no camera hot-path work.
- On the first visit, VEYRA simply shows the latest events.

## Gallery: human review queue

- Gallery now has a **DO WERYFIKACJI** source filter.
- It lists events already classified automatically by Vision (TP / FP / ?) that have not yet received a manual human verdict.
- Setting manual TP / FP / ? removes the event from that queue and advances review to the next loaded item.
- **RÄCZNIE ZWERYFIKOWANE** remains available for already reviewed events, and AUTO can still clear a manual override.
- GLARE remains outside Vision verification as introduced in 1.2.85.

## Frigate -> VEYRA configuration importer

System Settings -> **Veyra** now contains an on-demand Frigate YAML importer.

Workflow:

1. paste the existing Frigate YAML;
2. choose **Konwertuj Frigate -> VEYRA**;
3. inspect the generated VEYRA YAML and migration warnings;
4. choose **Wstaw do edytora** to copy the preview into the normal VEYRA config editor, or **OdrzuÄ‡**;
5. normal VEYRA validation/save remains the final explicit step.

The importer maps compatible camera/go2rtc/MQTT/detect/motion/object-filter settings. Recording, Frigate snapshots, zones and Frigate-specific detector/model sections are intentionally not migrated blindly. Existing VEYRA Coral/model/Vision/night/notification configuration is preserved, so importing Frigate does not create a second detector or inference path.

The parser/converter runs only when the user opens/uses the configuration tool in the browser; it has zero camera hot-path cost.

## Live / compatibility

- Native Live transport remains `browser -> nginx -> go2rtc -> camera`.
- `core/app/main.py`, `docker-compose.yml`, `live_proxy/default.conf.template` and `scripts/apply-update.sh` remain byte-identical to 1.2.85.
- HACS 0.3.3 remains protocol-compatible; no Home Assistant integration update is required.
