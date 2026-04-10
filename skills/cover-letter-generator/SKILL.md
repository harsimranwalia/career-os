---
name: cover-letter-generator
description: "Generates a tailored, ATS-optimised cover letter matched to a specific job description using the master resume already in the project. Outputs a professionally formatted .docx file by default, or .pdf on request. Trigger on: 'write a cover letter', 'generate a cover letter', 'cover letter for this job', or when a job description is provided alongside a request for application documents."
---

# Cover Letter Generator

## Overview

This skill generates a tailored cover letter for a specific job using the master resume
and LinkedIn profile already uploaded in this project. It never asks for the resume again.
Output is a polished, download-ready .docx file by default, or .pdf if requested.

---

## Step 1 — Detect Input & Output Format

### Input detection
Check what has been provided:
- **Job description present** → proceed immediately, no questions needed
- **Job title + company only (no full JD)** → ask: "Can you paste the full job description?
  Even a partial one helps me target the letter."
- **Table row from /linkedin-jobs** → extract Job Title, Company, Location, Description
  from that row and treat it as the job input

### Output format detection
Scan the user's message for format preference:
- "pdf" or "PDF" mentioned → output as `.pdf`
- "docx" or "word" mentioned → output as `.docx`
- Nothing mentioned → default to `.docx`

Store the detected format as `OUTPUT_FORMAT`.

---

## Step 2 — Analyse the Job Description

Silently extract the following before writing anything:

**Company signals:**
- Company name, industry, and size signals (startup / enterprise / agency / nonprofit)
- Tone of the JD — formal, casual, technical, values-driven?
- Any stated company values, mission language, or cultural keywords

**Role signals:**
- Exact job title and seniority level
- Top 3 responsibilities they care most about (by emphasis and repetition)
- Top 5–8 ATS keywords: skills, tools, methodologies, role-specific terms

**Candidate match:**
- From the master resume already in this project, identify:
  - 2–3 concrete achievements that directly map to the top responsibilities
  - The most relevant role from the candidate's history for this position
  - Any specific tools, certifications, or domain experience that matches

Do not output this analysis. Use it internally to write the letter.

---

## Step 3 — Write the Cover Letter

### Tone rules
- **Enterprise / formal JD** → professional, structured, confident
- **Startup / casual JD** → direct, energetic, first-person but not informal
- **Values-heavy JD** → open with mission alignment, then pivot to credentials
- Default if unclear: **confident and direct**

### Structure — 3 paragraphs, no filler

**Opening paragraph — The Hook**
Do NOT open with:
- "I am writing to express my interest in..."
- "I am excited to apply for..."
- "Please find attached my resume..."

DO open with one of these approaches:
- A direct statement of fit: why this role + this candidate is a natural match, named specifically
- A concrete achievement that speaks to the company's core problem
- A mission-aligned statement if the company's values are prominent in the JD

Keep it to 3–4 sentences. Include 1–2 ATS keywords naturally.

**Middle paragraph — Proof**
- 2–3 specific achievements from the master resume that map to the top responsibilities
- Use numbers, percentages, timeframes, or scale wherever they exist in the resume
- Each achievement should answer: "So what does that mean for this company?"
- Do not list bullet points — weave into flowing sentences
- Include 2–3 more ATS keywords naturally

**Closing paragraph — Forward-looking close**
- One sentence on what the candidate would focus on in the first 60–90 days
- Soft call to action: express genuine interest in a conversation, not desperation
- No clichés: avoid "I look forward to hearing from you" as a standalone closer
- Keep to 3 sentences max

### Letter structure (full document)

```
[Candidate Full Name]
[City, Province/State] | [Email] | [Phone] | [LinkedIn URL]

[Date — formatted as: April 8, 2026]

[Hiring Manager Name if known, otherwise omit]
[Company Name]
[Company City, if known]

Re: [Exact Job Title] — [Company Name]

Dear [Hiring Manager Name / Hiring Team],

[Opening paragraph]

[Middle paragraph]

[Closing paragraph]

Sincerely,

[Candidate Full Name]
```

Pull candidate contact details from the master resume in the project.
If hiring manager name is not in the JD, use "Hiring Team" — never use "To Whom It May Concern."

---

## Step 4 — Generate the Output File

Read the `OUTPUT_FORMAT` detected in Step 1.

### DOCX output (default)

Generate the .docx file using python-docx. Follow ALL formatting rules below precisely.

**Install and setup:**
```bash
pip install python-docx --break-system-packages
```

**Font and colour rules — must match the resume:**
- Candidate name: `#1F3765` dark navy, bold, 16pt
- Contact line: `#1A1A1A` black, 10pt, regular
- "Re:" line and job title reference: `#1F3765` dark navy, bold, 11pt
- Section label "Dear...": black, 11pt
- Body paragraphs: `#1A1A1A` black, 11pt, regular
- "Sincerely," and candidate sign-off name: black, 11pt
- Font family: Calibri throughout

**Page setup:**
```python
from docx import Document
from docx.shared import Pt, Inches, RGBColor
from docx.enum.text import WD_ALIGN_PARAGRAPH
import datetime

doc = Document()

# Page margins — 1 inch all sides
sections = doc.sections
for section in sections:
    section.top_margin = Inches(1)
    section.bottom_margin = Inches(1)
    section.left_margin = Inches(1)
    section.right_margin = Inches(1)
```

