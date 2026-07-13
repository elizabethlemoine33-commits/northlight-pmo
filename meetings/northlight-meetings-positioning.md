# Northlight Meetings — Positioning Document
*For internal use · CMO agent briefing · July 2026*

---

## What It Is

**Northlight Meetings** is a Windows desktop application that records client calls, transcribes them automatically, extracts structured notes and action items using AI, and files everything — to Google Drive, ClickUp, and Google Calendar — with one click.

It was built for solo consultants and small advisory practices that run on calls: discovery sessions, client check-ins, project reviews, strategy conversations. The tool handles everything that happens *after* the call ends so the consultant can stay present during it.

---

## The Problem It Solves

A typical consultant's post-call workflow looks like this:

1. Scramble to review handwritten or scattered notes
2. Write up a summary document (20–45 minutes)
3. Log action items to a task manager manually
4. Add follow-up dates to the calendar by hand
5. If it was a discovery call — spend 60–90 minutes drafting a proposal from scratch

Multiply that by 3–5 calls a week and you have a significant weekly overhead that generates no billable output. Existing transcription tools reduce step 1 but don't touch steps 2–5.

**Northlight Meetings eliminates steps 2–5 entirely.**

---

## How It Works

### Recording
The app captures **dual-channel audio** — the consultant's microphone and the system audio (what's coming through the speakers or headphones) simultaneously. This is done via VB-Audio Cable, a virtual audio router that routes system audio into a second recording channel. The result is a clean, full-conversation transcript — not just one side.

Most competitors capture mic audio only. Dual-channel capture is the difference between a transcript that says *"Yes, exactly — so what's your timeline?"* versus one that says *"So what's your timeline?"* — losing the context that made the question meaningful.

### Transcription
Audio is sent to **Groq Whisper** (whisper-large-v3), one of the fastest and most accurate transcription models available. A 45-minute call returns a full transcript in under 30 seconds. Cost: approximately $0.03 per call.

### Analysis
The transcript is passed to **Groq Llama 3.3 70B** for structured AI analysis. The model extracts:
- A structured meeting summary
- Action items (tagged by owner and due date)
- Key decisions made
- Dates and commitments mentioned
- If Proposal Mode is active: a complete client proposal draft

### Filing
With one click on the Review screen, the app:
- Creates a **Google Drive document** from a master template, filled with the meeting notes or proposal draft
- Creates **ClickUp tasks** for each action item
- Adds **Google Calendar events** for each date or deadline mentioned

No copy-paste. No tab-switching. No reformatting.

---

## Proposal Mode — The Key Differentiator

When a call is recorded in **Proposal Mode** (selected before the call begins), the AI analysis is tuned for discovery: it extracts the client's stated goals, pain points, constraints, budget signals, and timeline — and assembles them into a **formatted client proposal document** in Google Drive.

The output is a structured proposal the consultant can review, adjust, and send — not a raw transcript they have to write from.

A discovery call that used to cost 60–90 minutes of post-call writing now costs a 10-minute review.

No other consumer transcription tool offers this. The closest competitor behaviour is Gong's "deal intelligence" — but Gong is an enterprise sales platform at $1,000+/seat/year and requires a CRM integration. Northlight Meetings does it for ~$0.03 and files directly to Drive.

---

## Tech Stack (Plain English)

