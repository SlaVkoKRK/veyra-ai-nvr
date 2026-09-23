# VEYRA 1.2.76 â€” Glare Motion Guard + UI cleanup

## Ustawienia / Vision WORK

- pasek `Veyra / Vision / Aktualizacje` uĹĽywa dokĹ‚adnie tego samego komponentu `veyra-strip` i tej samej obudowy `unified-strip-shell` co Galeria, Statystyki i Logi;
- na mobile pasek pozostaje poziomo przewijalny i korzysta ze wspĂłlnych strzaĹ‚ek;
- w jasnym motywie caĹ‚y popup **Vision WORK** ma teraz jasne tĹ‚o, nie tylko wewnÄ™trzna miniatura.

## Glare Motion Guard

VEYRA potrafi teraz wykryÄ‡ szczegĂłlny scenariusz bezpieczeĹ„stwa: **ruchome ĹşrĂłdĹ‚o silnego Ĺ›wiatĹ‚a, ktĂłre zbliĹĽa siÄ™ i coraz mocniej oĹ›lepia kamerÄ™**.

Warunki alarmu sÄ… celowo wielostopniowe:

1. aktywny transient `GLARE` z istniejÄ…cego DetectorSceneProfile;
2. prawdziwa maska glare (saturated core + soft halo) nakĹ‚ada siÄ™ na `MotionFusion`;
3. obszar glare roĹ›nie w krĂłtkim oknie czasowym;
4. wymagane sÄ… co najmniej dwa potwierdzenia;
5. szerokie skoki ekspozycji / AE pump sÄ… odrzucane przez limit `spread_ratio`;
6. alarm ma cooldown per kamera i jest wysyĹ‚any tylko raz w jednym epizodzie glare.

Mechanizm jest **detector-free**: nie uruchamia dodatkowego Corala, nie tworzy drugiego ROI i nie dekoduje drugiego strumienia. Pracuje na istniejÄ…cej pĹ‚aszczyĹşnie Y oraz istniejÄ…cych boxach MotionFusion.

W Debug â†’ `GLARE MASK` pokazuje siÄ™ obszar `GLARE MOTION`, wzrost (`x`) i pokrycie ruchem (`%`).

Status kamery udostÄ™pnia dodatkowo:

- `glare_approach_active`
- `glare_approach_score`
- `glare_approach_growth`
- `glare_approach_motion_overlap`
- `glare_approach_area_fraction`
- `glare_approach_last_ts`
- `glare_approach_count`

## Home Assistant / HACS

Repozytorium `SlaVkoKRK/veyra-home-assistant` zostaĹ‚o zaktualizowane do **0.3.2**:

- obsĹ‚uga natywnego `glare_approach` z MQTT notifications topic;
- pilne powiadomienie â€žKtoĹ› zbliĹĽa siÄ™ i oĹ›lepia kamerÄ™â€ť;
- osobny `binary_sensor` per kamera: **ZbliĹĽajÄ…ce oĹ›lepienie**;
- osobny poziom powiadomieĹ„ `Powiadomienia Â· oĹ›lepianie kamery`;
- atrybuty sensora: score, growth, motion overlap, area fraction, last alert i licznik alarmĂłw.

## Live safety

Transport Live nie zostaĹ‚ zmieniony:

```text
browser -> nginx -> go2rtc -> camera
```

Brak Python websocket relay, dodatkowego SourceBuffer, playbackRate regulatora i dodatkowego przebiegu Corala.
