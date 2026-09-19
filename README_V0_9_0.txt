Veyra CORE 0.9.0

- Per-camera NV12 black-frame watchdog.
- Detects only sustained, nearly uniform black Y planes; ordinary dark/IR frames are not treated as failures.
- First recovery restarts the current VAAPI decoder; repeated black output on direct camera input uses the existing go2rtc fallback.
- Decoder status reports black_frame_streak, black_frame_recoveries, sample_luma and sample_luma_range.
- FFmpeg 7.0.2 and the stable Debian VAAPI stack from 0.8.9 remain unchanged.
