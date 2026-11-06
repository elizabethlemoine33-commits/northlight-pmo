# Northlight Connections — Learnings

*Newest at top. Append only — never edit past entries.*

---

## Phase 1 — Import and People, progress notes (2026-11-05)

- Merge a pull request only after the build session reports it final. PR 6 was merged before its last commit was pushed, so main lacked the change history and Help work until a second PR; the running app looked unchanged and it took a check of main to find why.
- Real data exposed problems the fake demo hid: very wide columns, 292 "awaiting me" threads back to 2013, and a page that jumped to the top after an action. Test on the real export early, with counts only.
- Keep "not now" (Declined) separate from "never" (Do not contact), and keep relationship (what someone is to you) separate from tags (what you want to do), or the labels blur.
- An app restart is needed to load new code; a browser refresh is not enough. Check the running app's menu or a known page before assuming an update worked.

---

## Phase 1 — Import and People (2026-11-05)

- The LinkedIn API cannot give a personal profile's followers by country or role, and has no messages or connection list. The data export is the only source for people-level data, so the tool is built on the export, not the API.
- A stage only makes sense inside a goal. The first demo put one stage on each person; Elizabeth's first look showed a person can be in several goals (a coffee chat, then "for work"), each with its own stages.
- About 4% of connections in the export are hidden (no name or profile link), and some export files have no header row, so the importer must never guess columns.
- Building on made-up export files from day one kept real people data out of the repo and out of the chat.

---
