# Northlight Website — Learnings

*Newest at top. Append only — never edit past entries.*

---

## Phase 1 — Launch + AEO Foundation (2026-07-07)

- AEO requires entity name discipline from day one: "Northlight Advisory Services" (not just "Northlight") in every extractable sentence prevents AI models conflating the practice with a theatre company or software product. This is cheapest to enforce at build time.
- A standalone FAQ page kept out of the nav but present in sitemap + llms.txt is a low-cost AI-facing entity page — it answers questions AI models are likely to be asked without cluttering the human experience.
- llms.txt automation via GitHub Action is the right call for a flat-HTML site: no CMS hook, so the Action is the only reliable way to keep the blog list current without a manual step.

---
