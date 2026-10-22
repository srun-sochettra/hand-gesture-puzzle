# Security and camera-data guidance

This is a local webcam game using third-party native image-processing dependencies. It is not security-audited. No supported-version programme, response deadline or guaranteed remediation schedule is established here.

## Camera and recordings

The inspected application contains no explicit network upload code. This is a source observation, not an independently verified guarantee about the complete dependency/runtime environment.

Optional MP4 recording includes the rendered camera view and can capture people and private surroundings. Obtain permission before recording or sharing. The current .gitignore does not exclude recordings. Inspect staged files; adding an ignore rule later does not remove previously tracked footage or history.

## Local state

Records/calibration and ghost paths are JSON files in the user's home directory. Ghost files contain coordinates, timestamps and pinch states rather than camera frames. The application does not encrypt these files.

Loaders perform limited structural validation. Do not import untrusted state files or assume malformed values are harmless. Writes suppress filesystem errors and are not atomic. A success overlay is not verified durable storage.

MediaPipe is pinned, but OpenCV and NumPy are not. Review installed dependencies separately; syntax parsing does not assess native-library security.

## Reporting

For ordinary defects, provide the affected module, environment and minimal reproduction. For sensitive concerns, use a verified private maintainer channel if available, or request a private contact without posting details. No dedicated mailbox or enabled private-reporting feature is confirmed by the supplied repository.

Do not publish private footage, credentials or identifying local paths. Address original exposures rather than relying only on deleting the current file. This documentation does not implement security fixes.
