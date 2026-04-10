# CLAUDE.md — Career OS

This file tells Claude Code how to operate within the Career OS repository.
Read this first before reading any skill files or running any commands.

---

## What This Repo Is

Career OS is a collection of eight composable job-search skills. Each skill
lives in `skills/<skill-name>/SKILL.md` and contains complete instructions
for how Claude should behave when that skill is invoked.

You are the agent. The human is the job seeker. Your job is to execute skills
accurately, chain them when asked, and save outputs to the right directories.

---

## Candidate Profile — Read First, Always

Before running any skill, read the candidate's profile:

```
profile/master-resume.md     ← Primary source. Always read this.
profile/master-resume.pdf    ← Fallback if .md is missing or incomplete.
profile/linkedin-profile.md  ← Supplementary context. Read if present.
```

**Never ask the human for their resume.** It is always in `profile/`.
If `profile/master-resume.md` is empty or missing, tell the human:
"Please add your resume to `profile/master-resume.md` before running skills."

---

## How to Run a Skill

When the human asks to run a skill — by command, by name, or by description —
follow this pattern:

1. **Read the skill file:** `skills/<skill-name>/SKILL.md`
2. **Read the candidate profile:** `profile/master-resume.md`
3. **Execute the skill** following its instructions exactly
4. **Save outputs** to the correct directory (see Output Rules below)
5. **Report completion** with the file path and a one-line summary

Do not ask clarifying questions before reading the skill file.
The skill file itself defines what to ask if information is missing.

---

## Skill Directory

| Command | Skill File |
|---------|-----------|
| `/job-scout` | `skills/job-scout/SKILL.md` |
| `/job-match` | `skills/job-match/SKILL.md` |
| `/tailored-resume-generator` | `skills/tailored-resume-generator/SKILL.md` |
| `/cover-letter-generator` | `skills/cover-letter-generator/SKILL.md` |
| `/interview-prep` | `skills/interview-prep/SKILL.md` |
| `/mock-interview` | `skills/mock-interview/SKILL.md` |
| `/linkedin-optimizer` | `skills/linkedin-optimizer/SKILL.md` |
| `/cold-outreach` | `skills/cold-outreach/SKILL.md` |

**Triggering by description:** If the human says "find me jobs", "tailor my resume",
"help me prep for this interview", or any natural language equivalent — map it
to the correct skill and run it without asking for confirmation.

---

## Output Rules

Save all generated files using this naming convention:

```
outputs/resumes/          resume_[Company]_[Role]_[YYYY-MM-DD].docx
outputs/cover-letters/    cover_letter_[Company]_[Role]_[YYYY-MM-DD].docx
outputs/reports/          match_report_[Company]_[Role]_[YYYY-MM-DD].md
outputs/interview-notes/  prep_[Company]_[Role]_[YYYY-MM-DD].md
                          mock_debrief_[Company]_[Role]_[YYYY-MM-DD].md
```

- Use underscores, not spaces, in all filenames
- Company and role names: lowercase, hyphens for spaces (e.g. `shopify`, `product-manager`)
- Always confirm the saved file path to the human after saving

---

## Pipeline Execution

When the human asks to run a pipeline (from `pipelines/`), read the pipeline
file first to understand the sequence, then execute each step in order.

**Available pipelines:**
```
pipelines/full-apply.md        Scout → Match → Resume → Cover Letter
pipelines/scout-and-match.md   Scout → Match top N results
pipelines/interview-ready.md   Interview Prep → Mock Interview
```

Between pipeline steps, briefly report what was completed and what's next.
Ask for confirmation only if a step requires a choice (e.g. which job to target).

---

## Web Search

The following skills require web search to function correctly:

| Skill | Why |
|-------|-----|
| `/job-scout` | Fetches live job listings from 7 sources |
| `/job-match` | Pulls salary data from Glassdoor, Levels.fyi |
| `/cold-outreach` | Researches the recipient and their company |

If web search is unavailable, tell the human which skill is affected and what
can still be done without it (e.g. `/job-match` can still evaluate a pasted JD,
it just won't have current salary data).

---

## File Generation

When a skill requires generating a `.docx` or `.pdf` file:

1. Use `python-docx` for `.docx` generation (install with `pip install python-docx --break-system-packages`)
2. Convert to PDF via LibreOffice if requested: `soffice --headless --convert-to pdf <file>`
3. Follow the formatting rules in the skill file exactly — font sizes, colours, and layout are specified
4. Validate the file opens cleanly after generation
5. Save to the correct `outputs/` subdirectory

---

## Session State

Claude Code has no memory between sessions. At the start of each session:

- Re-read `profile/master-resume.md`
- Check `outputs/` to understand what has already been generated
- If the human references a previous scout result or match report, look for it in `outputs/reports/`

If a previous output is referenced (e.g. "use the resume you made yesterday"),
search `outputs/` by filename pattern before asking the human to re-provide information.

---

## Chaining Skills — Context Passing

When skills are chained (e.g. scout → match → resume → cover letter),
pass context forward without requiring the human to re-paste information:

- Job description extracted in `/job-scout` → available for subsequent skills in the same session
- Match report from `/job-match` → used to inform resume keyword strategy
- Resume generated by `/tailored-resume-generator` → used as basis for cover letter
- Interview prep from `/interview-prep` → loaded into `/mock-interview` question bank

Always confirm before moving to the next step in a chain:
"✅ Resume saved to `outputs/resumes/resume_shopify_pm_2026-04-09.docx`.
Ready to generate the cover letter? (yes / skip)"

---

## Tone and Behaviour

- Be direct. Skip preamble. Start executing immediately after reading the skill.
- Never apologise for limitations — state what can be done and do it.
- Output formatting: follow the exact format specified in each skill file.
- If a skill file is ambiguous, use good judgment and proceed — don't stall on edge cases.
- When a skill produces output for the human to copy (LinkedIn text, outreach messages),
  use a code block so it's easy to copy cleanly.

---

## Error Handling

| Situation | Action |
|-----------|--------|
| Profile file missing | Tell the human, provide exact path to fix |
| Job description not provided | Ask once: "Please paste the job description" |
| Web search unavailable | Proceed without it, note which data is missing |
| File generation fails | Report the error, output the content as text instead |
| Skill file not found | Tell the human: "Skill file missing at `skills/<name>/SKILL.md`" |

---

## Adding New Skills

To add a skill to this repo:

1. Create `skills/<skill-name>/SKILL.md` with the frontmatter format:
```yaml
---
name: skill-name
description: "One sentence description for trigger matching."
---
```
2. Update this CLAUDE.md skill directory table
3. Update `pipelines/` files if the skill fits into a pipeline
4. Update `project-setup/project-system-prompt.md` for web/desktop users
