# VEYRA 1.2.74 â€” unified UI + slim Vision + first remote update test

VEYRA 1.2.74 is the first release intended to be installed from the **Ustawienia â†’ Aktualizacje** screen introduced in 1.2.73. A manual ZIP is still provided as a fallback.

## Unified navigation strips

All secondary tab/filter strips now use the same visual language as Gallery filters:

- System Settings: `Veyra / Vision / Aktualizacje`;
- Statistics: `PrzeglÄ…d / Kamery / Coral / Vision`;
- Logs source tabs;
- Camera settings tabs;
- Gallery filters remain the reference style.

The strips are horizontally scrollable on narrow/mobile layouts instead of widening the page.

## Updates UI

The Updates tab is rebuilt as a compact status card with:

- installed version;
- available version;
- updater state;
- preserved-data / rollback guarantees;
- a single update action and status area.

The old large flat status block is gone.

## Camera Debug layout

On wide desktop screens (`>= 1360 px`) Debug controls move to a dedicated right-side column so the camera image remains the primary element. On narrower desktop windows and mobile the Debug controls stay below the image.

## Vision cleanup

The runtime Vision stack is now intentionally **Ollama-only**:

- lightweight `vision-worker` orchestration container;
- no OpenVINO / Torch / Transformers stack inside the Vision worker;
- no GPU device passed into the orchestration worker â€” inference stays in Ollama;
- old Florence and person/vehicle/bike verifier model directories are removed by the update;
- retired legacy Vision source files are removed;
- Docker no longer mounts the old `/models/vision` tree into `vision-worker`;
- obsolete Vision selector environment variables are ignored now and are purged by the new updater on subsequent updates.

Historical Gallery metadata remains readable so old events do not break.

## Vision behavior

The current one-shot lifecycle remains unchanged:

```text
track active -> final best snapshot selected -> track ends -> one Vision job
```

The Vision job uses the saved Coral 512 image paired with that final snapshot. No extra Coral inference is added.

The stock VLM remains `qwen3.5:0.8b`. New event metadata no longer writes historical Florence defaults.

## Live safety

Live media remains:

```text
browser -> nginx -> go2rtc -> camera
```

`core/app/main.py` and `live_proxy/default.conf.template` are byte-identical to 1.2.73. `docker-compose.yml` is intentionally changed only to clean the Vision worker service. No Python video relay, custom SourceBuffer or additional detector pass is introduced.

## Remote update test

From an installed **1.2.73**:

1. Open **Ustawienia â†’ Aktualizacje**.
2. Click **SprawdĹş aktualizacje**.
3. The panel should show `1.2.74` as available and change the button to **Aktualizuj do 1.2.74**.
4. Confirm the update.
5. VEYRA downloads the GitHub package, verifies SHA256, makes a code backup, applies the update, rebuilds the slim Vision worker, restarts/health-checks the services and rolls back on failure.

The manual ZIP uses the existing command as a fallback:

```bash
/opt/ainvr/scripts/apply-update.sh /path/veyra_update_v1_2_74.zip
```
