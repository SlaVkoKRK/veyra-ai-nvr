# VEYRA 1.2.136

## Light Threat / Emitter Map
- Dodano twardy **Physical Emitter Eligibility Gate** przed `approach/growth/history`.
- Dla ĹşrĂłdeĹ‚ bez person/car alarm wymaga realnego **effective luminous footprint**: rdzeĹ„ + faktycznie rozjaĹ›nione lokalne otoczenie.
- MaĹ‚e odbicia, oczy zwierzÄ…t, owady, pajÄ…ki/pajÄ™czyny i punkty przy krawÄ™dzi nie mogÄ… zostaÄ‡ uratowane przez sam growth, motion ani `approach_signature`.
- Prawdziwa czoĹ‚Ăłwka/latarka/reflektory mogÄ… przejĹ›Ä‡ szybko, gdy tworzÄ… mocne halo, lokalny light impact i odpowiednio duĹĽy footprint.
- Motion overlap pozostaje narzÄ™dziem lokalizacji ROI, nie dowodem alarmowym.
- Zachowano szybki repeat alarmu (~1.2 s), IR Transition Guard i zasadÄ™ jedna analiza obrazu -> wielu konsumentĂłw.

## Updater
- Preflight wolnego miejsca i inode przed modyfikacjÄ… instalacji.
- DomyĹ›lnie wymagane min. 6 GiB, 8% wolnego filesystemu i 5% inode (progi moĹĽna nadpisaÄ‡ zmiennymi `VEYRA_UPDATE_MIN_*`).
- PrĂłba sprawdzenia Proxmox thinpool `pve/data`; gdy LXC nie ma dostÄ™pu, updater jawnie raportuje `NIEZWERYFIKOWANY` zamiast udawaÄ‡ peĹ‚ny preflight.
- Blokada przy thinpool >=92% data lub >=90% metadata, jeĹ›li dane sÄ… dostÄ™pne.
- Backup jest walidowany `tar -tzf` przed modyfikacjÄ… live tree.
- ENOSPC / BuildKit-bbolt dostajÄ… czytelny komunikat; updater nie kasuje automatycznie `/var/lib/docker/buildkit`.
- Etapy preflight/build/start/health sÄ… raportowane do istniejÄ…cego statusu aktualizacji.

## KompatybilnoĹ›Ä‡
- KanaĹ‚ aktualizacji pozostaje kompatybilny od 1.2.31.
- Live/go2rtc/nginx, `core/app/main.py` i `docker-compose.yml` bez zmian.
- `scripts/apply-update.sh` zostaĹ‚ celowo zmieniony w tej wersji z powodu hardeningu updatera.
