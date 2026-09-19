VEYRA AI-NVR 0.9.5

- Class names are now loaded automatically from the active model metadata / embedded labels.
- `classes:` is no longer required in ainvr.yaml.
- Existing installations with legacy `classes:` remain compatible as a fallback.
- Runtime refuses unsafe numeric class_N placeholders when semantic labels cannot be resolved.
- Detector class count follows the resolved model labels.
