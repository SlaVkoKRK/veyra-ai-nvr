# VEYRA 1.2.121

NVR / Review + Light Threat Decision Gate.

- Dodano lekkie nagrywanie per kamera przez lokalny RTSP go2rtc i FFmpeg `-c copy` â€” bez drugiego decode i bez transkodowania obrazu.
- Dodano ciÄ…gĹ‚e nagrywanie segmentowe, niezaleĹĽnÄ… retencjÄ™, event recording z pre/postbufferem oraz tymczasowy ring buffer, gdy nagrywanie ciÄ…gĹ‚e jest wyĹ‚Ä…czone.
- Dodano walidacjÄ™ gotowego mount pointa/katalogu nagraĹ„ oraz status zajÄ™tego i wolnego miejsca. VEYRA nie wykonuje systemowego mountowania NFS.
- Dodano gĹ‚Ăłwny widok **Review** z playerem, timeline, lukami w nagraniu, eventami, zoomem 24 h / 6 h / 1 h / 15 min, drag, hover i obsĹ‚ugÄ… dotykowÄ…/pinch.
- Dodano bezpieczne API nagraĹ„: status, segmenty, event clips, eventy zakresu, playback i retention cleanup. Playback jest ograniczony do drzew `VEYRA/continuous`, `VEYRA/ring` i `VEYRA/events`.
- Dodano twardy **Light Threat Decision Gate** przed publikacjÄ… i zapisem zdarzenia. Stare `glare` / `glare_approach` pozostajÄ… wyĹ‚Ä…cznie jako identyfikatory kompatybilnoĹ›ci.
- Naprawiono regresjÄ™, w ktĂłrej historyczne hity mogĹ‚y ponownie uzbroiÄ‡ alarm mimo braku bieĹĽÄ…cego emitera na Emitter Map.
- `GLARE RECOVERY` pozostaje etapem diagnostycznym/recovery i nie moĹĽe sam utworzyÄ‡ ani opublikowaÄ‡ zdarzenia.
- Zachowano IR Transition Guard z 1.2.120 oraz zasadÄ™ jednej analizy obrazu dla wielu konsumentĂłw.
- Nie zmieniono chronionej architektury Live.
