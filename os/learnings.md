---
phase: Phase 9 — Marketing Round Table
completed: 2026-07-22
---

## Phase 9 Learnings (Sessions 1–4)

- **Personal knowledge bases should be fetched at session time, not baked into specs** — pulling from a public GitHub repo at Round Table session start means the knowledge base stays current without any rebuild cycle. The 30-min TTL cache makes it fast across debate rounds without hammering the network on every call.
- **Per-persona injection scope matters as much as the content itself** — Wren needs the full corpus (frameworks + observations) because she's drafting to a standard; Sage only needs the worldview and principles to verify authenticity; Devon only needs framework names for named-entity awareness. Over-injecting leads to blended voices; under-injecting leaves value on the table.
- **Topic-scoring frameworks to select relevant content is good enough for Harper** — a simple keyword-count score against the scenario text picks the right 1–2 frameworks for Harper's context without requiring a vector database. For a 4-framework corpus, it's exact enough.
- **Drive doc export (`files.export`) reads Google Docs without parsing complexity** — exporting as `text/plain` via the Drive API is simpler than downloading and parsing the binary format. The `readDriveDoc(fileId)` pattern is now reusable anywhere a Drive doc needs to be injected as context.
- **Scope and deferral decisions require alignment, not unilateral calls** — do not defer committed session items to the next phase without a conversation with Elizabeth first. Elizabeth is product manager; Claude is project executor. When scope is unclear or seems large, surface the tradeoff and ask — don't decide and announce.

## Phase 9 Learnings (Sessions 1–3)

- **Lazy-fetch beats eager-fetch for conditional UI** — fetching rejection comments only when the ⊛ button is clicked (not at kanban load) prevents N+1 requests on every pipeline render. Pattern: button visibility via cheap field already in the task object (`status`); expensive fetch deferred until user intent is confirmed.
- **`status === REJECTED` is better than `rejectionCount > 0` as a button gate** — rejectionCount requires a ClickUp comment fetch at kanban load time; status is already in the task data from the existing `/api/marketing/tasks` call. Same UX, zero extra requests.
- **HMR errors persist across page refreshes if the module failed mid-update** — a `[hmr] Failed to reload` error can stick in the console even after the underlying syntax issue is fixed. Force a full browser navigate (not just reload) to confirm whether a compile error is real or stale.
- **Optional catch binding `catch {}` (no variable) can silently break Babel transforms** — use `catch (e)` or `catch (err)` for safety even when the error isn't used. The ES2019 feature is in spec but not universally supported in all project Babel configs.
- **Fragment `<>...</>` wrapping a return with a conditional modal + a main card is the correct pattern** — `modalTaskContext && <Modal />` at the top of the fragment renders the overlay without nesting it inside the card div. State lives in the parent component (`MarketingKanban`), not in the card itself.

---

---
phase: Phase 8 — Security Hardening & System Observability
completed: 2026-07-18
---

## Phase 8 Learnings

- **Auth logging with IP/UA is cheap and immediately useful** — `appendAuthEvent()` adding login/logout/blocked to Drive on every auth event costs almost nothing and creates an audit trail that would have been opaque before. Ship it early, not as an afterthought.
- **429 retry needs `Retry-After` header respect, not a fixed backoff** — Slack's API sends the actual wait time; ignoring it and using a fixed 1s retry causes cascading failures. Pattern: read `Retry-After`, `retry_after`, or `x-rate-limit-reset` headers; fall back to exponential only when the header is absent.
- **Queue monitoring as a Drive-backed count is "good enough"** — reading the Drive JSON queue files to count pending items gives the dashboard a live number without a dedicated database. Works well for low-volume queues (< 100 items); revisit if queues grow.
- **Skeleton key should be scoped to the minimum surface area it unlocks** — the key was originally middleware-wide; scoping it to `/schedule-social` only reduces blast radius without changing functionality. Always scope security bypasses to the exact route that needs them.

---

---
phase: Phase 7 — Hardening + Navigation Rework
completed: 2026-07-15
---

## What Shipped

- **Block 1** — Fixed OAuth premature-close error: token fetch was returning an invalid response on session resume; resolved race condition in the auth middleware.
- **Block 2** — Railway blog auto-publish: scheduled July 10 publish ran successfully; confirmed via Railway logs.
- **Block 3** — Advisory Board retests: CMO Doctrine, CFO finance, CPO Doctrine, and session-expiry modal all re-tested and passing. GEO Analytics module shipped in Marketing tab. Advisory Board: X-ray contrarian pushback capability added; auto-generate markdown documentation from bot recommendations shipped.
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
