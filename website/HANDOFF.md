# Northlight Website — Phase 6 Handoff
**Date:** 2026-08-09 · **Session:** 6 of Phase 6 · **13/18 tasks complete**

---

## What's live right now

- **bynorthlight.ca** — 51 pages on Jekyll / GitHub Pages / Railway
- **The Market-Aware COO** — published today at `/blog/the-market-aware-coo.html`
  - Spiral SVG diagram embedded inline
  - 12 inline academic citations (Kohli & Jaworski, Teece, Edmondson, Kashdan, et al.)
- **What Is a Fractional COO?** — published today at `/blog/what-is-fractional-coo.html`
  - 7 FAQs, answer-first format, discovery call CTA
- **GEO audit** — just ran, 82/100 overall (up from 65 pre-Phase 6)
- **LinkedIn** — 2 posts for Market-Aware COO + 2 for Fractional COO guide all queued in Buffer (personal profile)

---

## GEO Audit Scores — 2026-08-09

| Section | Score | Notes |
|---|---|---|
| Technical SEO | 100 | Perfect |
| Schema & Structured Data | 92 | sameAs at 7 links, 9/15 pts — room to grow |
| Crawler Access | 81 | All critical bots allowed; Apple/Amazon/ByteDance blocked (minor) |
| Content Citability | 74 | See gap below |
| Brand Authority | 60 | LinkedIn/GitHub/Crunchbase/Instagram confirmed; Reddit not detected |

**Content Citability breakdown:**
- Answer blocks: 170 total → 33/40 pts
- FAQ schema: 194 questions → 20/20 pts ✓
- Comparison tables: 5 → 15/15 pts ✓
- Numbered lists `<ol>`: 14 across 51 pages → **1/10 pts** ← biggest gap
- llms.txt bonus: +5 pts ✓

**Quick win for next session:** Convert bullet lists to `<ol>` in guide-format posts. 14 `<ol>` elements across 51 pages is way too sparse — targeting 3+ numbered steps per guide could recover ~7 pts with minimal effort.

---

## 5 Open Tasks (priority order)

1. **Write Topic Guide: OKRs for Canadian Small Businesses**
   - EOS source material already in knowledge repo (FRM-013-001 etc.) — no blocker
   - Long-form; pairs with the Fractional COO guide as cluster anchor

2. **Write comparison post: PM tools for small business operators**
   - Listicle format; high citation potential
   - No blocker

3. **Improve AI Score — SiteSpeak rerun**
   - Fixes already applied at 77/100: FAQ cancellation text added, support@bynorthlight.ca crawlable on coffee.html
   - Just needs a rerun to confirm the score moved

4. **Publish /resources.html — nav decision + 3 printables**
   - Page is drafted (9 cards, badge system) but unlinked/unpublished
   - Still needs: AI Implementation Partner RFP Template, AEO Audit Checklist, PIPEDA AI Compliance Checklist
   - Nav placement decision also needed before publish

5. **Research tasks (low priority)**
   - Review ScaleUpExec turnaround strategy post before writing on that topic
   - Coursera certifications in knowledge repo (EOS portion done; needs Elizabeth's cert list)

---

## Technical notes / gotchas

**GEO audit:**
- Limit is now 75 pages (was 30 — was silently dropping newest 21 posts). Both scripts updated: `geo_audit.py` + `dashboard/lib/geo-audit.js` in northlight-dispatcher
- Per-page timeout: 8s (avoids hanging on slow pages)
- Run from PowerShell: `python geo_audit.py` from northlight-dispatcher folder
- Or run from the OS dashboard (geo-audit.js covers the same logic)

**Blog index is hand-coded:**
- `blog/index.html` is NOT auto-generated — every new post needs a card added manually
- Both new posts (fractional-coo and market-aware-coo) are already in the index

**Push workflow:**
- Remote sometimes moves ahead before push → `git pull --rebase origin main` then push
- Happened once this session on the citation commit

**Knowledge repo:**
- Local: `C:\Users\erand\Documents\Northlight-Knowledge`
- ChatGPT builds locally; Claude pushes to GitHub
- Do NOT merge to main until ChatGPT signals done (branch `agent/knowledge-repo-cleanup` is complete and merged — main is current)

**Inline citations (The Market-Aware COO):**
- All 12 are inline markdown links in the body — not a reference list
- Decision: inline reads better in context; reference list would be buried after the FAQ

---

## File paths quick reference

| What | Path |
|---|---|
| Site root | `C:\Users\erand\Desktop\bynorthlight-site\` |
| Market-Aware COO post | `blog\the-market-aware-coo.md` |
| Fractional COO guide | `blog\what-is-fractional-coo.md` |
| Resources page (draft) | `resources.html` |
| Blog index (hand-coded) | `blog\index.html` |
| GEO audit Python | `C:\Users\erand\Desktop\northlight-dispatcher\geo_audit.py` |
| GEO audit JS | `C:\Users\erand\Desktop\northlight-dispatcher\dashboard\lib\geo-audit.js` |
| PMO dashboard | `C:\Users\erand\Desktop\northlight-pmo\website\phase-6.html` |
| Registry | `C:\Users\erand\Desktop\northlight-pmo\data\registry.json` |

---

## Where to pick up

Start with the OKRs topic guide — source material is ready in the knowledge repo and it's the highest-citation-potential piece still open. After that, numbered list additions across existing guide posts (quick GEO win). Resources page publish can happen once the 3 printables are ready.
