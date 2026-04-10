---
name: job-scout
description: "Searches Indeed and Google Jobs for roles matching a job title or the candidate's resume profile. Presents results in a ranked table sorted by /job-match score (highest first). Requires web search permission. Trigger on: '/job-scout', 'find me jobs', 'search for jobs', 'what jobs match my resume', 'find jobs for me on Indeed', 'search Google Jobs for X role', or any request to discover and rank job opportunities."
---

# Job Scout

## Overview

This skill searches Indeed and Google Jobs for live job listings, runs the
/job-match evaluation on each result, and outputs a ranked table sorted by
match score — highest first. The master resume and LinkedIn profile already
in this project are used as the candidate profile. Never ask for the resume.

Requires: **web search enabled** in Claude settings.

---

## Step 1 — Input Detection

### Mode A — Role-based search
Triggered when the user specifies a job title or role:
- "/job-scout Product Manager remote Canada"
- "find me AI Product Manager jobs in Vancouver"
- "search for fractional CTO roles"

Extract:
- `ROLE` — job title to search for
- `LOCATION` — city, country, or "remote" (default: "remote" if not specified)
- `COUNT` — number of results to find (default: 10, max: 15)

### Mode B — Resume-based search
Triggered when the user says "match my resume", "based on my profile",
or gives no specific role:
- "/job-scout"
- "find jobs that match my resume"
- "what roles should I be applying for?"

Silently read the master resume in this project. Extract:
- Most recent job title → use as primary `ROLE`
- Top 3 skills/tools → append to search queries for better targeting
- Seniority level → filter results accordingly
- Location preference → infer from resume location if not specified

Announce before searching:
```
Reading your resume to identify the best-matching roles...
Searching as: [ROLE] | [LOCATION] | Seniority: [detected]
```

### Clarification (only if truly ambiguous)
If the user gives neither a role nor enough resume context to infer one,
ask a single question:
"What type of role are you looking for? (e.g. 'Product Manager',
'AI Engineer', 'Marketing Director') — or say 'use my resume' and
I'll infer from your profile."

---

## Step 2 — Search for Jobs

### Search strategy

Run web searches across both sources. Use varied queries to maximise
result diversity — do not repeat the same query twice.

**Indeed searches (run 2–3 queries):**
```
site:indeed.com "[ROLE]" [LOCATION] jobs
indeed.com "[ROLE]" [LOCATION] hiring [current year]
indeed.com "[ROLE]" [LOCATION] full-time OR remote
```

**Google Jobs searches (run 2–3 queries):**
```
"[ROLE]" jobs [LOCATION] site:google.com/about/jobs OR jobs listing
[ROLE] [LOCATION] job opening [current year]
[ROLE] [LOCATION] hiring now
```

**Supplementary sources (run 1–2 queries if Indeed/Google yield < 8 results):**
```
"[ROLE]" [LOCATION] job posting site:linkedin.com/jobs
"[ROLE]" [LOCATION] careers site:greenhouse.io OR lever.co OR workday.com
```

### Deduplication
After collecting results, deduplicate by company name + job title.
If the same role at the same company appears from multiple sources,
keep the one with the most complete information.

### Minimum viable result
Aim for 8–12 unique job listings before proceeding to Step 3.
If fewer than 6 are found, broaden the search:
- Remove location restriction and add "remote"
- Try adjacent titles (e.g. "AI Product Manager" → also "Technical Product Manager")
- Run one more broad query and note the expansion to the user

---

## Step 3 — Extract Job Data

For each listing found, extract:

| Field | How to get it |
|-------|--------------|
| Job Title | From listing title |
| Company | From listing |
| Location | From listing |
| Work Type | Remote / Hybrid / On-site — from listing or infer from location field |
| Employment Type | Full-time / Part-time / Contract — from listing, default "Not listed" |
| Salary | From listing if shown, otherwise "Not listed" |
| Posted | Date posted or "Not listed" |
| Apply URL | Direct link to the job posting |
| Description | First 300–400 words of the job description — fetch via web_fetch if needed |

If a listing lacks a description (only a snippet is available), use web_fetch
to retrieve the full job posting page and extract the description from it.
This is required for /job-match to work accurately.

---

## Step 4 — Run /job-match on Each Listing

For each extracted job, silently run the /job-match scoring logic
against the master resume in this project.

**Do not output individual /job-match reports** — only extract the score.

Scoring shortcut for speed (full /job-match blocks are available on demand):

Internally score each job across these 7 categories using the master resume:

