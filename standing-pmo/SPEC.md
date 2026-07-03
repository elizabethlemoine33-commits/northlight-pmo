# Northlight Standing PMO — System Spec
**Status:** Draft · July 3, 2026  
**Author:** Elizabeth Lemoine + Claude  
**Goal:** Automated, cross-project PMO that stays current without manual maintenance

---

## What This Is

A system of three connected components that together give Northlight a living, automated project portfolio — from first idea in Claude Chat through active build and into steady state.

It replaces the current manual process of opening Claude Code sessions to update dashboards by hand.

### MCP Sellability Note
This system should be built with future generalization in mind. All Northlight-specific values (project names, Slack channels, Drive paths, GitHub org, schedule times) must live in a single config file — never hardcoded in core logic. This makes the system extractable as a sellable MCP skill with minimal rework. Every design decision should ask: "would another small business owner be able to configure this?"

---

## The Three Components

| Component | Where it runs | Trigger | What it does |
|---|---|---|---|
| `/new-project` (Chat) | Claude Chat | Manual | Captures idea, posts to parking lot, creates Drive brief |
| `/new-project` (Code) | Claude Code | Manual | Registers project, bootstraps PMO |
| Standing PMO skill | Dispatcher (Railway) | Scheduled + event | Sweeps all sources, updates portfolio |

Plus one update to the existing PMO skill: it commits to GitHub instead of writing to Desktop.

---

## Decisions Locked

| Decision | Value |
|---|---|
| GitHub repo name | `northlight-pmo` |
| GitHub Pages visibility | Public |
| Standing PMO schedule | Sunday 7 PM + Wednesday 8 AM |
| Slack channel (PMO summaries) | `#northlight-pmo` (new, dedicated) |
| Status change behaviour | Auto-apply (no confirmation needed) |
| Drive brief location | New folder: `Northlight PMO Briefs/` (to be created) |
| New idea destination | `#parking-lot-of-ideas` Slack channel |

---

## Component 1 — `/new-project` in Claude Chat

### Purpose
Capture a new project idea before it has a name, a repo, or a plan. Posts it to the parking lot so it isn't lost, and creates a Drive brief if the idea is developed enough to warrant one.

### Trigger
Elizabeth runs `/new-project` in Claude Chat.

### Inputs (skill asks Elizabeth)
1. Working name for the project
2. One sentence: what is it and why does it exist?
3. Status: Idea / Pre-build / Building / Live
4. Is this ready for a Drive brief, or just parking it for now? (Parking lot only / Full brief)

### What it does — Parking lot only
1. Posts to `#parking-lot-of-ideas` with structured message:
   ```
   💡 New idea: [Project Name]
   [One sentence description]
   Status: Idea — not yet ready to build
   Added via /new-project · [date]
   ```
2. No Drive file, no PMO folder, no GitHub commit

### What it does — Full brief
1. Posts to `#parking-lot-of-ideas` (same as above, with note "Drive brief created")
2. Creates a structured brief in Google Drive at `Northlight PMO Briefs/[project-name]-brief.md`
3. Posts a follow-up message to `#northlight-pmo` with Drive link

### Drive brief template
```
# [Project Name] — Project Brief
Created: [date]
Status: [Idea / Pre-build / Building / Live]

## What it is
[one sentence]

## Why it exists
[open field — Elizabeth fills in or Claude drafts from conversation]

## Open questions
[bullet list — what needs to be decided before building]

## Links
- GitHub: —
- PMO folder: — (not yet registered in Code)
- Parking lot: [link to Slack message]

## Notes
[free field — capture anything from the Chat conversation]
```

### Configurability (for future MCP sale)
The following must be config values, not hardcoded:
- `parking_lot_channel` (default: `#parking-lot-of-ideas`)
- `pmo_channel` (default: `#northlight-pmo`)
- `drive_brief_folder` (default: `Northlight PMO Briefs/`)
- `idea_message_format` (the Slack message template)

