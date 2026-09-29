# VEYRA 1.2.125

## Dashboard / Review / Emitter Map hotfix

- Naprawiono wieczny stan `Ĺadowanieâ€¦` w dolnym pasku panelu gĹ‚Ăłwnego. PrzyczynÄ… byĹ‚o uĹĽycie helpera formatowania bajtĂłw z innego skryptu strony; wyjÄ…tek zatrzymywaĹ‚ rĂłwnieĹĽ synchronizacjÄ™ stanĂłw CAM/MOT/DET/SNP/REC/SCN.
- PrzywrĂłcono osobne kolory aktywnych etykiet CAM, MOT, DET i SNP w jasnym i ciemnym motywie.
- Przebudowano timeline Review: bez czarno-biaĹ‚ego prostokÄ…ta, z powierzchniÄ… i kontrastem zaleĹĽnym od motywu, czytelnÄ… osiÄ… i znacznikiem czasu.
- Mobile Review: drag palcem przewija zakres, pinch skaluje, a podczas dotyku/przeciÄ…gania pokazywana jest etykieta czasu i pionowy wskaĹşnik.
- Emitter Map Debug nie jest juĹĽ zaleĹĽna od wĹ‚Ä…czonych powiadomieĹ„. W NIGHT analiza/cache odĹ›wieĹĽajÄ… siÄ™ z jednego runtime Light Threat pass; ustawienia powiadomieĹ„ wpĹ‚ywajÄ… dopiero na emisjÄ™ alarmu.
- Zachowano zasadÄ™ jedna analiza obrazu -> wielu konsumentĂłw oraz chroniony tor Live.
