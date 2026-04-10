# Career OS — Claude Web & Desktop Setup Guide

This guide walks you through setting up Career OS on [claude.ai](https://claude.ai) or the Claude desktop app — no coding or terminal required.

---

## What you'll set up

A **Claude Project** called Career OS that:
- Knows your resume and background — permanently, without re-pasting
- Has all 8 Career OS skills available as chat commands
- Keeps context across multiple chat sessions
- Generates downloadable `.docx` and `.pdf` files on demand

**Time to set up: 10–15 minutes**

---

## Step 1 — Create the Project

1. Go to [claude.ai](https://claude.ai) and sign in
2. In the left sidebar, click **Projects**
3. Click **+ New Project**
4. Name it: `Career OS`
5. Click **Create**

---

## Step 2 — Add Your Master Resume

Your resume is the foundation. All 8 skills read from it automatically.

1. Inside the Career OS project, click **Project knowledge** (or **Add content**)
2. Click **Add text**
3. Name it: `Master Resume`
4. Paste your complete resume as plain text — include all roles, achievements, skills, education, and contact info
5. Click **Save**

**Tips for best results:**
- Include specific metrics in your achievements ("grew revenue 34%", "managed a team of 8")
- List all skills and tools — the resume and job-match skills extract keywords from here
- Include your full contact info — the cover letter skill pulls it automatically

---

## Step 3 — Add Your LinkedIn Profile (Optional)

1. Click **Add content** → **Add text**
2. Name it: `LinkedIn Profile`
3. Paste your current LinkedIn headline and About section
4. Click **Save**

This is used by `/linkedin-optimizer` to understand your starting position. If you skip this, the skill will work from your resume instead.

---

## Step 4 — Add the Project Instructions

This is what activates all 8 skills and tells Claude how to behave.

1. Inside the Career OS project, click **Instructions** (or **Set instructions**)
2. Open the `CLAUDE.md` file from this repository
3. Copy the entire contents
4. Paste it into the Instructions field
5. Click **Save**

The project is now active. All 8 skills are ready.

---

## Step 5 — Enable Web Search

Several skills need web search to work properly:
- `/job-scout` — searches Indeed, Google Jobs, LinkedIn Jobs, and more
- `/job-match` — pulls salary data from Glassdoor and Levels.fyi
- `/linkedin-optimizer` — researches recruiter keywords for your target role
- `/cold-outreach` — looks up context about the recipient or company

To enable web search:
1. In any chat within the Career OS project, click the **Tools** icon (⊕ or similar)
2. Toggle **Web Search** to on
3. It stays enabled for that chat session

---

## Step 6 — Start a Chat and Use Skills

1. Click **New Chat** inside the Career OS project (not a general chat — it must be inside the project)
2. Type your first command:

```
/job-scout Product Manager remote Canada
```

or

```
/job-match
[paste a job description here]
```

---

## Using Each Skill

### Find jobs that match your resume
```
/job-scout [role] [location]
/job-scout Product Manager Vancouver
/job-scout AI Engineer remote
/job-scout                    ← reads your resume and infers the role
```

### Evaluate a specific job before applying
```
/job-match
[paste the full job description]
```

### Generate a tailored resume (.docx)
```
/tailored-resume-generator
[paste the job description]
```
or after running job-scout:
```
/tailored-resume-generator #1
```

### Generate a cover letter (.docx or .pdf)
```
/cover-letter-generator
[paste the job description]
```
To get a PDF instead: `"/cover-letter-generator #1 as PDF"`

### Get interview prep for a specific role
```
/interview-prep
[paste the job description]
```

### Run a mock interview
```
/mock-interview
[paste the job description]
```
After /interview-prep, just type `/mock-interview` — the JD is already in context.

### Optimise your LinkedIn profile
```
/linkedin-optimizer Senior Product Manager at a B2B SaaS company
```

### Write personalised outreach messages
```
/cold-outreach
Recruiter: Jane Smith, Senior Recruiter at Shopify
Goal: Connection request about PM openings
Their LinkedIn: [paste any details you have about them]
```

---

## Running Pipelines (multi-skill sequences)

You can chain skills together manually in one chat session. The context carries forward — you don't need to paste the JD or resume again.

### Full application (find → evaluate → resume → cover letter)
```
1. /job-scout Product Manager remote Canada
2. /job-match #1
3. /tailored-resume-generator #1
4. /cover-letter-generator #1
```

### Interview ready (prep → mock)
```
1. /interview-prep [paste JD]
2. /mock-interview
```

### Job discovery (find → evaluate top 3)
```
1. /job-scout Senior Engineer Toronto
2. /job-match #1
3. /job-match #2
4. /job-match #3
```

Or just say: `"Run job discovery pipeline for Senior Engineer in Toronto"` and Claude will chain them automatically.

---

## Tips for Best Results

**Keep the chat session alive during a workflow.** Context is maintained within a single chat. If you start a new chat, reference the job again with a quick paste.

**Use the project, not a general chat.** Skills only have access to your resume and instructions when you're inside the Career OS project. Starting a chat outside the project won't load your background.

**For job-scout**, enable web search before running it. The skill searches live job boards — without web search, it can't find real listings.

**For cover letters and resumes**, Claude generates a downloadable file. Click the download icon in the chat to save it. The file will open in Word or any compatible app.

**To adjust tone on cover letters:** after generating, just say "make it more conversational" or "make it more formal" — Claude rewrites and regenerates the file.

---

## Skill Reference Card

| Command | What you get |
|---------|-------------|
| `/job-scout [role] [location]` | Ranked table of live jobs from 7 sources, scored /100 |
| `/job-match` + JD | A–F evaluation, score /100, gap analysis, comp data |
| `/tailored-resume-generator` + JD | ATS-optimised .docx resume |
| `/cover-letter-generator` + JD | Matched cover letter .docx or .pdf |
| `/interview-prep` + JD | 10 questions, STAR+R frameworks, questions to ask |
| `/mock-interview` + JD | Live 5-question interview, per-answer feedback, debrief |
| `/linkedin-optimizer [target role]` | Rewritten headline, About, experience bullets, 15 skills |
| `/cold-outreach` + recipient details | 2-variant connection request, InMail, and cold email |
