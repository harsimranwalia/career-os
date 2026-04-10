---
name: job-match
description: "Full job evaluation skill. When a job posting (text, URL, or table row) is provided, delivers 6 structured blocks: role summary, profile match with gap analysis, level & positioning strategy, comp & market demand, personalisation plan for resume and LinkedIn, and interview story bank. Outputs a match score. Trigger on: '/job-match', 'evaluate this job', 'how do I match', 'score this role', 'is this job right for me', or when a JD is shared and the user wants a full assessment before applying."
---

# Job Match Evaluator

## Overview

When a job posting is provided, this skill delivers a complete A–F evaluation
across 6 blocks, ending with a match score and a clear go/no-go recommendation.

The master resume and LinkedIn profile are already uploaded in this project.
Never ask for them. Pull all candidate data from those files.

---

## Step 0 — Input & Archetype Detection

### Input handling
- **JD pasted as text** → proceed immediately
- **URL provided** → use web search to fetch the JD content, then proceed
- **Table row from /linkedin-jobs** → extract Job Title, Company, Location,
  Description, Work Type, Employment Type from that row
- **/job-match with nothing** → ask: "Paste the job posting or share the URL
  and I'll run a full evaluation."

### Role Archetype Classification

Classify the role into ONE primary archetype (or flag as hybrid of two):

| Archetype | Key Signals |
|-----------|-------------|
| **Builder** | "build", "ship", "engineer", "develop", "hands-on", "0 to 1", "IC" |
| **Consultant / Client-Facing** | "client", "stakeholder", "advise", "solutions", "pre-sales", "partner" |
| **Product** | "discovery", "roadmap", "prioritisation", "OKRs", "user research", "PM" |
| **Operator / Scale** | "process", "efficiency", "systems", "cross-functional", "ops", "scale" |
| **Manager / Lead** | "team", "direct reports", "hiring", "performance", "P&L", "director" |
| **Specialist / Expert** | Deep domain: AI/ML, data, legal, finance, design, security, compliance |

The archetype determines:
- Which proof points to surface in Block B
- How to position the candidate's founder experience in Block C
- Which interview stories to prioritise in Block F

---

## Block A — Role Snapshot

Output a clean summary table, then a one-line TL;DR.

```
─────────────────────────────────────────
BLOCK A · Role Snapshot
─────────────────────────────────────────
```

| Field | Value |
|-------|-------|
| Job Title | [exact title] |
| Company | [company name] |
| Archetype | [detected — e.g. "Builder (AI-focused)"] |
| Domain | [industry / tech area] |
| Function | [build / consult / manage / deploy / advise] |
| Seniority | [junior / mid / senior / staff / lead / director / VP] |
| Location | [city, country] |
| Work Type | [Remote / Hybrid / On-site] |
| Employment | [Full-time / Part-time / Contract] |
| Salary | [if listed — range or "Not disclosed"] |
| Team Size | [if mentioned — otherwise "Not mentioned"] |

