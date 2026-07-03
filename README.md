# Northlight PMO

Cross-project portfolio tracking for all Northlight projects.

**Live dashboard:** https://elizabethlemoine33-commits.github.io/northlight-pmo/

## Projects tracked
- Northlight OS (`os/`)
- Northlight Meetings (`meetings/`)
- Northlight Vault (`vault/`)
- Northlight Website (`website/`)
- Northlight Skills Market (`skills-market/`)
- TableReady (`tableready/`)
- Advisory Board Bot System (`advisory-board/`)
- Standing PMO System (`standing-pmo/`)

## Structure
- `index.html` — Portfolio dashboard (all projects)
- `data/registry.json` — Project registry + system config
- `data/changelog.md` — Standing PMO run history
- `[project]/` — Phase dashboards per project
- `_templates/` — Templates for new projects and phases

## Standing PMO
Runs automatically: Sunday 7 PM + Wednesday 8 AM Atlantic.  
Manual trigger: post `skill:standing-pmo` in #northlight-pmo.
