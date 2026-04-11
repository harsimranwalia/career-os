# Career OS

**An AI-powered job search operating system built on Claude.**

Career OS is a collection of eight composable skills that take you from finding the right jobs all the way to walking into an interview confident and prepared. Each skill runs independently or chains with the others. You bring your resume — Career OS handles everything else.

---

## What It Does

Most job seekers apply blind, send generic resumes, and wonder why they never hear back. Career OS fixes every step of that:

| Problem | Career OS Solution |
|---------|-------------------|
| Don't know which jobs to apply for | `/job-scout` searches 7 sources and ranks results by match score |
| Resume doesn't pass ATS | `/tailored-resume-generator` injects the exact keywords ATS scans for |
| Cover letter sounds generic | `/cover-letter-generator` writes a matched pair that tells one coherent story |
| Don't know if a job is worth applying to | `/job-match` gives a full A–F evaluation with a score out of 100 |
| Not prepared for the interview | `/interview-prep` builds STAR+R frameworks from your actual experience |
| Never practised answering live | `/mock-interview` runs a full session with per-answer feedback |
| LinkedIn isn't attracting recruiters | `/linkedin-optimizer` rewrites your profile for inbound discovery |
| Don't know how to reach out cold | `/cold-outreach` writes personalised DMs that feel human |

---

## The 8 Skills

```
skills/
├── job-scout/              Find & rank jobs from 7 sources
├── job-match/              Full A–F evaluation, score /100
├── tailored-resume-generator/  ATS-optimised .docx per job
├── cover-letter-generator/ Matched cover letter, .docx or PDF
├── interview-prep/         10 questions + STAR+R frameworks
├── mock-interview/         Live interview loop with feedback
├── linkedin-optimizer/     Rewrite profile for inbound recruiting
└── cold-outreach/          Personalised connection requests & DMs
```

---

## Two Ways to Use Career OS

---

### Option A — Claude Code (Recommended for power users)

Claude Code runs skills as autonomous agents directly in your terminal. Skills can be piped together, run in sequence, and output files directly to your filesystem.

**Prerequisites:**
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed
- A Claude account (Pro recommended for extended sessions)

**Setup:**

```bash
# Clone the repo
git clone https://github.com/yourusername/career-os.git
cd career-os

# Add your resume and LinkedIn profile
cp your-resume.pdf profile/master-resume.pdf
# Or paste resume text into:
nano profile/master-resume.md

# Start Claude Code
claude
```

**Running individual skills:**

```bash
# In Claude Code — reference a skill directly
> Read skills/job-scout/SKILL.md and run it for "Product Manager roles in Vancouver"

> Read skills/tailored-resume-generator/SKILL.md and tailor my resume at 
  profile/master-resume.md for this job: [paste JD]

> Read skills/job-match/SKILL.md and evaluate this job posting: [paste JD]
```

**Running the full pipeline:**

```bash
# Load and run the complete application pipeline
> Read pipelines/full-apply.md and run it for job #1 from my last scout results
```

**Piping skills together (agent chain):**

```bash
# Scout → Match → Resume → Cover Letter in one session
> Read pipelines/scout-and-apply.md and execute it for "AI Product Manager remote"
```

**Output files** land in:
```
outputs/
├── resumes/          Tailored .docx resumes per job
├── cover-letters/    Cover letters per job
├── reports/          Job match evaluation reports
└── interview-notes/  Prep kits and mock interview debriefs
```

---

### Option B — Claude.ai Web or Desktop (No coding required)

Use Career OS entirely inside a Claude Project on claude.ai. Setup takes 10–15 minutes.

**Step 1 — Create a Project**

