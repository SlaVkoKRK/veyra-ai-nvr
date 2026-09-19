VEYRA AI-NVR 0.9.6

Hotfix for model-defined classes:
- reads Ultralytics `metadata.json` embedded inside TFLite files;
- supports `TFLITE_ULTRALYTICS_METADATA.json`;
- supports legacy onnx2tf Python-literal metadata;
- keeps associated label files as fallback.

No detection, motion, tracker or notification thresholds changed.
