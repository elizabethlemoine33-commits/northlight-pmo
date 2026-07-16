# Northlight PMO — Claude Standing Instructions

## ClickUp write-back

After completing any task during a session, immediately mark it complete in ClickUp and append one line to that project's update-log.

When a session plan is finalized and approved, push each task to ClickUp before starting work — do not wait until after completion.

This applies to both:
- Tasks pulled from ClickUp → write completions back when done
- Tasks generated from a session plan → push to ClickUp at plan approval, before work starts

## Update-log format

Append to `C:\Users\erand\Desktop\northlight-pmo\[slug]\update-log.md` (newest first):

```
| YYYY-MM-DD | [short description of what was done] | [session context or phase] |
```

## Registry

Project list IDs and ClickUp folder IDs are in `C:\Users\erand\Desktop\northlight-pmo\data\registry.json`. Read it before asking Elizabeth which list to use.