1. Go to [claude.ai](https://claude.ai) and sign in
2. Click **Projects** in the left sidebar → **+ New Project**
3. Name it: `Career OS`

**Step 2 — Add your resume**

1. Inside the project, click **Project knowledge** → **Add text**
2. Name it: `Master Resume`
3. Paste your full resume as plain text — include all roles, achievements, skills, education, and contact info
4. Click **Save**

> Include specific metrics ("grew revenue 34%", "managed a team of 8") and your full contact info — skills extract these automatically.

Optionally add your LinkedIn profile: **Add text** → name it `LinkedIn Profile` → paste your headline and About section. Used by `/linkedin-optimizer`; skippable.

**Step 3 — Add the Project Instructions**

1. Click **Instructions** → **Edit project instructions**
2. Paste the full contents of `project-system-prompt.md` (root of this repo)
3. Click **Save**

The instructions are intentionally minimal — skill logic lives in the custom skills, not here.

**Step 4 — Install the Skills**

Skills run from **Claude custom skills**, not the project instructions. For each skill:

1. Inside the project, go to **Custom skills** → **Add custom skill**
2. Name it the skill command (e.g. `job-scout`)
3. Paste the full contents of the corresponding `skills/<name>/SKILL.md` file

| Custom skill name | File to paste |
|-------------------|---------------|
| `job-scout` | `skills/job-scout/SKILL.md` |
| `job-match` | `skills/job-match/SKILL.md` |
| `tailored-resume-generator` | `skills/tailored-resume-generator/SKILL.md` |
| `cover-letter-generator` | `skills/cover-letter-generator/SKILL.md` |
| `interview-prep` | `skills/interview-prep/SKILL.md` |
| `mock-interview` | `skills/mock-interview/SKILL.md` |
| `linkedin-optimizer` | `skills/linkedin-optimizer/SKILL.md` |
| `cold-outreach` | `skills/cold-outreach/SKILL.md` |

**Step 5 — Enable Web Search**

Required for `/job-scout`, `/job-match` (comp data), and `/cold-outreach`. In any project chat, click the **Tools** icon and toggle **Web Search** on.

**Step 6 — Start using skills**

Open a **New Chat inside the project** (not a general chat — it must be inside Career OS) and run any skill:

```
/job-scout Product Manager remote Canada

/job-match
[paste job description]

/tailored-resume-generator #1      ← references job #1 from a previous scout

/cover-letter-generator #1

/interview-prep #1

/mock-interview                    ← JD already in context after /interview-prep

/linkedin-optimizer targeting: Senior PM at a B2B SaaS company

/cold-outreach
Recruiter: Jane Smith at Shopify
Goal: Connection request about PM openings
```

**Chaining skills:** after `/job-scout` produces a ranked table, reference jobs by number. Context carries forward within the session — no need to paste the JD again.

```
/job-scout Senior Engineer Toronto
/job-match #1
/tailored-resume-generator #1
/cover-letter-generator #1
/interview-prep #1
/mock-interview
```

---

## The Standard Workflow

```
1. /job-scout                     → 20–50 live jobs from 7 sources
        ↓
2. Quick Score (automatic)        → Each job scored 0–20 across 4 dimensions
                                    Only jobs ≥ 14 proceed — the rest are shown but skipped
        ↓
3. /job-match #N                  → Deep A–F evaluation on shortlisted jobs only
                                    (duplicate check: warns if already applied)
        ↓
4. /tailored-resume-generator #N  → ATS-optimised .docx for top opportunities only
        ↓
5. /cover-letter-generator #N     → Matched cover letter (.docx or PDF)
        ↓
6. Apply                          → Submit — Claude logs it to application tracker
        ↓
7. /interview-prep #N             → 10 questions + STAR+R frameworks
        ↓
8. /mock-interview #N             → Live practice session with per-answer feedback
        ↓
9. Negotiate                      → Use /job-match comp data to anchor salary
```

The Quick Score layer is the key efficiency lever — `/job-match` is expensive and thorough, so it only runs on jobs that clear the bar. Quick Score uses your `targets.md` preferences (visa, location, role, industry) to filter automatically.

---

## Profile Files

```
profile/
├── master-resume.md        Your resume in markdown (edit this)
├── master-resume.pdf       Upload your PDF resume here
└── linkedin-profile.md     Paste your LinkedIn About + experience here (optional)
```

The `profile/` directory is the only thing you need to personalise. Every skill reads from it — you never re-enter your background.

---

## Memory (Claude Code only)

Career OS builds up a memory layer across sessions automatically — no setup required.

```
.claude/memory/
├── targets.md      Target roles, salary, location, visa, company preferences
└── learnings.md    Patterns from scout results, match scores, interview feedback
```

Claude writes to these files during conversations when it learns something durable:
- Tell it you want PM roles in Canada at $140k+ → written to `targets.md`
- Run `/job-scout` and certain sources perform well → noted in `learnings.md`
- Give feedback on a resume → captured for future runs

On the next session, Claude reads both files before running any skill — so it already knows your constraints without you repeating them. The files start empty and compound over time.

---

## Application Tracking (Claude Code only)

Every resume generation and application is logged automatically.

```
.claude/state/
└── applications.md     Running log of every application and its current status
```

Claude writes a new row whenever a resume is generated, and updates the status when you report back ("I got an interview at Shopify"). Before generating a resume, it checks for duplicates and warns you if you've already applied to that company + role.

Statuses: `resume_generated` → `applied` → `screen` → `interview` → `offer` / `rejected` / `withdrawn`

---

## Outputs

All generated files are saved to `outputs/` with consistent naming:

```
outputs/resumes/          resume_[Company]_[Role]_[Date].docx
outputs/cover-letters/    cover_letter_[Company]_[Role]_[Date].docx
outputs/reports/          match_report_[Company]_[Role]_[Date].md
outputs/interview-notes/  prep_[Company]_[Role]_[Date].md
```

---

## Skill Reference

| Command | Input | Output | Format |
|---------|-------|--------|--------|
| `/job-scout` | Role + location (or nothing — infers from resume) | Ranked job table | Chat |
| `/job-match` | Job description or #N from scout | A–F evaluation + score /100 | Chat + .md report |
| `/tailored-resume-generator` | Job description or #N | ATS-optimised resume | .docx |
| `/cover-letter-generator` | Job description or #N | Matched cover letter | .docx or PDF |
| `/interview-prep` | Job description or #N | 10 Qs + STAR+R + ask-them list | Chat |
| `/mock-interview` | Job description or #N | Live 5-question session + debrief | Interactive chat |
| `/linkedin-optimizer` | Target role (+ current profile optional) | Headline × 2, About, bullets, skills | Chat (copy-ready) |
| `/cold-outreach` | Recipient + purpose + channel | 2 message variants per channel | Chat (copy-ready) |

---

## Requirements

**For Claude Code:**
- Claude Code CLI installed
- Claude Pro or API access
- Web search enabled for `/job-scout`, `/job-match` (comp data), `/cold-outreach`

**For Claude.ai Projects:**
- Claude.ai account (Free tier works; Pro recommended for longer sessions)
- Web search enabled in Claude settings for `/job-scout`

---

## Contributing

Career OS is designed to be extended. To add a new skill:

1. Create `skills/your-skill-name/SKILL.md`
2. Follow the frontmatter format:
```yaml
---
name: your-skill-name
description: "Trigger description — what this skill does and when to use it."
---
```
3. Install it as a custom skill in your Claude Project (paste the full SKILL.md content)
4. Document it in this README

---

## License

MIT — use it, fork it, extend it. If you build something good on top of it, share it back.
