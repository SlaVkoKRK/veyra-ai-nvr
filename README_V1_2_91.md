# VEYRA 1.2.91 â€” poprawka popupu aktualizacji + MotionTrace Gesture Trigger

VEYRA 1.2.91 usuwa faĹ‚szywe ponowne wyĹ›wietlenie popupu **Aktualizacja dostÄ™pna** po udanym update i dodaje pierwszÄ… wersjÄ™ gestĂłw sterujÄ…cych Home Assistantem bez staĹ‚ego MediaPipe/OpenVINO i bez dodatkowej inferencji obrazu.

## Popup aktualizacji

W 1.2.88â€“1.2.90 `sessionStorage` mĂłgĹ‚ zachowaÄ‡ wynik `available=true` sprzed aktualizacji. Po przejĹ›ciu np. z 1.2.89 do 1.2.90 zwykĹ‚e F5 mogĹ‚o wiÄ™c ponownie pokazaÄ‡ popup dla wersji, ktĂłra byĹ‚a juĹĽ zainstalowana.

Od 1.2.91:

- cache przechowuje tylko zdalnÄ… wersjÄ™ `latest`;
- decyzja o popupie jest zawsze ponownie porĂłwnywana z aktualnym `s.version` z istniejÄ…cego `/api/status`;
- wykrycie zmiany wersji VEYRY czyĹ›ci stary cache, dismissal i otwarty popup;
- `latest <= current` automatycznie chowa popup i usuwa stary cache;
- nie dodano ĹĽadnego nowego pollingu ani requestu do hot-path.

## MotionTrace Gesture Trigger

Pierwsza wersja Gesture Trigger zostaĹ‚a zaprojektowana pod niski koszt CPU i sprzÄ™t VEYRY. Zamiast uruchamiaÄ‡ stale MediaPipe Hand Landmarker albo drugi model, uĹĽywa juĹĽ istniejÄ…cych danych:

```text
potwierdzony PERSON
       +
MotionFusion motion_boxes
       +
strefa gestĂłw
       â†“
krĂłtka trajektoria ruchu wzglÄ™dna do bbox PERSON
       â†“
CIRCLE CW / CIRCLE CCW / WAVE / UP / DOWN
       â†“
MQTT ainvr/gestures
       â†“
Home Assistant
```

### Zasady CPU

- funkcja jest domyĹ›lnie **OFF** per kamera;
- nie czyta pikseli i nie wykonuje resize;
- nie uĹĽywa `cv2`, NumPy, optical flow, MediaPipe ani OpenVINO;
- nie dodaje inferencji Coral;
- ignoruje `analysis_skipped` i pracuje tylko na realnych klatkach MotionFusion;
- przy wyĹ‚Ä…czonej funkcji koszt sprowadza siÄ™ do natychmiastowego warunku wyjĹ›cia;
- debug trajektorii powstaje wyĹ‚Ä…cznie z juĹĽ zebranych punktĂłw i jest rysowany tylko przy otwartym Debug + Tracki.

W lokalnym mikrobenchmarku samego recognizera (nie pomiar i5-6500T) wyĹ‚Ä…czony Gesture Trigger kosztowaĹ‚ okoĹ‚o **0.19 Âµs/call**, a aktywna analiza jednego PERSON z kilkoma istniejÄ…cymi motion boxami okoĹ‚o **20 Âµs/call**.

## ObsĹ‚ugiwane gesty

- `circle_cw` â€” kĂłĹ‚ko zgodnie z ruchem wskazĂłwek zegara;
- `circle_ccw` â€” kĂłĹ‚ko przeciwnie do ruchu wskazĂłwek;
- `wave` â€” celowe machanie lewo/prawo;
- `up` â€” wyraĹşny ruch rÄ™ki w gĂłrÄ™;
- `down` â€” wyraĹşny ruch rÄ™ki w dĂłĹ‚.

Gesture Trigger wymaga potwierdzonego `person`, minimalnego rozmiaru osoby, obecnoĹ›ci w konfigurowanej strefie, minimalnej pewnoĹ›ci i liczby potwierdzeĹ„. Po akceptacji obowiÄ…zuje cooldown.

## Konfiguracja per kamera

`Kamera -> Ustawienia -> Zaawansowane -> Gesture Trigger Â· Home Assistant`

DostÄ™pne sÄ…:

- ON/OFF;
- minimalna pewnoĹ›Ä‡;
- dĹ‚ugoĹ›Ä‡ okna trajektorii;
- cooldown;
- minimalna wysokoĹ›Ä‡ `person` w pikselach;
- nazwa strefy;
- prostokÄ…tna strefa gestĂłw w procentach obrazu;
- osobne ON/OFF dla kaĹĽdego gestu.

W Debug po wĹ‚Ä…czeniu **Tracki** wyĹ›wietlana jest trajektoria oraz stan `GESTURE ARMED` / kandydat.

## MQTT / Home Assistant

KaĹĽdy zaakceptowany gest jest publikowany na:

```text
ainvr/gestures
```

oraz na topic kamery:

```text
ainvr/<camera>/gesture
```

PrzykĹ‚adowy payload:

```json
{
  "type": "gesture",
  "camera": "kamera_brama",
  "gesture": "circle_cw",
  "confidence": 0.91,
  "track_id": 42,
  "zone": "brama",
  "source": "motion_trace"
}
```

To Home Assistant decyduje, czy taki event otwiera bramÄ™, zapala Ĺ›wiatĹ‚o czy wykonuje innÄ… akcjÄ™. VEYRA nie wykonuje bezpoĹ›rednio sterowania urzÄ…dzeniem.

PrzykĹ‚ad:

```yaml
trigger:
  - platform: mqtt
    topic: ainvr/gestures
condition:
  - condition: template
    value_template: >
      {{ trigger.payload_json.camera == 'kamera_brama'
         and trigger.payload_json.gesture == 'circle_cw'
         and trigger.payload_json.confidence | float > 0.85 }}
action:
  - service: cover.open_cover
    target:
      entity_id: cover.brama
```

## MediaPipe

MediaPipe pozostaje moĹĽliwym pĂłĹşniejszym **opcjonalnym verifierem** uruchamianym na kilka sekund po uzbrojeniu gestu, jeĹĽeli testy z prawdziwych kamer pokaĹĽÄ…, ĹĽe MotionTrace jest za maĹ‚o precyzyjny dla maĹ‚ych lub bardzo odlegĹ‚ych dĹ‚oni. Nie jest uruchamiany stale w 1.2.91.

## Live

Chronione pliki Live pozostajÄ… byte-identyczne z 1.2.90:

- `core/app/main.py`
- `docker-compose.yml`
- `live_proxy/default.conf.template`
- `scripts/apply-update.sh`

## Aktualizacja

```bash
/opt/ainvr/scripts/apply-update.sh /path/veyra_update_v1_2_91.zip
```