| Category | Weight |
|----------|--------|
| Skills & Tools Match | /20 |
| Experience Level Fit | /20 |
| Domain / Industry Relevance | /15 |
| ATS Keyword Coverage | /15 |
| Compensation Alignment | /10 |
| Culture & Work Type Fit | /10 |
| Growth Potential | /10 |

Compute total `/100` for each job.
Also assign a colour indicator:
- 🟢 85–100 — Strong fit
- 🟡 70–84 — Good fit
- 🟠 55–69 — Stretch role
- 🔴 Below 55 — Significant mismatch

---

## Step 5 — Output the Ranked Table

Sort all results by match score, highest first.

Output this header block first:

```
─────────────────────────────────────────
🔍 Job Scout Results
Role searched: [ROLE] | Location: [LOCATION]
Sources: Indeed + Google Jobs | Results: [N] listings
Ranked by: /job-match score against your resume
─────────────────────────────────────────
```

Then output the ranked table:

| Rank | Score | Job Title | Company | Location | Type | Salary | Posted | Apply |
|------|-------|-----------|---------|----------|------|--------|--------|-------|
| 1 | 🟢 88/100 | [Title] | [Company] | [Location] | [Remote/Hybrid/On-site] [FT/PT/Contract] | [Salary or —] | [Date or —] | [Link] |
| 2 | 🟢 84/100 | ... | | | | | | |
| ... | | | | | | | | |

**Table rules:**
- Sort strictly by score descending — no exceptions
- Include all results regardless of score — let the user decide what to pursue
- If two jobs have identical scores, rank the more recently posted one higher
- Apply URL should be a direct clickable link to the listing — not the search result

---

## Step 6 — Post-table Summary

After the table, output this block:

```
─────────────────────────────────────────
📊 Scout Summary

Top pick:    [Job Title] at [Company] — [Score]/100
             [One sentence on why this is the strongest match]

Worth a look: [Job Title] at [Company] — [Score]/100
             [One sentence on what makes it interesting despite a lower score]

Skip:        [N] roles scored below 55 — significant skill or level mismatch

─────────────────────────────────────────
Next Steps:
─────────────────────────────────────────
→ /job-match [#N]    — Full A–F evaluation for the job at rank N
→ /resume [#N]       — Generate tailored resume for job at rank N
→ /cover [#N]        — Write cover letter for job at rank N
→ /interview-prep [#N] — Interview prep kit for job at rank N
→ /scout-all         — Run full /job-match + /resume + /cover for top 3 jobs
─────────────────────────────────────────
```

---

## Step 7 — Handling Follow-up Commands

### `/job-match [#N]`
The user wants the full Block A–F evaluation for job at rank N.
Pull that job's full description from Step 3 and run the complete
/job-match skill on it. Output all 6 blocks + score.

### `/resume [#N]`
Run /resume skill for the job at rank N using its full description.
Output the ATS keyword injection table and instructions for
tailored-resume-generator.

### `/cover [#N]`
Run /cover skill for the job at rank N.
Generate and output the cover letter .docx file.

### `/interview-prep [#N]`
Run /interview-prep skill for the job at rank N.
Output the full prep kit: 10 questions + STAR+R frameworks + questions to ask.

### `/scout-all`
For the top 3 ranked jobs, run in sequence:
1. Full /job-match report
2. ATS keyword table (/resume)
3. Cover letter (/cover)

Pause between each job:
"✅ Done — Job #[N]: [Title] at [Company]. Continue to Job #[N+1]? (yes / skip / stop)"

### Refine the search
If the user says "show more", "different results", "try [other location]",
or "search for [different title]" → re-run Step 2 with updated parameters
and re-score all new results.

---

## Output Rules

- **Never fabricate job listings** — only output results found via web search
- **Never invent scores** — every score must be computed from the master resume
  against the actual job description fetched in Step 3
- **Never skip fetching descriptions** — a snippet is not enough for accurate
  /job-match scoring. Use web_fetch on listings that only show a snippet.
- **Never ask for the resume** — it is in the project
- **Always sort by score** — the user asked for ranked results; respect that order
- **Links must be real** — if a direct apply URL cannot be confirmed, use the
  search result URL and note: "(search result link — verify before applying)"
- **Be transparent about gaps** — if fewer results than requested were found,
  say so and explain what was done to compensate

---

## Web Search Permission Note

This skill requires **web search to be enabled** in Claude settings.

If web search is not available, output:
```
⚠️ Web search is not enabled in this session.
To use /job-scout:
1. Go to Claude settings → Tools
2. Enable "Web Search"
3. Re-run /job-scout

Alternatively, paste job listings directly and I'll run /job-match
and rank them for you.
```
