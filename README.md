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

### Option B — Claude.ai Web or Desktop (No setup required)

If you're not a developer, you don't need Claude Code. You can use Career OS entirely inside a Claude Project on claude.ai. This approach is slightly more manual but just as powerful.

**Step 1 — Create a Project**

1. Go to [claude.ai](https://claude.ai)
2. Click **Projects** in the left sidebar → **New Project**
3. Name it: `Career OS` (or your name + Job Search)

**Step 2 — Upload your profile files**

Inside the project, click **Add content** and upload:
- Your master resume (PDF or Word doc)
- Your LinkedIn profile export (optional — download from LinkedIn Settings → Data Privacy)
- Any other background documents (portfolio, bio, references)

**Step 3 — Add the Project Instructions**

Click **Edit project instructions** and paste the contents of:
```
project-setup/project-system-prompt.md
```

This tells Claude that your resume is always in context and enables all eight skills.

**Step 4 — Use skills by command**

In any chat within the project, trigger skills with their slash command:

```
/job-scout Product Manager remote Canada

/job-match [paste job description here]

/tailored-resume-generator [paste job description here]

/cover-letter-generator [paste job description here]

/interview-prep [paste job description here]

/mock-interview [paste job description here]

/linkedin-optimizer targeting: Senior Product Manager at a B2B SaaS company

/cold-outreach reach out to the hiring manager at Shopify for the PM role
```

**Step 5 — Chain skills manually**

After `/job-scout` produces a ranked table, pick a job and run:

```
/job-match #1
```

Then:
```
/tailored-resume-generator #1
/cover-letter-generator #1
/interview-prep #1
```

Then when the interview is scheduled:
```
/mock-interview #1
```

**Important:** In the web/desktop version, skills are loaded from the project instructions context — not from files. The `project-setup/project-system-prompt.md` file contains condensed versions of all skill triggers so Claude knows how to run each one without reading individual SKILL.md files.

---

## The Standard Workflow

```
1. /job-scout        → Ranked table of 10–15 live jobs, scored against your resume
        ↓
2. /job-match #N     → Is this job worth applying to? Full A–F report
        ↓
3. /tailored-resume-generator #N  → ATS-optimised .docx resume for that job
        ↓
4. /cover-letter-generator #N     → Matched cover letter (.docx or PDF)
        ↓
5. Apply             → Submit resume + cover letter
        ↓
6. /interview-prep #N → 10 questions + STAR+R frameworks before the interview
        ↓
7. /mock-interview #N → Live practice session with feedback
        ↓
8. Negotiate         → Use /job-match comp data to anchor your salary negotiation
```

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

## Outputs

All generated files are saved to `outputs/` with consistent naming:

```
outputs/resumes/          resume_[Company]_[Role]_[Date].docx
outputs/cover-letters/    cover_letter_[Company]_[Role]_[Date].docx
outputs/reports/          match_report_[Company]_[Role]_[Date].md
outputs/interview-notes/  prep_[Company]_[Role]_[Date].md
```

---

## Adding Skills to Claude.ai (Web/Desktop) Manually

If you want to install individual skills into Claude's web shortcuts instead of using a project:

1. Open the Claude Chrome Extension or Claude.ai sidebar
2. Go to **Settings → Shortcuts**
3. Create a new shortcut for each skill
4. Name it the skill's command (e.g. `job-scout`)
5. Paste the contents of `skills/job-scout/SKILL.md` as the shortcut body

This lets you trigger skills from any Claude chat with `/job-scout`, `/job-match`, etc. — even outside a project.

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
3. Add it to `project-setup/project-system-prompt.md` so web/desktop users can trigger it
4. Document it in this README

---

## License

MIT — use it, fork it, extend it. If you build something good on top of it, share it back.
