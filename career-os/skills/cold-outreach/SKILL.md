---
name: cold-outreach
description: "Generates personalised cold outreach messages — LinkedIn connection requests, InMails, and cold emails — that feel human and reference something real about the recipient or their company. Not spammy copy-paste. Designed to get replies. Trigger on: '/cold-outreach', 'write me a connection request', 'cold DM for this person', 'reach out to a recruiter', 'message this hiring manager', 'LinkedIn message for this job', or any request to write a professional outreach message to someone the candidate has not spoken to before."
---

# Cold Outreach Skill

## Overview

This skill generates personalised cold outreach messages for job searching —
LinkedIn connection requests, LinkedIn InMails, and cold emails to recruiters,
hiring managers, or warm contacts at target companies. Every message references
something real about the recipient so it never reads as copy-paste spam.

The master resume is already in this project. Never ask for it.

---

## Step 1 — Input Detection

### What's needed:
- **Recipient** — who is being contacted (recruiter, hiring manager, employee at target company, warm referral, etc.)
- **Purpose** — what the message is for (referral request, job inquiry, reconnecting, general networking, response to a job posting)
- **Channel** — LinkedIn connection request / LinkedIn InMail / cold email
- **Recipient context** — any details about the recipient: name, company, role, something from their LinkedIn profile, a post they made, a shared connection, a company announcement

