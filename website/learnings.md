# Northlight Website — Learnings

*Newest at top. Append only — never edit past entries.*

---

## Phase 2 — AEO Completion + Authority Building (2026-07-08)

- Prompt testing ChatGPT and Perplexity mid-phase is worth the 20 minutes: both AIs surfaced Northlight but couldn't place it in top lists because og:site_name said "Northlight" not "Northlight Advisory Services." A two-line meta tag fix is easier than months of outreach.
- When AI can't connect the dots between your services, that's a copy problem not an authority problem. The homepage hero was making users infer the unifying concept; making it explicit ("AI-Enabled Operations for Small Business") gave both AIs a citable H1 to pull from immediately.
- Halifax Chamber of Commerce and Digital Nova Scotia memberships ruled out at $300–400 fees — not justified at this stage. Local authority signals can be built through free directories and consistent NAP data instead.

## Phase 1 — Launch + AEO Foundation (2026-07-07)

- AEO requires entity name discipline from day one: "Northlight Advisory Services" (not just "Northlight") in every extractable sentence prevents AI models conflating the practice with a theatre company or software product. This is cheapest to enforce at build time.
- A standalone FAQ page kept out of the nav but present in sitemap + llms.txt is a low-cost AI-facing entity page — it answers questions AI models are likely to be asked without cluttering the human experience.
- llms.txt automation via GitHub Action is the right call for a flat-HTML site: no CMS hook, so the Action is the only reliable way to keep the blog list current without a manual step.

---
