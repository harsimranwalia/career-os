---
name: tailored-resume-generator
description: "Analyzes job descriptions and generates tailored resumes that highlight relevant experience, skills, and achievements to maximize interview chances"
---

## Instructions

When a user requests resume tailoring:

### 1. Gather Information

**Job Description Analysis**:
- Request the full job description if not provided
- Ask for the company name and job title

**Candidate Background**:
- If user provides existing resume, use it as the foundation
- If not, request:
  - Work history (job titles, companies, dates, responsibilities)
  - Education background
  - Key skills and technical proficiencies
  - Notable achievements and metrics
  - Certifications or awards
  - Any other relevant information

### 2. Analyze Job Requirements

Extract and prioritize:
- **Must-have qualifications**: Years of experience, required skills, education
- **Key skills**: Technical tools, methodologies, competencies
- **Soft skills**: Communication, leadership, teamwork
- **Industry knowledge**: Domain-specific experience
- **Keywords**: Repeated terms, phrases, and buzzwords for ATS optimization
- **Company values**: Cultural fit indicators from job description

Create a mental map of:
- Priority 1: Critical requirements (deal-breakers)
- Priority 2: Important qualifications (strongly desired)
- Priority 3: Nice-to-have skills (bonus points)

### 3. Map Candidate Experience to Requirements

For each job requirement:
- Identify matching experience from candidate's background
- Find transferable skills if no direct match
- Note gaps that need to be addressed or de-emphasized
- Identify unique strengths to highlight

### 4. Structure the Tailored Resume

**Professional Summary** (3-4 lines):
- Lead with years of experience in the target role/field
- Include top 3-4 required skills from job description
- Mention industry experience if relevant
- Highlight unique value proposition

**Technical/Core Skills Section**:
- Group skills by category matching job requirements
- List required tools and technologies first
- Use exact terminology from job description
- Only include skills you can substantiate with experience
- **Single-line format**: Each skill category must be on **one line** — bold category name followed by a colon, then comma-separated skills. Example: **Languages:** Python, JavaScript, SQL — never put the category title and skills on separate lines

**Professional Experience**:
- For each role, emphasize responsibilities and achievements aligned with job requirements
- Use action verbs: Led, Developed, Implemented, Optimized, Managed, Created, Analyzed
- **Quantify achievements**: Include numbers, percentages, timeframes, scale
- Reorder bullet points to prioritize most relevant experience
- Use keywords naturally from job description
- Format: **[Action Verb] + [What] + [How/Why] + [Result/Impact]**

**Education**:
- List degrees, certifications relevant to position
- Include relevant coursework if early career
- Add certifications that match job requirements

**Optional Sections** (if applicable):
- Certifications & Licenses
- Publications or Speaking Engagements
- Awards & Recognition
- Volunteer Work (if relevant to role)
- Projects (especially for technical roles)

### 5. Optimize for ATS (Applicant Tracking Systems)

- Use standard section headings (Professional Experience, Education, Skills)
- Incorporate exact keywords from job description naturally
- Avoid tables, graphics, headers/footers, or complex formatting
- Use standard fonts and bullet points
- **Font sizes**: Use 11pt or 11.5pt for body text and bullet points; section headings at 11.5pt–12pt; name/header at 14–16pt. Minimum font size anywhere on the resume is 10pt — never go below this, even for metadata or footer text.
- Include both acronyms and full terms (e.g., "SQL (Structured Query Language)")
- Match job title terminology where truthful

### 6. Format and Present

**Format Options**:
- **DOCX (default)**: Always generate a `.docx` file unless the user explicitly requests otherwise. Use the `docx` skill to produce a properly formatted Word document with correct fonts, colors, and layout.
- **Markdown**: Only if user explicitly requests it
- **Plain Text**: Only if user explicitly requests it

**Resume Structure Guidelines**:
- Keep to 1 page for <10 years experience, 2 pages for 10+ years
- Use consistent formatting and spacing
- Ensure contact information is prominent
- Use reverse chronological order (most recent first)
- Maintain clean, scannable layout with white space
- **Font size rules** (enforce strictly in all Word/PDF/formatted outputs):
  - Body text & bullet points: **11pt or 11.5pt** (prefer 11.5pt when space allows)
  - Subheadings / role titles: **11.5pt**
  - Section headings: **11.5pt–12pt**
  - Candidate name: **14–16pt**
  - Footer / page numbers / metadata: **10pt minimum** (never below 10pt)
