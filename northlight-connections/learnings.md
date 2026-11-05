# Northlight Connections — Learnings

*Newest at top. Append only — never edit past entries.*

---

## Phase 1 — Import and People (2026-11-05)

- The LinkedIn API cannot give a personal profile's followers by country or role, and has no messages or connection list. The data export is the only source for people-level data, so the tool is built on the export, not the API.
- A stage only makes sense inside a goal. The first demo put one stage on each person; Elizabeth's first look showed a person can be in several goals (a coffee chat, then "for work"), each with its own stages.
- About 4% of connections in the export are hidden (no name or profile link), and some export files have no header row, so the importer must never guess columns.
- Building on made-up export files from day one kept real people data out of the repo and out of the chat.

---
