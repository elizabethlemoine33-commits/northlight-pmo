# Northlight Vault — Learnings

*Newest at top. Append only — never edit past entries.*

---

## Phase 1 — Public Release, Verification & Search Filters (2026-11-04)
- Test with real accounts before writing the release claim. The changelog said search was name-only on all three providers; Elizabeth's OneDrive test showed Microsoft's search also matches file contents, and Microsoft offers no name-only option. We said so plainly in the changelog, privacy policy and docs instead of hiding it.
- Check a provider's search documentation before promising what it matches on. A quick read of the Graph search reference would have caught this before the changelog was written.
- A platform review is a waiting game, so record each one as its own open task with the date submitted. Say "submitted", not "verified", until the platform confirms.
- A request that finishes a promised feature (G2 wanted filters) is best answered with evidence people can open: release notes, a docs page and a redacted screenshot hosted on our own site, not a text block.
- Hosting a file and checking its public link in the same minute can leave a cached "not found" in front of it. Wait for the deploy, or test with a throwaway query string first.
