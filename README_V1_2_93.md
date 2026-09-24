# VEYRA 1.2.93 â€” GLARE reconstruction + Tracking/Stationary FP hardening

VEYRA 1.2.93 uszczelnia dwa zgĹ‚oszone przypadki z 1.2.92 bez dokĹ‚adania nowego passa obrazu ani inferencji.

## GLARE â€” peĹ‚niejsze learned background

Dynamic Glare Background nie koĹ„czy siÄ™ juĹĽ na jednym pierĹ›cieniu sÄ…siednich komĂłrek. Od 1.2.93 aktywna learned mask jest rekonstruowana na istniejÄ…cej siatce 128Ă—72:

- startuje wyĹ‚Ä…cznie z wysokiego learned core;
- rozszerza siÄ™ wyĹ‚Ä…cznie przez komĂłrki, ktĂłre juĹĽ majÄ… utrwalony glare score;
- moĹĽe przejĹ›Ä‡ przez caĹ‚e poĹ‚Ä…czone halo statycznego ĹşrĂłdĹ‚a, dziÄ™ki czemu biaĹ‚y ring wokĂłĹ‚ lampy/IR bloom powinien zostaÄ‡ domkniÄ™ty;
- odlegĹ‚a jasna wyspa nie moĹĽe zostaÄ‡ poĹ‚Ä…czona z learned mask bez ciÄ…gĹ‚ego score-path;
- nie dochodzi nowy resize, connected-components, optical flow ani detector pass.

DomyĹ›lny fringe threshold wynosi 0.14, a rekonstrukcja ma maksymalnie 12 maĹ‚ych krokĂłw na gridzie. To jest praca wyĹ‚Ä…cznie na mapie 128Ă—72.

## GLARE â€” maĹ‚y punkt bez person/car znowu moĹĽe byÄ‡ prawdziwym zagroĹĽeniem

Podstawowe zaĹ‚oĹĽenie GLARE pozostaje takie, ĹĽe mocne Ĺ›wiatĹ‚o moĹĽe zasĹ‚oniÄ‡ `person/car`, wiÄ™c brak carrier nie jest powodem odrzucenia.

1.2.93 usuwa hard-veto z 1.2.92 dla maĹ‚ego `area`. MaĹ‚e ĹşrĂłdĹ‚o przechodzi teraz do stanu `tiny_light_candidate` i jest obserwowane na istniejÄ…cej historii scalar metrics. Alarm moĹĽe powstaÄ‡ bez `person/car`, jeĹ›li ĹşrĂłdĹ‚o:

- roĹ›nie przez kilka kolejnych peĹ‚nych analiz;
- nie kurczy siÄ™ chaotycznie miÄ™dzy prĂłbkami;
- ma wyraĹşny caĹ‚kowity wzrost footprintu;
- ma spĂłjnÄ… trajektoriÄ™;
- nadal pokrywa siÄ™ z MotionFusion.

Owady/orby nadal dostajÄ… `erratic_light_veto`, jeĹ›li trajektoria jest szybka/chaotyczna lub footprint skacze gĂłra-dĂłĹ‚.

## Tracking / Stationary â€” statyczny FP nie przechodzi przez sam wysoki score

W 1.2.92 istniejÄ…cy `static_fp_guard` miaĹ‚ jeszcze score-only bypass: kandydat z motion ROI mĂłgĹ‚ po kilku identycznych trafieniach przejĹ›Ä‡ jako TP, jeĹ›li YOLO byĹ‚ okoĹ‚o 10 pp ponad confirm threshold. ZgĹ‚oszony `person 83%` na roĹ›linie trafiaĹ‚ dokĹ‚adnie w tÄ™ lukÄ™.

Od 1.2.93 istniejÄ…cy Tracking/Stationary jest twardszy:

- kandydat z `motion` / `night_weak_motion`, ktĂłrego bbox pozostaje statyczny i nie ma lokalnego motion support, pozostaje w `stationary_hold_no_local_motion` niezaleĹĽnie od samego score;
- sparse grass/leaf motion wewnÄ…trz prawie nieruchomego bboxa daje `stationary_hold_sparse_motion` zamiast automatycznej promocji po N hitach;
- realna zmiana pozycji tracka (`position_changes`) lub spĂłjny lokalny motion support natychmiast zwalnia hold;
- startup path pozostaje osobny, wiÄ™c osoba juĹĽ stojÄ…ca w kadrze przy starcie Core nie jest blokowana przez tÄ™ reguĹ‚Ä™.

Nie dodano nowego systemu ani nowej opcji UI â€” poprawka wykorzystuje istniejÄ…ce ustawienia `Tracking / stationary`.

## CPU

Zmiany sÄ… scalar/grid-only. Nie dodano:

- nowej inferencji Coral;
- nowego modelu;
- dodatkowego resize peĹ‚nej klatki;
- optical flow;
- nowego connected-components w camera hot-path.

## Live

Chronione pliki Live pozostajÄ… byte-identyczne z 1.2.92:

- `core/app/main.py`
- `docker-compose.yml`
- `live_proxy/default.conf.template`
- `scripts/apply-update.sh`

## Aktualizacja

```bash
/opt/ainvr/scripts/apply-update.sh /path/veyra_update_v1_2_93.zip
```
