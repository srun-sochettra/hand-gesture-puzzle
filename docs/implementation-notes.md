# Implementation notes

These are source observations, not runtime test results or repaired defects.

## Input

Per-handedness One Euro filters smooth cursor positions; pinch ratios use exponential smoothing. Solo gameplay maintains a Right slot, while two-hand mode adds Left. READY uses the primary detected hand, but PLAYING resolves input by handedness. A left-only detection can pass readiness without controlling the solo Right slot.

Only pointing is routed from held non-pinch classifications to a gameplay action. Open palm and fist are classified but unmapped. Calibration can save thresholds; skipping does not explicitly restore defaults.

## Timing and replay

Retry resets started_at to None while an active game remains PLAYING. The PLAYING update does not initialise it again. This is a source-level retry-timer concern requiring regression testing, not an executed failure report.

Ghost samples use time relative to their first sample; playback uses the level-start marker. Startup/calibration/readiness can offset those clocks. Do not claim perfectly aligned replay without testing.

Best records compare (whole seconds, moves) and share level keys across solo/two-hand modes. A two-hand record can replace the shared best without replacing the solo ghost. There is no full campaign-resume state or separate co-op leaderboard.

JSON loaders validate only limited shapes, and writes are non-atomic with suppressed filesystem errors. These limitations are not evidence of observed corruption.

## Recording and effects

VideoRecorder creates its directory at construction and opens an mp4v writer when toggled. It checks writer opening and releases it on stop. Frames use fixed configured 60 FPS metadata, not per-frame timestamps. Playback speed and codec support require target-machine checks.

Windows audio has silent fallback elsewhere. Silhouette initialisation can disable that effect, but the main tracker still requires MediaPipe. Individual effect toggles are not a global reduced-motion mode.
