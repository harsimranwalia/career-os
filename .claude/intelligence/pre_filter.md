# Pre-Filter: Quick Score Layer

Runs automatically after `/job-scout` and before any `/job-match`.
Scores each job 0–20 across four dimensions using the candidate's resume
and `.claude/memory/targets.md` as input.

---

## Scoring Rubric

### 1. Role Match (0–5)

Does the job title and core responsibilities match the candidate's target role?

| Score | Criteria |
|-------|----------|
| 5 | Title and responsibilities are an exact match to target role |
| 4 | Title differs slightly but core work is the same |
| 3 | Meaningful overlap — at least 60% of responsibilities align |
| 2 | Related role but significant scope mismatch |
| 1 | Tangentially related |
| 0 | Unrelated |

### 2. Seniority Fit (0–5)

Does the required experience level match the candidate's actual experience?

| Score | Criteria |
|-------|----------|
| 5 | Years, scope, and team size are a direct match |
| 4 | One level off but candidate can credibly position up or down |
| 3 | Requires stretch — candidate meets 60–80% of seniority signals |
| 2 | Significant gap — likely to screen out at resume stage |
| 1 | Clear mismatch (e.g. Director role for a mid-level candidate) |
| 0 | Not applicable / no experience overlap |

### 3. Industry Relevance (0–5)

Is the industry one the candidate has worked in or is explicitly targeting?

| Score | Criteria |
|-------|----------|
| 5 | Exact industry match (candidate has direct experience) |
| 4 | Adjacent industry — skills transfer cleanly |
| 3 | Different industry but domain knowledge partially applies |
| 2 | Industry is a stretch; JD likely expects sector-specific knowledge |
| 1 | Unrelated industry |
| 0 | Industry is a hard constraint mismatch (e.g. requires sector licence) |

### 4. Geography / Visa / Constraints (0–5)

Does the role satisfy location, remote policy, and work authorisation constraints from `targets.md`?

| Score | Criteria |
|-------|----------|
| 5 | All constraints satisfied (location, remote, visa) |
| 4 | Minor friction — e.g. hybrid when fully remote preferred |
| 3 | One constraint borderline — worth flagging but not a blocker |
| 2 | One hard constraint likely violated (e.g. requires sponsorship candidate can't get) |
| 1 | Multiple constraint mismatches |
| 0 | Hard block — role is not viable regardless of other scores |

---

## Threshold

| Score | Decision |
|-------|----------|
| 18–20 | Strong — prioritise for `/job-match` |
| 14–17 | Eligible — run `/job-match` |
| 10–13 | Borderline — show to human with reason, skip unless overridden |
| 0–9   | Skip — do not run `/job-match` |

---

## Output Format

Add a `QS` column to the scout results table. Show all jobs, including skipped ones.

```
| #  | Company  | Role              | QS    | Status   | Reason (if skipped)        |
|----|----------|-------------------|-------|----------|----------------------------|
| 1  | Shopify  | Senior PM         | 18/20 | ✅ Match  |                            |
| 2  | Stripe   | PM, Payments      | 16/20 | ✅ Match  |                            |
| 3  | Acme Co  | Director of PM    | 11/20 | ⚠️ Skip  | Seniority gap (2), visa (1)|
| 4  | US Corp  | Product Manager   | 6/20  | ❌ Skip  | Requires US citizenship    |
```

After the table, list eligible jobs (≥ 14) and prompt:
`"Ready to run /job-match on #1 and #2? (yes / pick / skip)"`

---

## Inputs

- `profile/master-resume.md` — candidate experience, skills, seniority signals
- `.claude/memory/targets.md` — target roles, salary, location, visa, company preferences
- Job listing data from `/job-scout` — title, responsibilities, location, requirements

## Override

If the human explicitly asks to run `/job-match` on a job below the threshold,
proceed without warning — the human's explicit instruction takes priority.
Note the override in `.claude/memory/learnings.md`.
