# Northlight Connections — Learnings

*Newest at top. Append only — never edit past entries.*

---

## Phase 2 — Privacy, Reminders and Connectors (2026-11-12)

- Claude desktop installed from the Microsoft Store redirects files that its child programs write under AppData and then shows them the private copy. A stray test copy of the data folder made the connector read a blank database and write its log where the app could not see it. Never run the app against the default data folder from a development session; the connector now detects a private copy and says so.
- Fake test names must differ in letters, not digits: matching ignores digits, so "Test01" and "Test02" looked identical and hid a matching problem.
- Real LinkedIn surnames carry credentials ("Surname, CPA"); matching on the last word missed obvious people until credentials were ignored.
- Append amendment sections to ISMS pages instead of rewriting them: ClickUp's page markdown can flatten tables. The one full-page edit (AIMS-004 version line) was checked line by line afterwards.
- When a connector says "off" while the app says "on", compare where each writes its log before suspecting the code.

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

- Giving each person an internal ID and making the LinkedIn address optional was a small change (a placeholder address, no table rebuild) that unlocked people met elsewhere; reviewing merges by hand is safer than auto-merging.
- A backup button and a desktop installer matter more than they look: the data is one file, and "no terminal" is what makes a tool feel finished.
- Reading the real brand (colours, fonts, the rule about white text on gradients) from the website source beat guessing; keeping every colour in one file made the restyle a single review.
- Fixed names for things the rules depend on (Closed, Declined, Won) must be protected in an editable library, or one rename silently breaks the logic.

---
