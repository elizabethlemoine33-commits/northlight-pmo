# Northlight Website — Learnings

*Newest at top. Append only — never edit past entries.*

---

## Phase 6 — Citation Authority & Content Production (2026-08-11)

- The `seo_title` front matter field cleanly separates title tag length from H1 length — when post titles are naturally long for readability, a shorter `seo_title` gives SEO control (60 char limit) without compromising the heading. Worth enforcing as a standard at publish time for any post whose title exceeds ~45 characters.
- Sunday morning (low post volume, reading mindset) + Monday morning (start-of-week planning mindset) are effective off-schedule slots for long-form evergreen promotion when the regular Tues/Wed/Thu content calendar is full — the promotion timing doesn't need to match the content calendar for non-time-sensitive pieces.
- Community ambassador / badge relationships with brands don't require FTC disclosure if there's no compensation, no free product, and no material benefit — a brief parenthetical in the post ("a community recognition badge, not a paid relationship") is transparent without triggering an obligation that doesn't exist.

## Phase 6 — Citation Authority & Content Production (2026-08-09)

- GEO score is a composite — Content Citability was the primary gap (59 at phase start), but the overall score was already at 65 before Phase 6 work. Publishing two FAQ-dense topic guides and adding 12 inline academic citations to the Market-Aware COO post moved overall from 65 to 82 in 3 days. The lever is content volume and structure, not technical tweaks.
- Numbered lists (`<ol>`) are a disproportionately high-leverage GEO signal: 14 `<ol>` elements across 51 pages scores 1/10 — the easiest +7-point gain available for Phase 7 is converting bullet lists to numbered steps in guide-format posts.
- GEO audit coverage must be checked against sitemap size before every run: the 30-page default was silently dropping 21 pages, making every audit result since launch misleading. Set the limit above the current page count and add headroom for growth.

## Phase 5 — Jekyll Migration & SEO/GEO/AEO Foundation (2026-08-07)

- Two-pass citation resolution is more efficient than fixing inline during migration — log issues as "yellow" during the pass, resolve in a dedicated second pass once all posts are migrated. 14 citations resolved without slowing migration speed.
- E-E-A-T signals (Article JSON-LD + one strong external citation) are the highest-ROI single fix for essay-format posts — moves P2 scores 5–10 points and takes under 10 minutes per post. Worth enforcing at publish time, not retroactively.
- Privacy-first brand consistency must extend to interactive tools, not just content — an ungated tool (print/save, no email) is the only approach consistent with a PIPEDA-first stance. Gating the fractional readiness check would have created a visible contradiction with the blog content.

## Phase 4 — Content Depth & Thought Leadership (2026-07-24)

- Publishing a batch of posts on the same day (July 20–21) made the related-reading cross-link pass far more efficient — touching all 21 posts in one sweep is easier than retroactively adding links each time a new post ships. Ship in batches, link in batches.
- Tiered CTAs on the About page outperform a single ask: "Grab a virtual coffee" (free, low-commitment) + EveryExpert $99 session gives visitors two entry points at different trust levels rather than forcing a binary choice.
- Social promotion lagged badly behind content production — 11 Phase 4 posts shipped with zero social queue set up. Schedule the social queue immediately after each publish batch, not as a separate "later" task.

## Phase 2 — HubSpot AEO + Prompt Testing (2026-07-09)

- Gemini entity collision is real and systematic: Gemini conflated Northlight Advisory Services (Halifax, Elizabeth Lemoine, strategy/ops/AI) with Northlight Solutions Group (NSG, a Salesforce/Agentforce partner) across 100% of tested prompts, inventing a fictional methodology and training program. The fix is content-based — a dense entity-signal page + explicit llms.txt disambiguation naming NSG by name.
- A robots.txt "HIGH" warning from an AEO tool doesn't mean AI crawlers are blocked — the tool can misread Cloudflare-managed sections. Always verify at the crawl-control dashboard directly, not by reading the raw robots.txt file through a summarizer.
- HubSpot AEO tool's Chatbeat citation landscape data is more actionable than its Recommendations tab: seeing *which sites* AI cites for your target prompts (capstacker.io, chiefjobs.com, gofractional.com) immediately shows you the format gap to close. Build a listicle; don't guess.

## Phase 2 — AEO Completion + Authority Building (2026-07-08)

- Prompt testing ChatGPT and Perplexity mid-phase is worth the 20 minutes: both AIs surfaced Northlight but couldn't place it in top lists because og:site_name said "Northlight" not "Northlight Advisory Services." A two-line meta tag fix is easier than months of outreach.
- When AI can't connect the dots between your services, that's a copy problem not an authority problem. The homepage hero was making users infer the unifying concept; making it explicit ("AI-Enabled Operations for Small Business") gave both AIs a citable H1 to pull from immediately.
- Halifax Chamber of Commerce and Digital Nova Scotia memberships ruled out at $300–400 fees — not justified at this stage. Local authority signals can be built through free directories and consistent NAP data instead.

## Phase 1 — Launch + AEO Foundation (2026-07-07)

- AEO requires entity name discipline from day one: "Northlight Advisory Services" (not just "Northlight") in every extractable sentence prevents AI models conflating the practice with a theatre company or software product. This is cheapest to enforce at build time.
- A standalone FAQ page kept out of the nav but present in sitemap + llms.txt is a low-cost AI-facing entity page — it answers questions AI models are likely to be asked without cluttering the human experience.
- llms.txt automation via GitHub Action is the right call for a flat-HTML site: no CMS hook, so the Action is the only reliable way to keep the blog list current without a manual step.

---
