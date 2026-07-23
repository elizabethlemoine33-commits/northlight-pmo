# Advisory Board Bot System — Learnings

*Newest at top. Append only — never edit past entries.*

---

## Phase 2 — Chat Bubbles, Memory Upgrades & Casey Conversations (2026-07-23)
- AB model outputs labels in `**Role (Name):**` format (role first, name in parens) — different from Round Table's `**Name:**` pattern. The bubble parser needs separate handling per system; never assume label format is consistent across orchestrators.
- Dedicated Slack bots per conversational agent (not persona-switching on a shared bot) is the right architecture — cleaner identity, cleaner routing, no cross-contamination of signing secrets and event streams. The middle-dot character (·) causes Slack app creation to silently fail; use plain hyphens or spaces in app names.
- Cross-table memory with a soft gate is a reasonable starting point, but it creates an untestable assumption — always define the 90-day kill signal at the time of shipping, not after. "Can I point to a concrete example where this changed an outcome?" is the right standard.

---