**Full generation script:**
```python
from docx import Document
from docx.shared import Pt, Inches, RGBColor
from docx.enum.text import WD_ALIGN_PARAGRAPH
from datetime import date

NAVY = RGBColor(0x1F, 0x37, 0x65)
BLACK = RGBColor(0x1A, 0x1A, 0x1A)

doc = Document()

# Remove default paragraph spacing
from docx.oxml.ns import qn
from docx.oxml import OxmlElement

def set_spacing(paragraph, before=0, after=0, line=None):
    pf = paragraph.paragraph_format
    pf.space_before = Pt(before)
    pf.space_after = Pt(after)
    if line:
        from docx.shared import Pt as Pt2
        pf.line_spacing = Pt2(line)

def add_run(paragraph, text, bold=False, size=11, color=BLACK):
    run = paragraph.add_run(text)
    run.bold = bold
    run.font.size = Pt(size)
    run.font.color.rgb = color
    run.font.name = "Calibri"
    return run

# ── HEADER: Candidate Name ──
p_name = doc.add_paragraph()
set_spacing(p_name, before=0, after=2)
add_run(p_name, "CANDIDATE FULL NAME", bold=True, size=16, color=NAVY)

# ── Contact line ──
p_contact = doc.add_paragraph()
set_spacing(p_contact, before=0, after=12)
add_run(p_contact, "City, Province  |  email@example.com  |  +1 (555) 000-0000  |  linkedin.com/in/handle",
        size=10, color=BLACK)

# ── Date ──
p_date = doc.add_paragraph()
set_spacing(p_date, before=0, after=8)
today = date.today().strftime("%B %-d, %Y")  # e.g. April 8, 2026
add_run(p_date, today, size=11, color=BLACK)

# ── Company block (omit if unknown) ──
# p_company = doc.add_paragraph()
# add_run(p_company, "Company Name\nCity, Province", size=11, color=BLACK)
# set_spacing(p_company, before=0, after=8)

# ── Re: line ──
p_re = doc.add_paragraph()
set_spacing(p_re, before=0, after=12)
add_run(p_re, "Re: ", bold=True, size=11, color=NAVY)
add_run(p_re, "Job Title — Company Name", bold=True, size=11, color=NAVY)

# ── Salutation ──
p_sal = doc.add_paragraph()
set_spacing(p_sal, before=0, after=8)
add_run(p_sal, "Dear Hiring Team,", size=11, color=BLACK)

# ── Body paragraphs ──
body_paragraphs = [
    "OPENING PARAGRAPH TEXT HERE.",
    "MIDDLE PARAGRAPH TEXT HERE.",
    "CLOSING PARAGRAPH TEXT HERE."
]

for body_text in body_paragraphs:
    p = doc.add_paragraph()
    set_spacing(p, before=0, after=10)
    add_run(p, body_text, size=11, color=BLACK)

# ── Sign-off ──
p_sign = doc.add_paragraph()
set_spacing(p_sign, before=12, after=0)
add_run(p_sign, "Sincerely,", size=11, color=BLACK)

p_name_end = doc.add_paragraph()
set_spacing(p_name_end, before=24, after=0)
add_run(p_name_end, "CANDIDATE FULL NAME", bold=True, size=11, color=BLACK)

# ── Save ──
output_path = "/mnt/user-data/outputs/cover_letter_[CompanyName]_[JobTitle].docx"
doc.save(output_path)
print(f"Saved: {output_path}")
```

Replace all placeholder values (`CANDIDATE FULL NAME`, body paragraphs, contact line,
job title, company name) with the actual generated content before running the script.

After saving, validate the file opens cleanly:
```bash
python -c "from docx import Document; Document('/mnt/user-data/outputs/cover_letter_*.docx'); print('OK')"
```

Then present the file to the user.

---

### PDF output

When `OUTPUT_FORMAT` is `pdf`, generate the .docx first using the steps above,
then convert to PDF using LibreOffice:

```bash
pip install python-docx --break-system-packages
# Generate the .docx first (same script as above), then:
python scripts/office/soffice.py --headless --convert-to pdf \
    /mnt/user-data/outputs/cover_letter_[Company]_[Title].docx \
    --outdir /mnt/user-data/outputs/
```

If `scripts/office/soffice.py` is not available, use the direct LibreOffice command:
```bash
soffice --headless --convert-to pdf \
    /mnt/user-data/outputs/cover_letter_[Company]_[Title].docx \
    --outdir /mnt/user-data/outputs/
```

Present the .pdf file to the user. Do not present the intermediate .docx if PDF was requested.

---

## Step 5 — Post-output Summary

After presenting the file, output this block and nothing else:

```
✅ Cover Letter — [Job Title] at [Company Name]
📄 Format: [DOCX / PDF]
🎨 Tone used: [Formal / Professional / Conversational]

💡 ATS keywords woven in: [comma-separated list of the keywords used]

---
Want adjustments?
- "Make it more casual" / "Make it more formal"
- "Shorter" (I'll cut to 2 tight paragraphs)
- "Different opening hook"
- "Generate as PDF instead" / "Generate as DOCX instead"
- "Generate the matching resume" → run tailored-resume-generator
```

---

## Output Rules

- **Never output the cover letter as plain text in chat** — the file is the deliverable
- **Never ask for the resume** — it is in the project
- **Never use filler openers** — no "I am excited", "I am writing to", "Please find attached"
- **Never use "To Whom It May Concern"** — use "Hiring Team" if name is unknown
- **Never exceed 1 page** — if content is too long, tighten paragraphs, do not reduce font size below 11pt
- **Filename format**: `cover_letter_[CompanyName]_[JobTitle].docx` (no spaces — use underscores)
- **Do not add a skills section, table, or any elements not in the structure above**
- Pull all candidate details (name, email, phone, LinkedIn, city) from the master resume in the project
