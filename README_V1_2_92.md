# VEYRA 1.2.92 â€” poprawne zdjÄ™cia integracji + GLARE orb/background hardening

VEYRA 1.2.92 poprawia dwa zgĹ‚oszone problemy z 1.2.91: zewnÄ™trzne integracje mogĹ‚y wysyĹ‚aÄ‡ nie ten obraz co Galeria, a maĹ‚y nocny punkt IR/owad mĂłgĹ‚ zostaÄ‡ uznany za zbliĹĽajÄ…ce siÄ™ ĹşrĂłdĹ‚o Ĺ›wiatĹ‚a przez bardzo duĹĽy wzglÄ™dny wzrost liczony od kilku pikseli.

## Integracje â€” dokĹ‚adnie ten obraz co Galeria

- `confirmed` i `repeat` preferujÄ… teraz `review.jpg` z aktywnego zdarzenia â€” ten sam oznaczony frame, ktĂłry otwiera Galeria;
- GLARE rĂłwnieĹĽ przekazuje providerom `review.jpg` z bboxem zamiast czystego obrazu;
- `WyĹ›lij test` pobiera najnowsze realne zdarzenie z Galerii i uĹĽywa jego `review.jpg`;
- gdy Galeria jest pusta, test jest wysyĹ‚any bez zdjÄ™cia â€” VEYRA nie podstawia juĹĽ sztucznego JPEG-a;
- log `INTEGRATION SENT` pokazuje teraz `image=true/false` oraz bezpiecznÄ… nazwÄ™ pliku (`review.jpg`, `thumbnail.jpg` itp.); tokeny i peĹ‚ne Ĺ›cieĹĽki nie sÄ… logowane.

## GLARE â€” maĹ‚e IR orb / owady

ZgĹ‚oszony przypadek miaĹ‚ okoĹ‚o:

```text
Glare score 0.52
Growth 4.8x
Area 0.001567
BBox 45x40 px
learned overlap 0
novel 1
```

Wzrost 4.8x nie jest wiarygodnym sygnaĹ‚em zbliĹĽania, jeĹ›li pierwszy komponent miaĹ‚ tylko kilka pikseli. Od 1.2.92 ĹşrĂłdĹ‚o bez wykrytego `person/car/truck/bus/motorcycle/bicycle/train` nie moĹĽe wywoĹ‚aÄ‡ alarmu GLARE, dopĂłki jego absolutny footprint nie przekroczy `glare_motion_unclassified_min_area_fraction` (domyĹ›lnie `0.0022`).

JeĹ›li Coral widzi wiarygodnego nosiciela Ĺ›wiatĹ‚a, szybka Ĺ›cieĹĽka pozostaje aktywna i ten bezwzglÄ™dny limit nie blokuje alarmu.

Nowa decyzja jest scalar-only: wykorzystuje istniejÄ…cy `component_area_fraction`; nie dodaje resize, connected-components, optical flow ani inferencji.

## Dynamic Glare Background â€” domkniÄ™cie biaĹ‚ego obrzeĹĽa

DuĹĽe staĹ‚e biaĹ‚e ĹşrĂłdĹ‚o potrafiĹ‚o mieÄ‡ nauczony fioletowy rdzeĹ„, ale pozostawiaÄ‡ biaĹ‚y ring na granicy, poniewaĹĽ halo/recovery w tych komĂłrkach oscylowaĹ‚o tuĹĽ poniĹĽej gĹ‚Ăłwnego progu learned.

1.2.92 dodaje hysteresis/fringe sealing wyĹ‚Ä…cznie na istniejÄ…cej siatce 128Ă—72:

- gĹ‚Ăłwny learned threshold pozostaje bez zmian;
- komĂłrka o niĹĽszym, ale juĹĽ utrwalonym score moĹĽe wejĹ›Ä‡ do aktywnej maski tylko wtedy, gdy leĹĽy maksymalnie 2 komĂłrki od juĹĽ nauczonego komponentu;
- odlegĹ‚y nowy reflektor nie moĹĽe dziÄ™ki temu stworzyÄ‡ nowej wyspy learned;
- brak nowego passa po obrazie.

Lokalny mikrobenchmark samego `_refresh_dynamic_glare_mask` na tej maszynie testowej: okoĹ‚o 14 Âµs/call w 1.2.91 i okoĹ‚o 33 Âµs/call w 1.2.92 przy syntetycznie wypeĹ‚nionej siatce 128Ă—72. To dodatkowe ~19 Âµs tylko na maĹ‚ej mapie i nie jest pomiarem i5-6500T; nie dochodzi ĹĽaden nowy koszt obrazu 1080p.

## Live

Chronione pliki Live pozostajÄ… byte-identyczne z 1.2.91:

- `core/app/main.py`
- `docker-compose.yml`
- `live_proxy/default.conf.template`
- `scripts/apply-update.sh`

## Aktualizacja

```bash
/opt/ainvr/scripts/apply-update.sh /path/veyra_update_v1_2_92.zip
```
