---
phase: Phase 7 — Hardening + Navigation Rework
completed: 2026-07-15
---

## What Shipped

- **Block 1** — Fixed OAuth premature-close error: token fetch was returning an invalid response on session resume; resolved race condition in the auth middleware.
- **Block 2** — Railway blog auto-publish: scheduled July 10 publish ran successfully; confirmed via Railway logs.
- **Block 3** — Advisory Board retests: CMO Doctrine, CFO finance, CPO Doctrine, and session-expiry modal all re-tested and passing.
- **Block 4** — OS Navigation Rework: collapsed 11 top-level tabs → 7 (`Home`, `Focus`, `Marketing`, `Finance`, `Compass`, `Capture`, `System`). Added sticky two-row nav (primary tabs + subtabs). Subtab state lifted to App.jsx and persisted via `sessionStorage`. MarketingPanel refactored to accept `subTab`/`onSubTabChange` props. Commit `4e010e9`.
- **Block 5** — GEO Audit History migrated from ephemeral JSON file to Google Sheets ("GEO Audit History" tab on Finance sheet). Resolved Railway container reset data loss.
- **Block 6** — Background job + polling pattern to replace SSE (Railway nginx proxy kills long-lived SSE connections).
- **Block 7** — npm audit: `form-data` CRLF injection CVE fixed; Slack `#finance` channel cleanup scoped (prerequisite for Sam ambient flag mode).

## Decisions

| Decision | Choice | Rationale |
|---|---|---|
| Where to store GEO audit history | Google Sheets (Finance sheet, new tab) | Railway filesystem is ephemeral — any redeploy wipes local JSON. Sheets persists across deploys and is already connected. |
| Long-running GEO audit delivery | Background job + polling (not SSE) | Railway nginx proxy has a hard timeout on SSE connections, causing premature stream close. Polling is simpler and Railway-compatible. |
| Subtab state location | Lifted to App.jsx | Subtab row must live inside the `sticky top-0 z-20` div (so it doesn't scroll away). Panel components can't render that zone — state had to be in App. |
| Python → Node.js for Railway | Port all scripts to Node.js | Railway's delta snapshot breaks Dockerfile multi-stage builds with Python; Node.js is the native runtime for the rest of the codebase. |
| Finance subtabs | None (flat layout) | Finance has 3 panels (Summary, Revenue, Expense) that all benefit from being visible together; subtabs would fragment information the user needs at a glance. |
| Work tab retirement | Distribute content to Home + Focus | Goals/Wins/Review belong on Home (daily context). Clients/Jobs/Learning belong in Focus (active workstreams). No content lost — reorganized. |

## Learnings

- **Railway ephemeral filesystem is a hard constraint** — any state that must survive a redeploy needs an external store (Sheets, Postgres, Redis). Don't write to the container filesystem for anything persistent.
- **Railway nginx proxy kills SSE** — long-lived streaming connections get terminated. The fix pattern is: start background job → return job ID → client polls `/status/:id` until done. Cleaner than SSE anyway.
- **Sticky subtabs require state at the App level** — if a panel component owns its own subtab state, the subtab row can't be in the sticky header. Lift it up or the second nav row scrolls away.
- **`hidden` attribute vs CSS `display:none`** — using the HTML `hidden` attribute for panel visibility keeps all panels mounted (good for `sessionStorage` and connection persistence) while hiding them from view. No routing needed.
- **MarketingPanel internal subtab bar was a blocker** — the component managed its own tab state, which made it impossible to lift the bar to App. Refactoring to props-in/callback-out unblocked sticky nav without changing the panel's rendering logic.
- **`sessionStorage` for tab + subtab persistence** — survives page refresh but not browser close. Right balance for an OS dashboard (restore context on accidental refresh, but start fresh each session).
