# VEYRA 1.2.142

> **Updater candidate-version hotfix over 1.2.141.** 1.2.141 could build and start correctly, with Coral and iGPU healthy, but the pre-commit health gate saw `version: "unknown"` and rolled back to 1.2.140.

## Root cause
VEYRA intentionally uses a two-phase version commit: `/opt/ainvr/VERSION` must stay on the installed release until the newly built candidate passes CORE health, Coral and iGPU verification. The 1.2.141 updater attempted to expose the staged version to the candidate by bind-mounting a temporary `/tmp/.../VERSION.candidate` file over `/run/ainvr/VERSION`. In the production LXC/Docker path this temporary bind did not reach the CORE runtime reliably, so `/health` returned `version: "unknown"` even though CORE, Coral and iGPU were healthy.

## Fix
- CORE now carries a tiny **baked runtime release identity** (`core/app/release_version.py`) so the image can identify itself before the installed VERSION marker is committed. This is what makes the update installable directly from 1.2.140, whose updater is the process that starts the 1.2.142 candidate.
- The 1.2.142 updater additionally injects an **ephemeral runtime identity** through `AINVR_RUNTIME_VERSION` for future candidate starts.
- `core/app/main.py` prefers the explicit candidate identity, then the baked image identity, and only uses `/run/ainvr/VERSION` as a compatibility fallback for older images.
- The installed `/opt/ainvr/VERSION` remains unchanged during build/start/health verification.
- Only after candidate CORE + Coral + iGPU pass does the updater atomically commit `VERSION = 1.2.142`.
- CORE is then recreated from the canonical compose configuration, without the candidate override; `/opt/ainvr/VERSION` must equal `1.2.142` and the running image must report `/health.version = 1.2.142`.
- `version: unknown` is **not** accepted as success.

This preserves the rollback guarantee while removing the circular dependency that blocked 1.2.141.

## Diagnostics
If the candidate health gate fails, the updater now prints a short summary before full logs, including:
- expected candidate version,
- reported candidate version,
- Coral READY / NOT READY,
- iGPU READY / NOT READY.

## Light Threat
All Light Threat / Static Light changes from 1.2.141 are retained unchanged, including trusted fixed-light scene anchoring and compensation for motion-sensor porch lights.

## Architecture
- No second decode.
- No second Coral inference.
- No optical flow.
- No additional full-frame resize.
- `docker-compose.yml` and `live_proxy/default.conf.template` are unchanged.
- `core/app/main.py` changes only in runtime-version source selection required by the two-phase candidate health gate.

## Targeted validation
- Candidate version regression for the exact `version: unknown` failure.
- Two-phase commit ordering: candidate health before installed VERSION commit.
- Canonical CORE recreation after commit.
- Coral/iGPU health gate remains mandatory when hardware is present.
- Existing 1.2.141 Static Light scene tests and updater tests.
- **45 targeted tests passed.**
- Python compile, shell `bash -n`, YAML parse and 1.2.140 bootstrap copy-path verification passed.
- Package manifest/archive integrity is verified during release packaging.
