# Security

Veyra is designed for local/private-network deployment.

## Repository hygiene

Never commit:
- `.env` or credentials,
- `config/ainvr.yaml` or migrated user configuration,
- generated go2rtc configuration,
- camera RTSP URLs containing credentials,
- MQTT credentials,
- model files,
- event databases, snapshots or `/data`.

Only `config/ainvr.example.yaml` is intended for source control. It contains placeholders only.

## Web control bridge

The Veyra web container does not receive Docker socket or root access. Web update/restart requests are written to `/data/control` and a restricted host-side systemd service executes only an allowlist of actions: `update`, `restart-core`, and `restart-all`.

Updates preserve `/config`, `/models`, `/data` and `.env`, create backups, perform a health/version check, and roll back on failure.
