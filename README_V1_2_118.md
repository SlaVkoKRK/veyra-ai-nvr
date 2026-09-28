# VEYRA 1.2.118

## Static Light zamiast twardych masek GLARE

- RÄ™czna `Static Light Mask` zachowuje istniejÄ…cy zapis `night.manual_glare_masks`, ale dziaĹ‚a jako miÄ™kki priorytet dla Light Threat, a nie twarde veto.
- `reject_overlap_percent` pozostaje kompatybilnym polem konfiguracji i okreĹ›la moment rozpoczÄ™cia tĹ‚umienia znanego staĹ‚ego Ĺ›wiatĹ‚a.
- SpĂłjna translacja ĹşrĂłdĹ‚a, wzrost jasnoĹ›ci/obszaru, carrier support lub realny wpĹ‚yw Ĺ›wiatĹ‚a na otoczenie mogÄ… przebiÄ‡ priorytet maski.
- `Adaptive Static Light Map` wykorzystuje istniejÄ…cÄ… pamiÄ™Ä‡ `dynamic_glare_*`, ale obniĹĽa confidence dla nauczonych staĹ‚ych ĹşrĂłdeĹ‚ zamiast tworzyÄ‡ Ĺ›lepÄ… strefÄ™.
- Stare klucze configu i techniczne identyfikatory pozostajÄ… dla zgodnoĹ›ci aktualizacji z wersji 1.2.31+.
- Nie dodano drugiego Coral inference, dodatkowego streamu, optical flow ani dodatkowego peĹ‚noklatkowego pipeline'u.

## Smuklejsze cienie UI

- Znacznie delikatniejsze cienie kart kamer na Dashboardzie.
- Delikatniejsze karty ostatnich zdarzeĹ„ i Galerii.
- Delikatniejsze kamery oraz gĂłrny toolbar Monitor Wall.
- Odchudzone cienie dialogĂłw, popupĂłw i menu wyboru kamery.
- Zachowane zostaĹ‚y cienkie obwĂłdki focus oraz semantyczne akcenty aktywnej detekcji.
