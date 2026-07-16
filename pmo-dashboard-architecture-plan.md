# PMO Dashboard & Data Architecture — Brainstorm Plan

## Context

The current PMO system uses static HTML dashboards (northlight-pmo repo, served via GitHub Pages) for project and phase tracking. These have three compounding problems:

1. **No write-back to ClickUp** — Claude reads ClickUp during sessions but never updates it. Tasks stay open even after completion. Session handoffs break because ClickUp doesn't reflect reality.
2. **PMO master dashboard is stale** — The STANDING-PMO-TRIGGER fires when a phase dashboard is committed, but the master dashboard hasn't updated since July 3. Something in the dispatcher isn't closing this loop.
3. **No live dashboard in the OS** — Dashboards are scattered HTML files with no home in the Focused Hub. No navigation between projects. No drilldown from a master view.

Goal: move from static HTML artifacts maintained by hand to a live, connected system where ClickUp is the data source and dashboards reflect actual state automatically.

---

## Phase 1 — Fix the Data Loop (1–2 sessions)

**What:** Make Claude update ClickUp during sessions without being asked.

### 1a — Standing behavior instruction
Add a CLAUDE.md to `C:\Users\erand\Desktop\northlight-pmo\` (applies when Claude works in PMO context): *After completing any task during a session, immediately mark it complete in ClickUp and append one line to that project's update-log. When a session plan is finalized and approved, push each task to ClickUp before starting work.*

This covers both task origins:
- Tasks pulled from ClickUp → completions get written back
- Tasks generated from a session plan → get pushed to ClickUp at plan approval, before work starts

### 1b — Add ClickUp write-back to phase-dashboard skill
Current skill reads ClickUp (Step 3) but never writes back. Add steps to:
- Mark completed tasks as done in ClickUp when dashboard is generated
- Push any open items from the dashboard into ClickUp as tasks if they don't exist yet
- File: `C:\Users\erand\.claude\commands\phase-dashboard.md`

### 1c — Investigate STANDING-PMO-TRIGGER
Read the dispatcher code to find where the trigger is handled and why the PMO master dashboard hasn't updated since July 3. This is a diagnosis step — fix depends on findings.
- Dispatcher repo: `C:\Users\erand\Desktop\northlight-dispatcher\`
- Look for: how the trigger message is parsed, what action it fires, whether the PMO dashboard regeneration actually runs

---

## Phase 2 — Restructure ClickUp (1 session)

**What:** Consolidate fragmented ClickUp lists into one clean structure that can drive dashboard data.

Proposed structure:
- One folder containing all project backlogs
- One list per project (OS, Website, Meetings, Skills Market, Social Media, Advisory Bot)
- Folder-level view = top-level PMO board (ClickUp supports this natively)
- Add new lists as new projects start

**Why this matters:** A consistent list structure means Claude can query ClickUp reliably by project without hunting across folders.

**Deliverable:** registry.json updated with new list IDs. Existing tasks migrated. Old structure archived or removed.

---

## Phase 3 — Decide What Replaces the PMO Master (1 session, depends on 1c findings)

**What:** The PMO master dashboard needs to retire as an HTML file. Phase 1c findings drive this decision.

Two scenarios:
- **If trigger is wired but regenerating HTML:** redirect it to write structured data (JSON) instead — feed for the Phase 4 live app
- **If trigger isn't working at all:** fix the handler to write structured data, skip HTML entirely

Success criteria: STANDING-PMO-TRIGGER produces a data update, not an HTML file. The HTML master dashboard is retired.

---

## Phase 4 — Live Dashboard in OS Focused Hub (2–3 sessions)

**What:** Replace static HTML as the living project view with a dynamic page inside the OS app.

Architecture:
- Add a `/projects` route to the dispatcher (Railway app)
- Route reads from ClickUp API on page load — always live, never stale
- Shows: list of active projects, current phase, phase status, last updated, link to drilldown
- Drilldown per project: current phase tasks (open/complete), recent decisions, recent log entries rendered as formatted HTML (not raw markdown)
- Page lives at a URL embedded in the OS Focused Hub as the primary project view

Individual phase HTML dashboards can remain as **historical archives** — they don't need to be the live view anymore.

**Note:** This is a real build. Scope carefully before starting. Define the exact page structure first.

---

## Phase 5 — Log Architecture (1 session)

**What:** Ensure learnings and decisions are consistently maintained and human-readable in the OS.

**Decision made:** Keep markdown files. Claude reads and writes `.md`. The OS app (Phase 4) renders them as formatted HTML on the fly — proper headings, tables, readable text. You see a clean rendered view; Claude sees raw markdown. Same file, two views.

Work in this phase:
- Enforce consistent structure across all `update-log.md` and `learnings.md` files (fill gaps where missing or sparse)
- Ensure the Phase 4 `/projects` route includes a rendered log view per project
- Confirm mid-session write instruction (from Phase 1a) keeps logs current without a separate trigger

---

## What This Is Not

- Notion (new paid tool, not needed)
- Google Sheets as a database (adds a third system; ClickUp + Drive is sufficient)
- Full rebuild of the OS app (Phase 4 adds a route, not a rewrite)

---

## Sequencing

| Phase | Prerequisite | Effort |
|-------|-------------|--------|
| 1a — Standing instruction | None | Small |
| 1b — Skill write-back | None | Medium |
| 1c — Trigger investigation | None | Small (diagnosis) |
| 2 — ClickUp restructure | 1c findings | Medium |
| 3 — PMO auto-update | 1c + 2 | Medium |
| 4 — Live OS dashboard | 2 + 3 | Large |
| 5 — Log architecture | Decision only | Small |

Phases 1a and 1b can start immediately. Phase 4 should not start until ClickUp structure (Phase 2) is stable — the data model needs to be right before building the UI on top of it.

---

*Brainstormed 2026-07-16. To resume: share this file back to Claude and say "let's work on PMO architecture Phase 1."*
