# VEYRA 1.2.123

Recording feature gate, safe storage / Space Guard and authoritative Light Threat debug.

- Naprawiono bĹ‚Ä…d `esc is not defined` w ustawieniach nagrywania kamery.
- Dodano globalny przeĹ‚Ä…cznik `recording.enabled`. Gdy jest wyĹ‚Ä…czony, recordery nie zapisujÄ…, **Review** nie jest pokazywany w menu, a zakĹ‚adka nagrywania znika z ustawieĹ„ kamer.
- DomyĹ›lny magazyn zmieniono na `/data/recordings`. VEYRA moĹĽe automatycznie utworzyÄ‡ katalog pod `/data`; zewnÄ™trzny dysk/NFS musi byÄ‡ podmontowany po stronie hosta/LXC tak, aby byĹ‚ widoczny w kontenerze Core. Panel nadal nie wykonuje systemowego mountowania NFS.
- Dodano **Space Guard**: domyĹ›lnie zachowuje co najmniej wiÄ™kszÄ… z wartoĹ›ci 2 GB lub 5% wolnego miejsca, usuwa najpierw najstarsze continuous, potem ring, a event clips dopiero na koĹ„cu. PoniĹĽej 512 MB recorder blokuje/zatrzymuje FFmpeg zamiast zapchaÄ‡ filesystem.
- Dodano logi start/stop, problemĂłw magazynu, restartĂłw FFmpeg, utworzenia event clipĂłw, retention cleanup i Space Guard.
- Importer Frigate mapuje teraz `record/events` oraz dostÄ™pne ustawienia alert/detection retention i pre/post-capture do recordera VEYRA.
- Dla Light Threat gĹ‚Ăłwnym obrazem diagnostycznym w Galerii jest teraz **Emitter Map z dokĹ‚adnie tej klatki, ktĂłra przeszĹ‚a Decision Gate**. GLARE Recovery pozostaje osobnym obrazem diagnostycznym i nie jest ĹşrĂłdĹ‚em alarmu.
- Naprawiono `??` w dolnym pasku Debug â†’ Maski adaptacyjne: OpenCV nie renderowaĹ‚ separatora Unicode `Â·`; tekst overlay uĹĽywa teraz znakĂłw ASCII.
- Chroniona architektura Live i jej pliki pozostajÄ… bez zmian.

## Canonical YAML recording schema

```yaml
recording:
  enabled: false
  path: /data/recordings
  segment_seconds: 60
  ring_segment_seconds: 10
  ring_retention_seconds: 1800
  pressure_cleanup_enabled: true
  min_free_gb: 2
  min_free_percent: 5
  hard_stop_free_mb: 512

cameras:
  front:
    recording:
      continuous_enabled: false
      continuous_retention_days: 7
      events_enabled: false
      event_retention_days: 30
      prebuffer_seconds: 5
      postbuffer_seconds: 10
```

Recorder korzysta z lokalnego RTSP go2rtc i FFmpeg `-c copy`; konfiguracja nie dodaje drugiego decode ani transkodowania obrazu.


## 1.2.123 â€” Emitter Map debug isolation

- Debug **Light Threat / Emitter Map** reuses the reduced runtime analysis produced by the single Light Threat pass; it no longer reconstructs the map from the 4K BGR frame.
- This removes the large full-resolution float32/YCrCb allocation burst that could terminate the capture worker and leave Camera/Debug in ERROR while native Live kept working.
- Gallery Light Threat evidence uses the same authoritative runtime emitter planes. A bounded <=480 px compatibility fallback exists only for stale/legacy analysis payloads.
- Other expensive diagnostic stages are reduced to the configured debug output size before preprocessing.
- No Live proxy, go2rtc path, recorder stream-copy path, or Coral inference path is changed.