### What it does NOT do
- Does not create a PMO folder (that's Code's job)
- Does not add a portfolio card
- Does not commit anything to GitHub

---

## Component 2 — `/new-project` in Claude Code

### Purpose
Full project registration. Converts a Chat idea (or a project Elizabeth is ready to build) into a tracked entry in the portfolio system.

### Trigger
Elizabeth runs `/new-project` in Claude Code, typically from the `northlight-pmo/` directory.

### Inputs
1. Project name (or reads from Drive brief if one exists)
2. Drive brief URL (optional — skill searches Drive `Northlight PMO Briefs/` for a matching brief automatically)
3. Status: Idea / Pre-build / Building / Live
4. GitHub repo URL (optional — can be added later)
5. Local folder path (optional — can be added later)

### What it does
1. Reads Drive brief if found (pre-populates name, description, status, open questions)
2. Creates `northlight-pmo/[project-slug]/` folder
3. Creates bootstrap files from `_templates/`:
   - `update-log.md` — empty, with header and format template
   - `learnings.md` — empty, with header and format template
   - `README.md` — project name, description, status, links
4. Adds a new project card to `northlight-pmo/index.html` (portfolio dashboard)
   - Status badge from input
   - 0 open items
   - No phase dashboards yet
   - Links to Drive brief, GitHub repo, local folder (whatever is known)
5. Updates `northlight-pmo/data/registry.json` — adds project entry
6. Commits all changes to GitHub: `feat: register [project name]`
7. Posts to `#northlight-pmo`: "New project registered: [name] — PMO folder live at [GitHub Pages URL]"

### registry.json structure
```json
{
  "config": {
    "owner": "Elizabeth Lemoine",
    "org": "elizabethlemoine33-commits",
    "pmo_repo": "northlight-pmo",
    "pages_url": "https://elizabethlemoine33-commits.github.io/northlight-pmo/",
    "drive_brief_folder": "Northlight PMO Briefs",
    "parking_lot_channel": "parking-lot-of-ideas",
    "pmo_channel": "northlight-pmo",
    "schedule": ["Sunday 19:00", "Wednesday 08:00"]
  },
  "projects": [
    {
      "id": "northlight-os",
      "name": "Northlight OS",
      "slug": "os",
      "status": "live",
      "github": "https://github.com/elizabethlemoine33-commits/northlight-dispatcher",
      "local": "C:/Users/erand/Desktop/northlight-dispatcher",
      "drive_brief": null,
      "pmo_folder": "os/",
      "portfolio_card_id": "card-northlight-os",
      "registered": "2026-05-24",
      "last_refreshed": "2026-07-03"
    }
  ]
}
```

Note: the `config` block at the top is what makes this system extractable for another user — they change config, nothing else.

### Configurability (for future MCP sale)
- All paths, channel names, URLs come from `registry.json` config block
- Skill reads config before doing anything — no hardcoded values in skill logic

---

## Component 3 — Standing PMO Skill (Dispatcher)

### Purpose
Keep the portfolio dashboard current without Elizabeth having to do it manually. Sweeps all sources, diffs against last run, updates the portfolio, notifies on changes.

### Trigger options (all three active)
1. **Scheduled** — Sunday 7 PM + Wednesday 8 AM
2. **Event** — new phase dashboard committed to `northlight-pmo/` → immediate run
3. **Manual** — Elizabeth posts `skill:standing-pmo` in Slack

### Sources (what it reads)
| Source | What it looks for |
|---|---|
| ClickUp | Open tasks per project · status changes since last run |
| Slack | Messages in project channels since last refresh · decisions · blockers · completions |
| GitHub | New commits · PRs · repo activity per project |
| Phase dashboards | New files in `northlight-pmo/*/` · progress % · known issues |
| Update logs | New entries since last run timestamp |
| Claude sessions | Recent sessions mentioning projects · open items · decisions |
| Google Drive | New/updated briefs in `Northlight PMO Briefs/` |

