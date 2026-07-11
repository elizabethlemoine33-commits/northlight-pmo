# Social Media — Learnings

## Phase 1 — Founder Campaign Launch (2026-07-11)
- Buffer's 10-post queue cap is a real constraint at daily Threads volume — Drive queue + date-triggered dispatch is the right workaround; don't try to fight it
- `.env` values containing `#` must be quoted or dotenv silently truncates them — cost a full debugging session to discover
- Deduplication matters: same post on both LI Personal and LI Page reads as spam to followers who follow both channels; pick one per post
- Social card images are a bottleneck — have them ready before the post slot arrives, not after
