# Northlight Meetings — Learnings

## Phase 1 — Recording + Transcription Foundation (2026-07-03)

- **Audio driver state can silently block everything downstream.** Voicemeeter signal was reaching the A1 bus (visible on the level meter) but producing no sound — because Realtek Audio Console had the speakers muted at the driver level. Always check the hardware audio console when Voicemeeter routing appears correct but output is silent.
- **Webpack + Electron main process package incompatibilities surface late.** node-fetch and form-data both appeared to install cleanly but failed at runtime due to webpack bundling. Node 24's native fetch/FormData/Blob globals are the right answer for Electron main process — no packages needed.
- **Test with representative audio, not YouTube.** YouTube audio is mastered loud (music + voice + effects). It's a poor proxy for call audio and caused misleading gain calibration results. Real call testing (two conversational voices) is the correct validation for the audio pipeline.