| Component | What It Does |
|---|---|
| Electron (Windows) | The desktop app shell — runs locally, no browser needed |
| VB-Audio Cable | Virtual audio router — captures system audio (the other person's voice) |
| Groq Whisper | Transcription — fast, accurate, ~$0.03/call |
| Groq Llama 3.3 70B | AI analysis — extracts notes, actions, proposals |
| Google Drive + Docs API | Creates and fills document templates automatically |
| ClickUp API | Creates tasks from extracted action items |
| Google Calendar API | Adds events from extracted dates |
| DPAPI (Windows) | Encrypts API keys locally — nothing stored in the cloud |

---

## Pricing Model

Northlight Meetings has **no subscription fee**. The only cost is API usage:

- Groq Whisper: ~$0.111/hour of audio
- Groq Llama 3.3 70B: ~$0.59/million tokens

**Real-world cost: approximately $0.03 per call.**

A consultant running 4 calls per week spends roughly **$6/year** on API costs. Compare to:
- Otter.ai Pro: $16.99/month ($204/year)
- Fireflies.ai Pro: $18/month ($216/year)
- Fathom: free tier limited; paid plans $19+/month
- Gong: $1,000+/seat/year

---

## Competitor Landscape

### Otter.ai
**What it does:** Real-time transcription, meeting summaries, action item extraction. Integrates with Zoom, Google Meet, Teams.
**Where it falls short:** Web-based; no system audio capture on Windows; no direct Drive or ClickUp filing; no proposal generation; subscription pricing.

### Fireflies.ai
**What it does:** Automatic meeting recording via a bot that joins calls, transcription, searchable library.
**Where it falls short:** Requires a bot to join the meeting (visible to all participants); cloud storage of all recordings; no proposal mode; no direct filing to task managers; subscription pricing.

### Fathom
**What it does:** Automatic summaries and highlights, free tier, Zoom-focused.
**Where it falls short:** Zoom-only; no ClickUp or Calendar integration; no proposal mode; no Windows system audio capture; cloud-dependent.

### tl;dv
**What it does:** Meeting recording and highlight clipping, GPT-powered summaries, CRM push.
**Where it falls short:** Browser extension required; cloud storage; no proposal mode; limited to Google Meet / Zoom / Teams.

### Gong / Chorus.ai
**What it does:** Enterprise call intelligence — sentiment analysis, deal risk scoring, coaching, CRM sync.
**Where it falls short:** Enterprise pricing ($1,000+/seat); requires CRM (Salesforce/HubSpot); not designed for solo practitioners or small firms; massive overhead for what a solo consultant needs.

### How Northlight Meetings Is Different

| | Northlight Meetings | Otter / Fireflies / Fathom |
|---|---|---|
| Dual-channel audio (both sides) | ✅ | ❌ (mic only or bot) |
| Proposal mode | ✅ | ❌ |
| Files to Drive + ClickUp + Calendar | ✅ | ❌ (partial at best) |
| Runs locally (Windows desktop) | ✅ | ❌ (browser/cloud) |
| No subscription fee | ✅ | ❌ |
| Cost per call | ~$0.03 | $1–2 equivalent |
| No bot joins the meeting | ✅ | ❌ (Fireflies uses bot) |
| Data stays local | ✅ | ❌ |

---

## Who It's For

**Primary:** Solo consultants and small advisory firms (1–5 people) who:
- Run their practice on calls (discovery, delivery, check-ins)
- Use Google Workspace + ClickUp for project management
- Write proposals manually today
- Are comfortable installing a Windows desktop app

**Secondary:** Freelancers and independent contractors who bill by the hour and want a record of what was discussed, agreed, and committed to on each call.

**Not for:** Teams that need multi-user access, mobile recording, real-time transcription visible to all participants, or CRM integration.

---

## Current Status

- ✅ Built and packaged (Windows installer)
- ✅ End-to-end pipeline confirmed: record → transcribe → analyse → Drive + ClickUp + Calendar
- ✅ Proposal mode live
- ✅ Privacy policy published (bynorthlight.ca/meetings-privacy.html)
- ⏳ GCP OAuth verification in progress (submitted July 5, 2026 — Google review takes a few weeks)
- ⏳ Distribution plan TBD — Gumroad storefront pending GCP clearance

---

## Possible Angles for Content / Positioning

1. **"The post-call tax"** — every call generates 45–90 minutes of admin that doesn't bill. Here's how to eliminate it.
2. **"Your meeting bot is lying to you"** — bot-based recorders miss half the conversation. Here's why dual-channel matters.
3. **"Discovery call to proposal in 10 minutes"** — walk through what Proposal Mode actually produces.
4. **"$0.03 vs $204/year"** — the cost of transcription tools is a subscription problem, not an API problem.
5. **"What actually happens to your call recordings"** — privacy angle; local processing, no cloud storage.
6. **The consultant's stack** — how Drive + ClickUp + Calendar integration removes the tab-switching tax.

---

*Built by Northlight · elizabethlemoine33@gmail.com · bynorthlight.ca*
