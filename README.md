# Hand Gesture Puzzle

**Pinch, drag and match. Your webcam becomes the controller.**

Move geometric shapes onto matching outlines using your thumb and index finger. Five levels progress from stationary targets to faster two-axis movement, with pinch calibration, two-hand play and a ghost cursor for revisiting a personal best.

Built with **Python, OpenCV and MediaPipe Hands** by **Srun Sochettra** as a course final project.

## Play

1. Follow the relaxed-hand and pinched-hand calibration prompts, or press Space to skip.
2. Show your hand until play begins. Solo gameplay uses the hand labelled Right by the tracker.
3. Move your index fingertip near a shape and pinch to grab it.
4. Drag to its matching outline and release to snap it into place.
5. On completion, press lowercase `n` or hold a pointing gesture to advance. After the final level, this restarts from level one.

The pointing hold threshold is **0.55 seconds**, with a **1-second gesture cooldown**. Open palm and fist are classified but have no gameplay action mapped in the main loop.

## Features

- **Five levels:** Warm-up, Moving Targets, Four Shapes, Crosswind and Final Challenge. Three to five shapes, with stationary, vertical and two-axis targets.
- **Filtered input:** One Euro cursor filtering, smoothed pinch ratios, separate grab/release thresholds and short tracking-loss grace periods.
- **Calibration:** two sampling phases estimate thresholds; successful results are saved locally.
- **Two-hand mode:** separate Right and Left slots can hold different shapes. This is not arbitrary two-player identity tracking.
- **Personal records and ghost replay:** best time/move pairs are stored locally; a new solo best can save its cursor/pinch path.
- **Visual feedback:** particles, trails, silhouette rim, screen shake, level cards and confetti.
- **Sound cues:** threaded Windows winsound sequences, with silent fallback elsewhere.
- **Optional recording:** save the rendered camera/game view as MP4.

Smoothing and frame pacing are implementation choices, not measured latency or guaranteed frame-rate results.

## Run locally

You need a webcam and a graphical desktop for OpenCV windows. Python **3.11** is the project's existing recommended starting point, not a newly tested compatibility guarantee.

From the repository root, using Windows PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python main.py
```

For a POSIX shell, activate with `source .venv/bin/activate` instead.

[requirements.txt](requirements.txt) pins MediaPipe to 0.10.21 and leaves OpenCV and NumPy unpinned. The code uses legacy `mp.solutions` APIs; do not upgrade MediaPipe blindly. No complete dependency lockfile or tested platform matrix is supplied.

Camera selection tries index **1**, then **0**, then **2**. Change [config.py](config.py) if needed. Capture requests 1280 × 720, but the actual camera output may differ; the playfield adjusts to received frame dimensions.

No API key or application server is configured. Package installation, camera access and codec support still need verification on the actual machine.

## Controls

Letter shortcuts are **lowercase**, even where the screen legend uses capitals. Keep the OpenCV window focused.

| Key | Action |
| --- | --- |
| `q` or Esc | Quit normal gameplay |
| Esc / Space | Quit / skip during calibration |
| `r` | Restart the current level |
| `n` | Advance after completion; restart from level one after the final level |
| `h` | Toggle debug overlay |
| `d` | Toggle landmarks, visible only with debug enabled |
| `s` / `t` / `g` | Toggle silhouette / trail / ghost |
| `2` | Toggle two-hand mode and reset the level |
| `v` | Start/stop video recording |
| `c` | Recalibrate |
| `,` / `.` | Lower/raise grab threshold and save thresholds |
| `?` or `/` | Show temporary settings panel |

Calibration handles only Esc and Space. Skipping does not explicitly reset previously loaded thresholds to defaults.

Adaptive assistance is session-local: two qualifying resets of an unfinished, started level reduce target speed to 75% and add four pixels to grab/snap radii. It is reset-driven assistance, not a general skill model.

## Local data

| Location | Contents |
| --- | --- |
| `~/.gesture_puzzle_records.json` | Best time/move pairs and calibration |
| `~/.gesture_puzzle_ghosts.json` | Cursor coordinates, relative timestamps and pinch states |
| `recordings/gpgame-<timestamp>.mp4` | Rendered camera/game footage when enabled |

`~` is the current user's home directory. Recordings are relative to the working directory. The directory is created on startup, but video recording starts only when toggled.

**Recordings are not excluded by the current ignore rules.** Inspect staged files and obtain permission before sharing footage. See [SECURITY.md](SECURITY.md).

Records compare whole seconds first, then moves. Solo and two-hand results share level keys. JSON writes suppress filesystem errors, so a displayed record is not proof that it persisted.

## Code map

| Modules | Responsibility |
| --- | --- |
| [main.py](main.py), [config.py](config.py) | Frame loop, controls and configuration |
| [vision.py](vision.py), [gestures.py](gestures.py), [calibration.py](calibration.py) | Tracking, classification and threshold sampling |
| [game.py](game.py) | State machine, hand slots, dragging and snapping |
| [renderer.py](renderer.py), [effects.py](effects.py), [motion.py](motion.py) | Drawing, particles, animation and shake |
| [persistence.py](persistence.py), [replay.py](replay.py) | Records, calibration and ghost paths |
| [recording.py](recording.py), [audio.py](audio.py), [segmentation.py](segmentation.py) | Video, Windows audio and silhouette |

## Verification and limitations

All supplied Python modules were inspected and syntax-parsed without importing or executing them. No dependency installation, webcam session, audio check or MP4 playback test was performed. No automated test suite is included.

Target frame rate and recording metadata are configured at 60 FPS, not measured here. Actual processing speed can differ, affecting exported playback timing. No complete mouse/keyboard alternative for moving shapes or global reduced-motion mode is implemented.

Read [implementation notes](docs/implementation-notes.md) for timing/replay caveats and [CONTRIBUTING.md](CONTRIBUTING.md) before making changes.

## Licence

[MIT](LICENSE), copyright 2026 Srun Sochettra. The existing licence is unchanged.