### Input handling:
- **All context provided** → proceed immediately, generate message
- **Recipient name + company only** → use web search to find context about the company (recent news, product launches, growth signals, the team's work) to make the message feel researched
- **"/cold-outreach" with nothing** → ask:
  "Who are you reaching out to, and what's the goal?
  (e.g. 'Message to a recruiter at Shopify about a PM role' or 'Connection request to a Senior Engineer at a startup I want to join')"

### Channel character limits:
- **LinkedIn connection request** → 300 characters max — extremely tight, every word must earn its place
- **LinkedIn InMail** → 200 character subject + 1,900 character body
- **Cold email** → no hard limit, but aim for under 200 words — recruiters and hiring managers read on mobile

---

## Step 2 — Research the Recipient

If the recipient's LinkedIn URL, name, or company is provided, silently gather context:

**About the person:**
- Their current role and how long they've been there
- Any recent posts, articles, or comments they've made publicly
- Shared connections (if mentioned by the user)
- Career trajectory — are they a generalist or specialist?

**About the company:**
- Recent news: funding rounds, product launches, expansions, awards, press coverage
- The team's work: what the company is building, what problem they solve
- Any job postings that signal growth or a specific team need
- Company culture signals from their LinkedIn page or Glassdoor

Use web search for this when needed. A message that mentions "I saw you just raised your Series B" or "I read your post about building for emerging markets" gets opened. A generic message does not.

---

## Step 3 — Determine Message Type and Tone

### Recipient type → tone calibration:

**Recruiter at a company the candidate is targeting:**
- Direct, respectful of their time, specific about the role or team
- Lead with fit, not flattery — they hear "I love your company" 50 times a day
- Make it easy for them to say yes: one clear ask

**Hiring manager (direct):**
- More personal than a recruiter message
- Reference something specific about their work or their team
- Show you've done research — this signals the candidate would do the same on the job
- Softer ask — "would love your perspective" rather than "I'm applying"

**Employee at a target company (referral path):**
- Warmest tone — this is person-to-person, not candidate-to-recruiter
- Lead with the connection or common ground
- Be honest about the ask: "I'm considering applying and would love to hear your honest take on working there"
- Make the ask easy and low-commitment

**Warm contact / reconnecting:**
- Reference the shared history genuinely — don't pretend you're closer than you were
- Update them briefly on where you are now
- Make the ask feel like a natural continuation of a real relationship

---

## Step 4 — Write the Messages

Always generate **2 variants** — different angles, not just tone shifts.
The candidate picks the one that fits their personality.

---

### LinkedIn Connection Request (300 chars max)

**Rules:**
- Cannot include a link
- Must feel human in the first 3 words — never start with "I" or "Hi, I"
- Reference one specific thing about them or their company
- The ask must be implicit, not explicit — connection requests that ask for a job get ignored
- End with something that gives them a reason to accept

**Format:**
```
Option A (N chars):
[Message text]

Option B (N chars):
[Message text]
```

---

### LinkedIn InMail

**Subject line rules:**
- Under 200 characters
- Do NOT use: "Exciting opportunity", "Quick question", "Touching base"
- Reference something specific: their company, a recent event, a shared connection, or the role
- Make them curious — the subject line is all they see before deciding to open

**Body rules:**
- Para 1 (2 sentences): Why you're reaching out + the one specific thing you researched that makes this message non-generic
- Para 2 (2–3 sentences): Who you are — 1 achievement relevant to their world, not a life story
- Para 3 (1–2 sentences): The ask — specific, small, easy to say yes to
- Sign-off: First name only — formal sign-offs ("Best regards") read as templates

**Format:**
```
Option A:
Subject: [subject line]

[Body]

---

Option B:
Subject: [subject line]

[Body]
```

---

### Cold Email

**Subject line:** Same rules as InMail subject line — specific, curious, not generic

**Body structure:**
- **Line 1:** Reference — something specific about them or their company that shows you did real research
- **Line 2–3:** Brief, relevant credibility — one achievement or one sentence on your background that's directly relevant to their world. Pull from master resume.
- **Line 4:** The ask — one specific, easy thing: a 15-minute call, a reply with feedback, an introduction to the right person
- **P.S.:** Optional — one line that adds a human touch or makes the email memorable. Often the most-read line in a cold email.

**Format:**
```
Option A:
Subject: [subject line]

[Body]

---

Option B:
Subject: [subject line]

[Body]
```

---

## Step 5 — Output Block

After generating all message variants, output this block:

```
─────────────────────────────────────────
📨 Cold Outreach — [Recipient Name / Role] at [Company]
Channel: [LinkedIn Connection / InMail / Cold Email]
─────────────────────────────────────────

[All message variants as formatted above]

─────────────────────────────────────────
💡 Personalisation used:
[One sentence on the specific detail referenced in the messages and where it came from]

📋 Sending tips:
• Send connection requests between Tue–Thu, 9–11am local time — highest accept rates
• If no response to InMail in 5 days, follow up once with a 2-sentence nudge — not an apology
• Track replies in a simple spreadsheet: Name / Company / Sent / Reply / Outcome
• The best follow-up is a new reason: a company announcement, a shared article, a job posting

─────────────────────────────────────────
```

---

## Step 6 — Follow-up Messages (on request)

If the user asks for a follow-up message after no response, generate:

**Follow-up rules:**
- Maximum 3 follow-ups total — after that, move on
- Each follow-up must add something new — a reason to re-engage, not just "checking in"
- Follow-up 1 (5 days after): New angle or new context — a relevant company announcement, a shared article, a job posting at their company
- Follow-up 2 (10 days after): Graceful close — give them an easy out, signal you won't follow up again. This often gets a reply.
- Never: "Just circling back", "Following up on my previous message", "Touching base"

**Graceful close template:**
"[Name], I know your inbox is full. I'll leave it here — but if timing is ever right, I'd love to connect. [First name]"

---

## Output Rules

- **Generate 2 variants for every message type** — always give the candidate a choice
- **Every message must reference something specific** — no generic messages ever
- **Never ask for the resume** — it is in the project
- **Stay within character limits** — hard limit for connection requests, aim for brevity on InMail/email
- **Never fabricate context** — if no recipient details are provided, say what's missing and what would make the message stronger
- **Tone must match the channel** — connection requests are casual, emails can be slightly more formal, but never stiff
- **One ask per message** — messages with multiple asks get no response
- **Use web search** when a company name or person's LinkedIn is provided — research makes these messages significantly more effective
