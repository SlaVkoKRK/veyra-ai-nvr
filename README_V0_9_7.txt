VEYRA AI-NVR 0.9.7

- Frigate-style external labelmap support (`coral.labelmap_path`).
- Auto-discovers labelmap.txt / labels.txt / classes.txt and model-name label files next to the active model.
- External label map has priority over embedded TFLite metadata.
- No changes to detector thresholds, motion, tracker, or event logic.
