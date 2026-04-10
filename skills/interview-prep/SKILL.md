---
name: interview-prep
description: "Generates role-specific interview preparation based on a job description and the master resume in the project. Produces: top 10 interview questions labelled by type, 3 STAR+R answer frameworks built from actual resume experience, and 5 smart questions to ask the interviewer. Trigger on: 'prep me for this interview', 'interview questions for this role', 'help me prepare', '/interview-prep', or when a JD is shared with intent to prepare for an interview."
---

# Interview Prep Skill

## Overview

This skill generates a complete, role-specific interview preparation kit using the
master resume and LinkedIn profile already uploaded in this project. It never asks
for the resume. It reads the JD, maps it to the candidate's actual experience, and
produces practise-ready outputs — not generic advice.

---

## Step 1 — Input Detection

Check what has been provided:
- **Full JD pasted** → proceed immediately
- **Job title + company only** → ask: "Can you paste the job description? Even a
  partial one helps me target the questions accurately."
- **Table row from /linkedin-jobs** → extract Job Title, Company, Description from
  that row and treat as the JD input
- **"/interview-prep" with no job** → ask: "Which role are you prepping for? Paste the JD
  and I'll build your full prep kit."

---

## Step 2 — Analyse the Role

Silently extract before generating any output:

**Role signals:**
- Seniority level (junior / mid / senior / lead / director)
- Primary function: builder / consultant / manager / individual contributor
- Top 5 responsibilities by emphasis in the JD
- Hard skills required (tools, platforms, languages, certifications)
- Soft skills emphasised (leadership, communication, cross-functional, etc.)
- Industry / domain context

**Archetype classification** — classify the role into one of these (or hybrid of two):

| Archetype | Signals |
|-----------|---------|
| **Builder** | "build", "ship", "engineer", "develop", "hands-on", "0 to 1" |
| **Consultant / Advisor** | "client-facing", "stakeholder", "advise", "solutions", "pre-sales" |
| **Product** | "discovery", "roadmap", "prioritisation", "metrics", "user research" |
| **Operator / Ops** | "process", "scale", "efficiency", "systems", "cross-functional" |
| **Manager / Lead** | "team", "reports", "hiring", "performance", "strategic direction" |
| **Specialist / Expert** | deep domain: legal, finance, data, security, design, compliance |

Store the archetype — it determines which question types to emphasise and how to
frame STAR stories.

**Candidate match:**
From the master resume in this project, identify:
- Most relevant role(s) for this position
- 3–5 concrete achievements that map to the top JD responsibilities
- Any tools, certifications, or domain experience that directly matches
- Visible gaps (be honest — they will come up in the interview)

---

## Step 3 — Generate the Prep Kit

Output in this exact order with these exact headers.

---

### 🎯 Interview Prep — [Job Title] at [Company]
**Archetype:** [detected] | **Seniority:** [detected] | **Focus:** [top theme from JD]

---

### Part 1 — Top 10 Interview Questions

Generate 10 questions the interviewer is most likely to ask, based on the JD.
Label every question with its type. Prioritise question types based on archetype:

- **Builder** → weight toward Technical + Situational
- **Consultant** → weight toward Behavioural + Situational
- **Product** → weight toward Behavioural + Case
- **Operator** → weight toward Situational + Behavioural
- **Manager** → weight toward Leadership + Behavioural
- **Specialist** → weight toward Technical + Behavioural

**Question type labels:**
- `[Behavioural]` — "Tell me about a time when..."
- `[Technical]` — role-specific knowledge or skill test
- `[Situational]` — "What would you do if..."
- `[Leadership]` — managing people, conflict, direction
- `[Culture Fit]` — values, ways of working, motivation
- `[Case]` — problem-solving, trade-off analysis

Format:

```
1. [Type] Question text here?
2. [Type] Question text here?
...
```

Do not add answer guidance here — answers come in Part 2.

---

### Part 2 — STAR+R Answer Frameworks

Pick the 3 most likely behavioural or situational questions from Part 1.
For each, build a STAR+R skeleton using actual experience from the master resume.

**STAR+R format:**
- **S — Situation:** Set the context — what was the environment, team size, stage?
- **T — Task:** What were you specifically responsible for?
- **A — Action:** What did YOU do? (Not the team — use "I", not "we")
- **R — Result:** Quantified outcome where it exists in the resume. Use placeholders
  like `[X%]` or `[N users]` if the number exists in the resume but needs confirming.
- **Reflection:** What did this teach you? What would you do differently?
  This is what separates senior candidates from junior ones.

**Why STAR+R matters:** Junior candidates describe what happened.
Senior candidates extract lessons. The Reflection column is your seniority signal.

Format each story as:

```
─────────────────────────────────────────
Q: "[Question text]"

S: [2 sentences — context and stakes]
T: [1 sentence — your specific ownership]
A: [3–4 sentences — concrete actions, decisions, trade-offs you made]
R: [1–2 sentences — outcome with numbers if available]
★ Reflection: [What you learned or would do differently — 1–2 sentences]

💡 Tip: [One coaching note specific to this question and this role]
─────────────────────────────────────────
```

---

### Part 3 — Questions to Ask Them

5 smart questions the candidate should ask the interviewer. Rules:
- Specific to this JD and company — not generic
- Each signals a different dimension: strategic thinking, team dynamics,
  growth path, success metrics, company challenges
- None should be answerable by reading the job posting
- Avoid questions about salary/benefits at first interview unless it's a recruiter screen

Format:

```
1. [Question] — 💡 Why ask this: [one sentence on what it signals about you]
2. ...
```

---

### ⚡ Quick Prep Checklist

Output this block at the end:

```
Before the interview:
□ Research [Company Name] — recent news, funding, product launches
□ Review your STAR stories above out loud (not just in your head)
□ Prepare your "tell me about yourself" — 90 seconds, 3 acts: past / present / future
□ Know your numbers — be ready to quantify any achievement they probe on
□ Prepare for: "Why [Company]?" and "Why this role?" — make it specific

On the day:
□ Have the JD open for reference
□ Have your resume open — interviewers often read from it line by line
□ Take 3 seconds before answering behavioural questions — it signals confidence

After:
□ Send a follow-up note within 24 hours — one specific thing from the conversation
□ Run /mock if you want to practise live before the real interview
```

---

## Output Rules

- Never output generic interview advice — every question and story must be
  specific to this JD and this candidate's actual resume
- Never fabricate achievements — only use experience from the master resume
- Never ask for the resume — it is in the project
- The STAR+R stories are frameworks, not scripts — tell the candidate to
  personalise the wording in their own voice
- Do not output a file — this skill outputs directly in chat for easy reading
  and quick reference during practise sessions

---

## After Output

End with:

```
---
Ready to practise live? Run /mock-interview and I'll be your interviewer for [Job Title] at [Company].
Want the cover letter too? Run /cover with the same JD.
```