**TL;DR:** [One sentence — what this role is fundamentally about and who it's for]

---

## Block B — Profile Match

```
─────────────────────────────────────────
BLOCK B · Profile Match
─────────────────────────────────────────
```

Read the master resume in this project. Map every requirement from the JD
to specific evidence from the candidate's background.

**Requirements mapping table:**

| JD Requirement | Match Level | Evidence from Resume |
|----------------|-------------|----------------------|
| [Requirement 1] | ✅ Strong / ⚠️ Partial / ❌ Gap | [Exact role, achievement, or skill from resume] |
| ... | ... | ... |

**Match levels:**
- ✅ **Strong** — direct, provable match with specific evidence
- ⚠️ **Partial** — adjacent experience or transferable skill, not exact
- ❌ **Gap** — no evidence in the resume; honest absence

**Archetype-adapted proof point priorities:**
- **Builder** → delivery speed, shipped products, technical depth, scale achieved
- **Consultant** → client outcomes, stakeholder management, proposal/deal wins
- **Product** → discovery process, metrics improved, roadmap decisions, user research
- **Operator** → efficiency gains, systems built, cross-functional influence
- **Manager** → team size, hiring, performance management, culture building
- **Specialist** → depth of domain expertise, certifications, published work

---

### Gap Analysis

For each ❌ Gap and ⚠️ Partial match, provide:

| Gap | Hard Blocker? | Adjacent Experience? | Mitigation Plan |
|-----|--------------|----------------------|-----------------|
| [Gap] | Yes/No | [What adjacent experience exists] | [Concrete action: reframe, project, course, cover letter angle] |

**Mitigation options to consider:**
- Reframe existing experience using the JD's language
- Surface a side project or consulting work that covers the gap
- Address it directly in the cover letter as "actively building in this area"
- Propose a portfolio project that could close the gap within 2–4 weeks
- For hard blockers: flag clearly — do not paper over them

---

## Block C — Level & Positioning Strategy

```
─────────────────────────────────────────
BLOCK C · Level & Positioning Strategy
─────────────────────────────────────────
```

### 1. Seniority assessment

**JD expects:** [seniority level detected from JD]
**Candidate's natural fit for this archetype:** [honest assessment from resume]
**Delta:** [Exact match / Slightly above / Slightly below / Significant gap]

### 2. "Sell senior without lying" plan

Specific framing strategies for this role and archetype:

- **Founder positioning:** [How to frame the founder/operator experience as an
  advantage for this specific role — e.g. "At AIOrders you owned the full
  product cycle — frame that as 0→1 product leadership, not 'I ran my own thing'"]
- **Consulting positioning:** [How fractional/consulting work maps to this JD's
  requirements — specific language to use]
- **Top 3 achievements to lead with:** [From the resume, which 3 achievements
  make the strongest case for this level — with suggested framing for each]
- **Language to use:** [3–5 specific phrases from the JD to mirror in the resume
  summary and cover letter — exact words, not paraphrases]

### 3. "If they down-level me" plan

If the company offers a level below what was applied for:
- Accept if: [compensation criteria — what makes it worth accepting]
- Negotiate: structured 6-month review with explicit promotion criteria
- Counter with: [specific language for the negotiation conversation]
- Walk away if: [what would make this a no-go even at reduced level]

---

## Block D — Compensation & Market Demand

```
─────────────────────────────────────────
BLOCK D · Comp & Market Demand
─────────────────────────────────────────
```

Use web search to pull current data. Do not invent figures — if data is unavailable,
say so explicitly.

Search for:
- Current salary range for this role + seniority + location (Glassdoor, Levels.fyi,
  LinkedIn Salary, Indeed, Blind)
- Company's compensation reputation (generous / average / below market)
- Demand trend for this role type (growing / stable / shrinking)
- Any recent layoffs, funding rounds, or financial news about the company

| Data Point | Finding | Source |
|------------|---------|--------|
| Market salary range | $[X] – $[Y] | [Source] |
| Company comp reputation | [generous / average / below market] | [Source] |
| Role demand trend | [growing / stable / shrinking] | [Source] |
| Company financial health | [notes] | [Source] |
| Role listed salary | [if disclosed, or "Not disclosed"] | JD |

**Negotiation anchor:**
[Based on market data and the candidate's profile, what is a reasonable target
number and range to negotiate from? Give a specific dollar figure, not a range.]

---

## Block E — Personalisation Plan

```
─────────────────────────────────────────
BLOCK E · Personalisation Plan
─────────────────────────────────────────
```

### Top 5 Resume Changes

| # | Section | Current State | Proposed Change | Why |
|---|---------|---------------|-----------------|-----|
| 1 | [e.g. Summary] | [what it currently says or implies] | [specific proposed rewrite or addition] | [maps to JD requirement X] |
| 2 | | | | |
| 3 | | | | |
| 4 | | | | |
| 5 | | | | |

### Top 5 LinkedIn Changes

| # | Section | Current State | Proposed Change | Why |
|---|---------|---------------|-----------------|-----|
| 1 | [e.g. Headline] | [current] | [proposed] | [reason] |
| 2 | | | | |
| 3 | | | | |
| 4 | | | | |
| 5 | | | | |

### ATS Keywords to Inject

List 15–20 keywords extracted from the JD that an ATS would scan for.
Group by section they should appear in:

**Summary:** [keywords]
**Experience:** [keywords]
**Skills:** [keywords]

These are the exact terms to use when running `/resume` for this job.

---

## Block F — Interview Story Bank

```
─────────────────────────────────────────
BLOCK F · Interview Story Bank
─────────────────────────────────────────
```

Generate 6–8 STAR+R stories mapped to the JD's key requirements.
Pull all stories from actual experience in the master resume — no fabrication.

**STAR+R format:**
- **S** — Situation: context, stakes, environment
- **T** — Task: your specific ownership
- **A** — Action: what YOU did (not the team)
- **R** — Result: quantified outcome where available
- **Reflection** — what you learned or would do differently
  *(This is the seniority signal — senior candidates extract lessons)*

| # | JD Requirement | Story Title | S | T | A | R | Reflection |
|---|---------------|-------------|---|---|---|---|------------|
| 1 | [requirement] | [short title] | [context] | [ownership] | [actions] | [outcome] | [lesson] |
| 2 | | | | | | | |
| ... | | | | | | | |

**Archetype story emphasis:**
- **Builder** → speed of delivery, technical decisions, shipping under constraints
- **Consultant** → client outcomes, difficult stakeholders, scoping under ambiguity
- **Product** → discovery insights, prioritisation trade-offs, metrics moved
- **Operator** → systems built, efficiency created, cross-functional influence
- **Manager** → hiring decisions, performance conversations, team culture
- **Specialist** → depth of expertise demonstrated, novel problem solved

### Recommended Case Study

Which of the candidate's projects or roles should be prepared as a full case study
for this role, and how to structure it:

**Project:** [name from resume]
**Why this one:** [why it maps best to this role]
**How to structure it:** [3–4 sentences on how to present it — what to lead with,
what metrics to emphasise, how to tie it to the JD's core problem]

### Red-Flag Questions & How to Answer Them

Questions the interviewer may ask that could be uncomfortable, and how to handle them:

| Question | Why It's Asked | How to Answer It |
|----------|---------------|-----------------|
| "Why did you leave / sell your company?" | Assessing risk, commitment, ambiguity tolerance | [Specific framing using candidate's actual situation] |
| "You've been independent for X years — can you work within a structure?" | Culture fit, coachability | [How to reframe operator independence as an asset] |
| "You don't have [specific gap] — how would you handle that?" | Testing self-awareness and learning agility | [Specific bridge using adjacent experience] |
| [Other role-specific red flags based on the JD and resume gaps] | | |

---

## Match Score

```
─────────────────────────────────────────
MATCH SCORE
─────────────────────────────────────────
```

| Category | Score | Rationale |
|----------|-------|-----------|
| Skills & Tools Match | /20 | |
| Experience Level Fit | /20 | |
| Domain / Industry Relevance | /15 | |
| ATS Keyword Coverage | /15 | |
| Compensation Alignment | /10 | |
| Culture & Work Type Fit | /10 | |
| Growth Potential | /10 | |

**Total: [X] / 100**

**Interpretation:**
- 85–100 → 🟢 Strong fit. Apply immediately. Prioritise this one.
- 70–84 → 🟡 Good fit with gaps. Apply — and address gaps in cover letter.
- 55–69 → 🟠 Stretch role. Apply if you're targeting growth, not comfort.
- Below 55 → 🔴 Significant mismatch. Consider whether to invest the time.

**Recommendation:**
[One honest paragraph — should they apply, and if so, what's the strategy?
 If not, what type of role would be a better fit right now?]

---

## After the Evaluation

End with this menu:

```
─────────────────────────────────────────
Next Steps:
─────────────────────────────────────────
→ /resume          — Inject the ATS keywords and generate a tailored .docx resume
→ /cover           — Write the matched cover letter (uses this evaluation as context)
→ /interview-prep  — Generate interview questions and STAR frameworks for this role
→ /mock-interview  — Run a live mock interview for this specific role
→ /all             — Run all four back-to-back (resume → cover → interview-prep, then offer /mock-interview)
─────────────────────────────────────────
```

---

## Output Rules

- **Never fabricate salary data** — if Levels.fyi or Glassdoor data isn't found,
  say "No reliable data found for this exact role/location — negotiate anchored
  to [adjacent role] data instead."
- **Never paper over gaps** — the gap analysis must be honest. A match skill that
  inflates scores is useless. The candidate needs truth to decide whether to apply.
- **Never ask for the resume** — it is in the project
- **All story content must come from the actual resume** — the STAR stories are
  frameworks drawn from real experience, not invented narratives
- **Archetype must drive everything** — question weighting, proof point selection,
  positioning language, and story emphasis all flow from the archetype detected
  in Step 0
- **Be specific** — every recommendation should reference actual lines from the
  resume or actual language from the JD
