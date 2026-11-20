# Finance Tab — Learnings

*Newest at top. Append only — never edit past entries.*

---

## Phase 1 — Foundation, Ledger and receipts (2026-11-20)

- The receipt column of every auto-logged expense was empty: the approve flow archived the receipt to Drive but never wrote its link to the row, so the Ledger showed every receipt as missing. A screen that reads a field exposes whether anything ever wrote it.
- The Expense Log keeps only the CAD amount. The original foreign amount and the rate are on the card statement, so they are best taken at reconciliation, not from receipts.
- Google Sheets allows about 60 writes a minute; the first apply stopped at 16 of 37 items until writes were batched.
- Saved monthly reports go stale whenever an adjustment is applied after they were made; four of the saved reports differed from the log by exactly the cleanup entries. The Reports tab now flags this and Apply offers to regenerate.
- Pushing to a branch after its pull request has merged does not ship the change; check the PR state before pushing and open a new PR if it has merged.
- Two Claude sessions shared one working folder and a hard reset in one wiped the other's uncommitted edit. Use a separate git worktree for build work.

---
