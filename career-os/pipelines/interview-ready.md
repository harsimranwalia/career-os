# Pipeline: Interview Ready

**Goal:** Prepare thoroughly for a specific interview — role-specific questions, STAR+R answer frameworks, and live practise with feedback.

**Skills used in order:** `/interview-prep` → `/mock-interview`

**Time estimate:** 20–35 minutes (10 min prep generation + 15–25 min live mock)

---

## When to use this pipeline

- You have an interview scheduled and need to prepare fast
- You want to practise a specific role before applying
- You just received a callback and have 24–48 hours to prepare

---

## Execution Steps

### Step 1 — Generate Prep Kit (`/interview-prep`)

**Trigger:** `"Run interview ready pipeline"` then paste the job description.

Or: `"Interview ready pipeline for [Job Title] at [Company]"` if the job is already in context from a previous skill.

Claude runs `/interview-prep` and outputs:

- **Part 1:** Top 10 interview questions, labelled by type (Behavioural / Technical / Situational / Leadership / Culture / Case)
- **Part 2:** 3 STAR+R frameworks built from your actual resume for the hardest questions
- **Part 3:** 5 smart questions to ask the interviewer
- **Quick prep checklist**

**Pause point:**
> "Prep kit ready. Review the STAR+R frameworks above and adjust the details to match your real experience.
>
> Want to practise live now with a mock interview? (yes / save for later)"

---

### Step 2 — Live Mock Interview (`/mock-interview`)

Claude runs `/mock-interview` using the same role context from Step 1.

The session:
1. Claude selects 5 questions weighted by role archetype
2. Asks ONE question at a time — waits for your answer
3. Gives per-answer feedback: ✅ Strong / ⚠️ Needs Work / ❌ Miss
4. Continues through all 5 questions
5. Delivers full debrief: Overall Readiness + strongest/weakest answer + top 1 thing to fix

**Mid-session commands:**
- `"hint"` → coaching nudge without giving away the answer
- `"again"` → retry the same question after feedback
- `"skip"` → move to next question
- `"stop"` → end early and go straight to debrief

---

## Output Summary

```
─────────────────────────────────────────
✅ Interview Ready Pipeline Complete

Role: [Job Title] at [Company]

Prep kit:
  ✓ 10 role-specific questions generated
  ✓ 3 STAR+R frameworks built from your resume
  ✓ 5 questions to ask the interviewer

Mock interview:
  Overall Readiness: [Ready / Nearly Ready / More Prep Needed]
  Strongest answer: Q[N]
  Top 1 thing to fix: [specific action before the interview]

Next: Review your weakest answer and run /mock-interview again for a second session.
─────────────────────────────────────────
```