### Process
1. Read `data/last-refresh.json` for last run timestamp
2. Read `data/registry.json` for project list and config
3. Sweep all sources scoped to "since last refresh"
4. For each project:
   - Pull open ClickUp tasks (count + top items by priority)
   - Pull Slack mentions since last refresh
   - Check for new phase dashboards in the project's PMO folder
   - Check update log for new entries
   - Check Drive for updated briefs
   - Determine if status has changed — apply automatically
5. Reconcile conflicts across sources (Slack > ClickUp for recency; phase dashboard > both for structural changes)
6. Update `northlight-pmo/index.html`:
   - Regenerate open items table
   - Update project card status badges, progress %, open item count
   - Update "Last refreshed" timestamp in footer
7. Write changelog entry to `data/changelog.md`
8. Update `data/last-refresh.json`
9. Commit + push to GitHub: `chore: standing PMO refresh [date] [trigger]`
10. Post summary to `#northlight-pmo`:
    - What closed since last run
    - What's newly open
    - Status changes applied
    - What needs Elizabeth's attention (flagged items only)

### What it does NOT update
- Individual phase dashboards — existing PMO skill's job
- Content of update logs or learnings — written by sessions and PMO skill
- Parking lot Slack messages — those are inputs, not outputs

### last-refresh.json structure
```json
{
  "last_run": "2026-07-06T19:00:00Z",
  "triggered_by": "schedule-sunday",
  "sources_swept": ["clickup", "slack", "github", "sessions", "drive"],
  "projects_updated": ["northlight-os", "northlight-meetings"],
  "items_closed_since_last": 2,
  "items_opened_since_last": 3,
  "status_changes": [],
  "next_scheduled": "2026-07-09T08:00:00Z"
}
```

### Configurability (for future MCP sale)
- Schedule times from `registry.json` config block
- Source list is configurable (a user without GitHub can disable that source)
- Slack channels from config
- Conflict resolution rules documented and overridable

---

## Updated Existing PMO Skill

### What changes
Currently: writes HTML phase dashboards to `C:\Users\erand\Desktop\[project]\PMO\`  
After: writes to `northlight-pmo/[project-slug]/phase-N.html` (local clone of the repo), commits + pushes

### What stays the same
- How it synthesizes information from sessions, Slack, specs
- The HTML format and design system
- Elizabeth still triggers it manually when a phase closes or she wants a refresh

### New behaviour after writing a phase dashboard
Posts to `#northlight-pmo` with a special flag the dispatcher watches for:
```
STANDING-PMO-TRIGGER: new dashboard committed — os/phase-3.html
```
Standing PMO skill sees this flag → triggers immediate portfolio refresh outside the normal schedule.

---

## Repository Structure — `northlight-pmo`

```
northlight-pmo/
├── index.html                    ← Portfolio dashboard
├── data/
│   ├── registry.json             ← Project registry + system config (source of truth)
│   ├── last-refresh.json         ← Timestamp + stats from last Standing PMO run
│   └── changelog.md              ← Append-only log of every Standing PMO run
├── os/
│   ├── index.html                ← OS master dashboard
│   ├── phase-1.html through phase-6.html
├── meetings/
│   └── phase-1.html
├── vault/
├── website/
├── skills-market/
├── tableready/
├── advisory-board/
└── _templates/
    ├── phase-dashboard.html      ← Template the PMO skill uses for new phase dashboards
    ├── update-log.md
    └── learnings.md
```

GitHub Pages serves from root → `elizabethlemoine33-commits.github.io/northlight-pmo/`

---

## Build Sequence

