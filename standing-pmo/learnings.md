# Standing PMO System — Learnings

---

## Phase 5 — Health Status & Log Architecture (2026-07-16) — Closeout

- ClickUp tasks can silently end up in two lists at once — this caused in-progress tasks to appear missing from the Standing PMO list filter even though they existed. Always verify list membership directly in ClickUp if task counts look wrong after a sweep. Resolution: tasks removed from the duplicate list manually.

## Phase 5 — Health Status & Log Architecture (2026-07-16)

- Completion-based health rubrics always show red at phase start — momentum is the right signal instead. "Is something in flight? Is it blocked? Has nothing moved in 10 days?" covers the real failure modes without needing planned durations or scope baselines.
- Pre-build and idea projects should be explicitly grey (parked, not sick) — a health rubric that treats "not started" as unhealthy generates noise and erodes trust in the signal.
- Showing the *reason* for a yellow or red signal in the drawer is as important as the dot on the card — without it, the indicator is a prompt to open the drawer and find out why, which is still useful, but explaining it directly is faster.

## Phase 3+4 — Live Dashboard (2026-07-16)

- Status-based task filtering is the right default for phase-aware ClickUp sweeps — no naming convention or tag discipline required. Backlog = excluded; ready/in-progress = active this phase; complete after phase_started = done this phase. Simple, low-friction, and survives projects that don't follow any tagging convention.
- Infrastructure outages (Railway US West) can look like code failures — repeated "Scheduling build on Metal" with no build logs is an infra signal, not an app error. Always check the provider's status page before debugging code. Fix: wait + empty-commit trigger.
- Auto-updating registry.json inside the skills (phase-dashboard Step 7b, new-project Step 7) eliminates a whole class of "stale OS card" bugs. The rule: any skill that changes a project's phase must also update registry.json before posting STANDING-PMO-TRIGGER.

## Phase 2 — ClickUp Restructure (2026-07-16)

- One folder + one list per project is the right ClickUp shape for this PMO. Previous state (multiple lists per project, tasks scattered) made it impossible to sweep a project cleanly by list ID. Registry-driven list IDs are now the single source of truth.
- Social Media is intentionally excluded from Project Backlogs — its tasks live in the Marketing folder by pipeline (Threads, LinkedIn, Blog, Aurora Brief). Document this exception explicitly rather than leaving it as a null in registry.json with no explanation.
- 57 tasks migrated with no losses. Always do a before/after count when mass-moving tasks — ClickUp doesn't warn you if a move silently fails.

## Phase 1 — Data Loop Fix (2026-07-16)

- The STANDING-PMO-TRIGGER had two separate problems: (1) the check in slack-events.js sat inside an Elizabeth-only message guard so bot-posted messages were filtered before reaching it; (2) the Slack app was not subscribed to `message.channels` bot events, so channel messages never reached the webhook at all. Both had to be fixed. Always verify the Slack app's event subscriptions when a channel-based trigger isn't firing — the code can be correct but the webhook never called.
- ClickUp write-back must happen during the session, not after — by the time a phase dashboard is generated, ClickUp is already stale. Embedding it in the skill rather than relying on manual follow-through is the right pattern.
- CLAUDE.md in the PMO repo is the right place for standing PMO behaviours — it applies automatically whenever Claude is working in that context without needing to be in the session prompt.
