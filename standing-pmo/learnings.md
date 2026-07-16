# Standing PMO System — Learnings

---

## Phase 1 — Data Loop Fix (2026-07-16)

- The STANDING-PMO-TRIGGER was wired correctly in the dispatcher code but silently broken because it lived inside an Elizabeth-only message guard — bot-posted messages never reached it. Always check whether a handler can receive messages from the expected sender before assuming it works.
- ClickUp write-back must happen during the session, not after — by the time a phase dashboard is generated, ClickUp is already stale. Embedding it in the skill rather than relying on manual follow-through is the right pattern.
- CLAUDE.md in the PMO repo is the right place for standing PMO behaviours — it applies automatically whenever Claude is working in that context without needing to be in the session prompt.
