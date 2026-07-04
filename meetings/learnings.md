# Northlight Meetings — Learnings

## Phase 3 — Google Integrations (2026-07-04)

- **Drive API and Docs API are separate GCP enablements.** Enabling the Drive API does not enable the Docs API. The batchUpdate call (replaceAllText) requires the Docs API to be enabled independently — easy to miss since both are "Google" APIs in the same project.
- **LLM date inference needs an anchor year explicitly in the prompt.** Without year guidance, the model defaulted to 2024 for a partial date ("September 15th") even though the current year is 2026. Injecting `new Date().getFullYear()` at prompt-build time costs nothing and eliminates the ambiguity permanently.
- **Hidden fields in a review screen become invisible bugs.** The isoDate field was stored correctly by the LLM but had no UI — so even when Elizabeth manually edited the date text, the actual calendar date didn't change. Any field that drives downstream behaviour needs to be visible and editable in the review screen.

## Phase 2 — Analysis + Review (2026-07-04)

- **Template-keyed JSON from day one saves a parsing layer in Phase 3.** The original Replit app returned `sowDraft` as a freeform markdown string. Changing it to a structured object whose keys match the Google Docs `{{tag}}` names exactly means the Phase 3 Drive push is a mechanical `replaceAllText` loop — no extraction, no mapping, no ambiguity about what goes where.
- **Post-call verbal feedback is cheap and high-value.** A 30-second spoken note transcribed in parallel with the main audio and injected as HIGH PRIORITY context meaningfully improves LLM output quality — it captures corrections and priorities that never make it into the transcript. The pattern (separate mic-only recorder, parallel Whisper pass, prompt injection) is reusable for any context-enrichment scenario.
- **`approved` flags on every item make Phase 3 integration clean before it's built.** Storing `approved: true/false` on each action item, task, and decision in the session JSON at save time means Phase 3 push logic never needs to re-derive intent — it just filters. This is the right place to make that decision.

## Phase 1 — Recording + Transcription Foundation (2026-07-03)

- **Audio driver state can silently block everything downstream.** Voicemeeter signal was reaching the A1 bus (visible on the level meter) but producing no sound — because Realtek Audio Console had the speakers muted at the driver level. Always check the hardware audio console when Voicemeeter routing appears correct but output is silent.
- **Webpack + Electron main process package incompatibilities surface late.** node-fetch and form-data both appeared to install cleanly but failed at runtime due to webpack bundling. Node 24's native fetch/FormData/Blob globals are the right answer for Electron main process — no packages needed.
- **Test with representative audio, not YouTube.** YouTube audio is mastered loud (music + voice + effects). It's a poor proxy for call audio and caused misleading gain calibration results. Real call testing (two conversational voices) is the correct validation for the audio pipeline.
