# Contributing

Keep changes focused on the gesture puzzle and its existing modules. Read the [README](README.md), [implementation notes](docs/implementation-notes.md) and [code of conduct](CODE_OF_CONDUCT.md).

Use the README setup and existing MediaPipe pin. Put shared tuning values in config.py and preserve the separation between tracking, game state, rendering and persistence. Avoid new frameworks or dependencies without a concrete need.

## Verification

No automated test suite is supplied. Add a focused regression check for changed non-trivial logic where practical. Distinguish syntax checks, mocked logic tests and real webcam sessions.

Relevant scenarios include camera fallback/read failures; calibration success, insufficient samples and skipping; handedness and temporary hand loss; pinch hysteresis and snapping; retry/next-level timing; two-hand shape ownership; record/ghost interactions; malformed or unwritable JSON; and MP4 output on the target machine.

Only report checks actually performed. State platform, Python/package versions and relevant settings for camera/tracking issues. Performance claims require a measurement method and real results, not just the configured FPS target.

Use permitted footage and redact private surroundings. Review generated files before committing: recordings are not currently ignored.

## Pull requests

Describe the problem, focused change, verification and remaining gaps. Keep unrelated dependency upgrades and redesigns separate. Follow [SECURITY.md](SECURITY.md) for sensitive reports.
