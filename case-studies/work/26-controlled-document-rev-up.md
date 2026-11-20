# 26 · Revising controlled quality procedures without breaking the paper trail

**Stack:** Python, PyMuPDF (page surgery), HTML → headless Chromium (rebuild), a re-runnable patch script per round

## Context
Two approved Quality Procedure PDFs (a 25-page one and a 20-page one) had to move to a new
revision. Controlled documents carry a revision triangle per changed page, a per-page date and REV,
a change-record table and a cover with page totals. Reviewers read these first. A wrong number
there costs more trust than a wrong sentence in the body.

## Problem
- Each round of feedback ("this page", "from X to Y") touched a different part, and a fix in one
  place silently broke another: a white-out rectangle reached the page frame, a font name reused
  after an edit printed garbage glyphs, a page cloned many times made the file open in 34 s.
- A short answer ("whole row out") could be read two opposite ways; my first reading was wrong.
- Some inputs did not exist yet: the procedure lists six forms but no form files were provided.

## Approach
1. **One tool per document type.** Documents that stay mostly as approved: edit the PDF page by
   page. Documents with many layout changes: rebuild from a spec in code, never by hand, so every
   round is re-runnable and a baseline rebuild with the *old* spec must be pixel-identical to the
   previous file (that proves the pipeline before any change is made).
2. **Trace every edit to one sentence** of the current request: a table
   `request sentence → page → action`, checked against the render afterwards.
3. **Measure, don't eyeball.** Frame gaps must be an empty list; border strips pixel-diffed against
   the original (0 diff); line weights measured at 600 dpi and matched to the header.
4. **Change record says "from what to what"**, checked against the old PDF — not "edited". Pages
   untouched keep the previous date and REV; new pages are REV 0 with no triangle.
5. **Speed gate before delivery:** every page renders in under a second, file growth capped.
6. **Report format:** path, what changed per page, what was *measured* vs only looked at, what was
   *not* done and why, and the one question that blocks the rest. A change log with a section per
   round.

## Decision / trade-off
- Rebuilding from a spec costs a pipeline up front but ended four rounds of whack-a-mole.
- Missing inputs stay **OPEN in the report**; I did not invent six form pages to make the page
  count look finished.
- Scope clause holds scope only; "documents replaced" belongs in the change record. A
  stakeholder's one-line correction became a rule so it cannot recur.

## Result
Both documents revised with consistent dates, REV and totals, and a change record that lists real
changes. Every rule learned (about 20) lives in the generating skill's checklist, so the next
revision starts from them. Still open and written down: the six form files, and two reference
decisions that need an owner's yes/no.

## What I would do differently
Run the pixel-diff and speed gate automatically as the last step of the script rather than as
a checklist I must remember.
