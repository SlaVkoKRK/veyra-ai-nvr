# VEYRA 1.2.77 â€” Glare Threat Guard v2

## Dlaczego 1.2.76 przepuszczaĹ‚o czoĹ‚Ăłwki / dalekie auta

1.2.76 uruchamiaĹ‚o analizÄ™ zbliĹĽajÄ…cego siÄ™ ĹşrĂłdĹ‚a Ĺ›wiatĹ‚a dopiero wtedy, gdy globalny
`DetectorSceneProfile` wszedĹ‚ juĹĽ w `NIGHT_GLARE`. Dla maĹ‚ej czoĹ‚Ăłwki lub reflektorĂłw
daleko od kamery caĹ‚y kadr czÄ™sto nie zmieniaĹ‚ jeszcze `p99 / bright_fraction` na tyle,
ĹĽeby wĹ‚Ä…czyÄ‡ GLARE. W efekcie lokalnie byĹ‚o widaÄ‡ rosnÄ…cÄ… maskÄ™ i MotionFusion, ale
Ĺ›cieĹĽka alarmowa w ogĂłle nie zaczynaĹ‚a liczyÄ‡ wzrostu.

Dodatkowo 1.2.76 miaĹ‚o konserwatywne minima: okoĹ‚o 0,1% kadru, +22% wzrostu i 45 s
cooldownu. To byĹ‚o zbyt wolne do scenariusza ochrony kamery.

## Glare Threat Guard v2

1.2.77 odwraca zaleĹĽnoĹ›Ä‡. W nocnym kontekĹ›cie VEYRA najpierw szuka **lokalnego**
komponentu glare wspieranego przez surowe boxy MotionFusion.

- maĹ‚y ruchomy punkt Ĺ›wiatĹ‚a moĹĽe natychmiast obudziÄ‡ `NIGHT_GLARE`;
- nie trzeba czekaÄ‡, aĹĽ jasne ĹşrĂłdĹ‚o zmieni statystyki caĹ‚ego obrazu;
- statyczne lampy / okna / odbicia nie dominujÄ… juĹĽ metryki â€” wybierany jest komponent
  glare z najwiÄ™kszym lokalnym pokryciem ruchem;
- wzrost liczony jest z trzech sygnaĹ‚Ăłw: powierzchnia komponentu glare, jego bbox oraz
  powierzchnia wspierajÄ…cego boxa MotionFusion;
- dziÄ™ki temu osoba z czoĹ‚ĂłwkÄ… moĹĽe zostaÄ‡ rozpoznana jako zbliĹĽajÄ…ca siÄ™ nawet wtedy,
  gdy sam nasycony punkt ma prawie staĹ‚y rozmiar, ale roĹ›nie maska ruchu sylwetki;
- dalekie reflektory auta mogÄ… wejĹ›Ä‡ do recovery przy znacznie mniejszej powierzchni;
- aktywny `NIGHT_IR` jest utrzymywany jako kontekst przez krĂłtki grace period, ĹĽeby
  reflektor nie oszukaĹ‚ klasyfikatora noc/dzieĹ„ w dokĹ‚adnie najgorszym momencie.

## Szybsza reakcja

Nowe wartoĹ›ci domyĹ›lne:

- okno wzrostu: **1,8 s**;
- minimalny wzrost: **10%** zamiast 22%;
- minimalny lokalny komponent: okoĹ‚o **0,012% obrazu analitycznego**;
- overlap MotionFusion: **10%**;
- `NIGHT_GLARE` moĹĽe zostaÄ‡ wymuszony juĹĽ przez lokalny motion-supported candidate;
- 2 potwierdzenia dla normalnego wzrostu;
- fallback po 4 spĂłjnych trafieniach dla bliskiego, ruchomego ĹşrĂłdĹ‚a, ktĂłrego maska jest
  juĹĽ nasycona i nie roĹ›nie numerycznie;
- safety cooldown: **8 s** zamiast 45 s;
- nowy epizod moĹĽe uzbroiÄ‡ siÄ™ po okoĹ‚o **1,25 s** czystej przerwy.

Cooldown nadal chroni przed lawinÄ… powiadomieĹ„, ale kolejne realne przejĹ›cie czĹ‚owieka
lub auta nie jest blokowane przez prawie minutÄ™.

## Debug

`Debug â†’ GLARE MASK` pokazuje teraz:

- `WATCH` â€” brak lokalnego wsparcia;
- `LOCK` â€” motion-supported glare candidate, recovery jest juĹĽ aktywowane;
- `ALERT` â€” alarm zostaĹ‚ potwierdzony;
- wzrost `x`;
- overlap `%`;
- liczbÄ™ kolejnych trafieĹ„;
- `glare_approach_reason` w statystykach debug.

Status API zostaĹ‚ rozszerzony o `glare_approach_candidate`,
`glare_approach_candidate_hits`, `glare_approach_hits`,
`glare_approach_motion_coverage`, `glare_approach_motion_area_fraction` i
`glare_approach_reason`.

## Home Assistant

Nie jest wymagana nowa wersja integracji do samego algorytmu 1.2.77.
HACS 0.3.3 nadal odbiera natywny `glare_approach` oraz pokazuje sensor
**ZbliĹĽajÄ…ce oĹ›lepienie** i poziom powiadomieĹ„ glare.

## Safety / wydajnoĹ›Ä‡

- brak drugiego inference Corala;
- brak dodatkowego decode;
- brak zmian transportu Live;
- wykorzystywane sÄ… istniejÄ…ca pĹ‚aszczyzna Y, maska glare i boxy MotionFusion;
- stabilne profile masek pozostajÄ… `DAY / NIGHT_IR / NIGHT_WHITE_COLOR`;
- `GLARE` nadal jest tylko transient modifier.
