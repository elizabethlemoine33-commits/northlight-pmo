# Standing PMO System — Learnings

---

## Phase 2 — ClickUp Restructure (2026-07-16)

- One folder + one list per project is the right ClickUp shape for this PMO. Previous state (multiple lists per project, tasks scattered) made it impossible to sweep a project cleanly by list ID. Registry-driven list IDs are now the single source of truth.
- Social Media is intentionally excluded from Project Backlogs — its tasks live in the Marketing folder by pipeline (Threads, LinkedIn, Blog, Aurora Brief). Document this exception explicitly rather than leaving it as a null in registry.json with no explanation.
- 57 tasks migrated with no losses. Always do a before/after count when mass-moving tasks — ClickUp doesn't warn you if a move silently fails.

## Phase 1 — Data Loop Fix (2026-07-16)

- The STANDING-PMO-TRIGGER had two separate problems: (1) the check in slack-events.js sat inside an Elizabeth-only message guard so bot-posted messages were filtered before reaching it; (2) the Slack app was not subscribed to `message.channels` bot events, so channel messages never reached the webhook at all. Both had to be fixed. Always verify the Slack app's event subscriptions when a channel-based trigger isn't firing — the code can be correct but the webhook never called.
- ClickUp write-back must happen during the session, not after — by the time a phase dashboard is generated, ClickUp is already stale. Embedding it in the skill rather than relying on manual follow-through is the right pattern.
- CLAUDE.md in the PMO repo is the right place for standing PMO behaviours — it applies automatically whenever Claude is working in that context without needing to be in the session prompt.