**Session 1 — Foundation** ✅ Complete — July 3, 2026
- [x] Create `northlight-pmo` GitHub repo, enable GitHub Pages
- [x] Create `data/registry.json` with all 8 current projects + config block
- [x] Create `data/last-refresh.json` and `data/changelog.md`
- [x] Copy existing dashboards into correct folder structure
- [x] Copy existing phase dashboards (OS Phases 1–6, Meetings Phase 1) into project folders
- [x] Create `_templates/` with update-log, learnings, README templates
- [x] Verify GitHub Pages is serving → https://elizabethlemoine33-commits.github.io/northlight-pmo/
- [x] Create `Northlight PMO Briefs/` folder in Google Drive
- [x] Create `#northlight-pmo` Slack channel — C0BELQQR6ET

**Session 2 — `/new-project` skill (Code)** ✅ Complete — July 3, 2026
- [x] Write the Claude Code skill file (reads config from registry.json)
- [x] Bootstrap all project folders (vault, website, skills-market, tableready, advisory-board) with README, update-log, learnings
- [ ] Test: run it end-to-end on a real new project
- [x] Backfill: all 8 projects represented in registry.json

**Session 3 — Updated PMO skill**
- [ ] Update existing PMO skill to write to `northlight-pmo/` instead of Desktop
- [ ] Add STANDING-PMO-TRIGGER Slack flag to its output
- [ ] Test: run for a dummy phase, verify GitHub commit + Slack flag

**Session 4 — Standing PMO dispatcher skill**
- [ ] Write `skill-standing-pmo-SPEC.md` in dispatcher specs
- [ ] Build the skill in the dispatcher
- [ ] Wire the STANDING-PMO-TRIGGER Slack listener
- [ ] Wire the Sunday 7 PM + Wednesday 8 AM schedule
- [ ] Wire manual trigger via `skill:standing-pmo` in Slack
- [ ] Test: run manually, verify all sources swept + portfolio updated + Slack summary

**Session 5 — `/new-project` skill (Chat)**
- [ ] Write the Chat-side skill (reads config from registry.json via Drive or hardcoded config)
- [ ] Test: parking-lot-only path → verify Slack post
- [ ] Test: full brief path → verify Drive file + Slack posts
- [ ] Test full handoff: Chat brief → Code registration

**Migration (during Session 1)**
- Copy files from `C:\Users\erand\Desktop\Northlight Standing PMO\` → GitHub repo
- Copy files from `C:\Users\erand\Desktop\Northlight Meeting\PMO\` → `northlight-pmo/meetings/`
- Desktop folders kept as backup — not actively maintained after migration

---

## Future: MCP Skill Version

When extracting this as a sellable skill, the work is:
1. Rename all Northlight-specific labels to generic ones in templates
2. Ship `registry.json` as a setup template with placeholder values
3. Write a setup wizard (the `/new-project` Chat skill becomes the onboarding flow)
4. Document required integrations: Slack (required), ClickUp (optional), GitHub (required), Drive (optional)
5. Package as `skill:standing-pmo` on MCP marketplace

The config-first design means this is a documentation + packaging exercise, not a rewrite.

### Brand skill integration

Customers can link their own brand skill so dashboards are generated in their visual identity. Config:

```json
"brand": {
  "skill": "northlight-brand-skill",
  "theme": "dark"
}
```

When Standing PMO generates or updates any dashboard HTML, it loads the linked brand skill first — pulls colour tokens, typography, logo path, gradient definition — and applies them via CSS variables. Customers without a brand skill get a clean neutral default theme.

**Design constraint:** all `_templates/` HTML must use CSS variable names only — never hardcoded hex values or font names. This is already true of the Northlight dashboards, so no rework needed. The brand skill populates the variables; the template structure stays the same regardless of who's using it.

This makes the visual output fully customisable with zero template changes — just swap the brand skill in config.

**Tiers for MCP listing:**
- Free / base: neutral default theme, no brand skill
- Paid add-on: brand skill integration (customer brings their own, or purchases one separately)
- Bundle opportunity: sell Standing PMO + a brand skill setup session together

---

*Built July 3, 2026 · Northlight Standing PMO · Decisions locked · Ready for Session 1*