- **Color rules** — apply consistently across all formatted outputs:
  - **Candidate name**: Dark navy `#1F3765` — bold
  - **Section headings**: Dark navy `#1F3765` — bold, **center aligned**
  - **Company names**: Dark navy `#1F3765` — bold
  - **Skill category labels** (e.g. "Languages:"): Black `#000000` — **bold**, followed immediately by a colon, then the skills list in black regular weight on the **same paragraph/line**. No color styling — do not apply navy or any color to skill labels. In `python-docx`, add two runs to a single paragraph: `run1` (bold, black) for `"CategoryName:"` and `run2` (normal weight, black) for `" Skill1, Skill2, Skill3"`.
  - All other text (body, bullets, dates, contact info): Black `#000000` or near-black `#1A1A1A`
  - In `python-docx`, set color via: `run.font.color.rgb = RGBColor(0x1F, 0x37, 0x65)`
- **Alignment rules**:
  - Section headings: `paragraph.alignment = WD_ALIGN_PARAGRAPH.CENTER`
  - **Company name + date range on the same line**: Do NOT put the date on a separate paragraph. Use a single paragraph with a right-aligned tab stop so the company name sits on the left and the date range sits flush right on the same row. Implementation:
    ```python
    from docx.oxml.ns import qn
    from docx.oxml import OxmlElement
    from docx.shared import Pt, Inches

    def add_company_date_row(doc, company, date_range, indent_left=0):
        """Adds a paragraph: Company Name <tab> Date Range (right-aligned)."""
        p = doc.add_paragraph()
        p.paragraph_format.space_before = Pt(0)
        p.paragraph_format.space_after = Pt(0)

        # Set a right-aligned tab stop at the right margin (e.g. 6.5 inches)
        pPr = p._p.get_or_add_pPr()
        tabs = OxmlElement('w:tabs')
        tab = OxmlElement('w:tab')
        tab.set(qn('w:val'), 'right')
        tab.set(qn('w:pos'), str(int(6.5 * 1440)))  # 1440 twips per inch
        tabs.append(tab)
        pPr.append(tabs)

        # Company name run (bold, navy)
        run_co = p.add_run(company)
        run_co.bold = True
        run_co.font.color.rgb = RGBColor(0x1F, 0x37, 0x65)

        # Tab character to jump to right-aligned stop
        p.add_run('\t')

        # Date range run (normal weight, black)
        run_date = p.add_run(date_range)
        run_date.bold = False
        run_date.font.color.rgb = RGBColor(0x00, 0x00, 0x00)

        return p
    ```
  - All other content: left aligned
- **CRITICAL — docx font size conversion**: When generating `.docx` files, always set font size via `run.font.size = Pt(n)` from `docx.shared` — this handles the conversion automatically and correctly. If you must write a manual `pt()` helper, the correct formula is `n * 2` (half-points), **never** `n * 20` (that is twips, used only for paragraph spacing/margins). Using `n * 20` makes every font size exactly 10× too large (e.g. 18pt → 180pt).

### 7. Provide Strategic Recommendations

After presenting the tailored resume, offer:

**Strengths Analysis**:
- What makes this candidate competitive
- Unique qualifications to emphasize in cover letter or interview

**Gap Analysis**:
- Requirements not fully met
- Suggestions for addressing gaps (courses, projects, reframing experience)

**Interview Preparation Tips**:
- Key talking points aligned with resume
- Stories to prepare based on job requirements
- Questions to ask that demonstrate fit

**Cover Letter Hooks**:
- Suggest 2-3 opening lines for cover letter
- Key achievements to expand upon

### 8. Iterate and Refine

Ask if user wants to:
- Adjust emphasis or tone
- Add or remove sections
- Generate alternative versions for different roles
- Create format variations (traditional vs. modern)
- Develop role-specific versions (if applying to multiple similar positions)

### 9. Best Practices to Follow

**Do**:
- Be truthful and accurate - never fabricate experience
- Use industry-standard terminology
- Quantify achievements with specific metrics
- Tailor each resume to specific job
- Proofread for grammar and consistency
- Keep language concise and impactful

**Don't**:
- Include personal information (age, marital status, photo unless requested)
- Use first-person pronouns (I, me, my)
- Include references ("available upon request" is outdated)
- List every job if career is 20+ years (focus on relevant, recent experience)
- Use generic templates without customization
- Exceed 2 pages unless very senior role

### 10. Special Considerations

**Career Changers**:
- Use functional or hybrid resume format
- Emphasize transferable skills
- Create compelling narrative in summary
- Focus on relevant projects and coursework

**Recent Graduates**:
- Lead with education
- Include relevant coursework, projects, internships
- Emphasize leadership in student organizations
- Include GPA if 3.5+

**Senior Executives**:
- Lead with executive summary
- Focus on leadership and strategic impact
- Include board memberships, speaking engagements
- Emphasize revenue growth, team building, vision

**Technical Roles**:
- Include technical skills section prominently
- List programming languages, frameworks, tools
- Include GitHub, portfolio, or project links
- Mention methodologies (Agile, Scrum, etc.)

**Creative Roles**:
- Include link to portfolio
- Highlight creative achievements and campaigns
- Mention tools and software proficiencies
- Consider more creative formatting (while maintaining ATS compatibility)
