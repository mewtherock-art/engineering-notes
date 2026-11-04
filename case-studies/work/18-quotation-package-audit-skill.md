# 18 · An AI skill that audits a quotation package

**Stack:** Claude Code skill (`cbd-check`), PyMuPDF, Python for recomputation, a read-only HTTP API

## Context
Before a quotation goes out, a cost engineer has to check a PDF package: the customer's request
form, the assembly cost breakdown, one cost breakdown per sub-part, engineering drawings and a
material standard. Every page repeats the same header facts, and the numbers have to add up.
It is slow, and tired eyes miss a swapped digit in a part number.

## Problem
Make a repeatable audit that reads every page, recomputes every figure, compares with the previous
quote of the same part, and returns a **verdict** — without inventing anything that is not in the document.

## Approach
A skill (a written procedure the agent follows), not a script, because half the work is judgement:
1. **Prepare**: work on a copy; check each page for a text layer; scanned pages are rendered,
   rotated and cropped so small table cells are legible. Map the *real* page numbers first —
   the pages people quote from memory are often off.
2. **Benchmark outside the PDF**: pull the latest completed quote of the same part from the
   cost-breakdown system's read-only API; fall back to the scanned archive. Latest first — comparing
   with an old year once produced a wrong "+27 %" conclusion.
3. **Cross-check** each fact (header, quantity, length, part number, weight vs the standard) →
   `PASS / WARNING / FAIL`, each with a reason and a page number.
4. **Recompute in Python**, never in the model's head. Calculation correct ≠ price reasonable, so
   those are separate sections.
5. **Report** by fixed sections and a ranked issue table, ending in
   `APPROVE / APPROVE WITH CONDITIONS / NEED REVISION / HOLD`.

## Decision / trade-off
- Three explicit labels instead of guessing: `NOT READABLE`, `NOT FOUND`, `CANNOT VERIFY`.
- **A "not an issue" list** written from the owner's corrections (rounding up of lengths,
  tooling split between sub-parts and assembly, revision letter on a standard, naming of a joint).
  Without it the audit drowns the reader in false positives and gets ignored.
- New checks were added when a human reviewer bounced a real quote back: *expiry date of a
  purchased component's price confirmation*, *curing machine vs the previous quote*,
  *every sub-part line in the assembly sheet must carry its part number*. Each check came from a miss.
- Read-only by rule: no edits to the source, no mail, nothing leaves the machine.
- A permanent "short form" for the boss: verdict + price first, three reasons it passes, only the
  decisions that belong to the human.

## Result
The audit is repeatable and every claim traces back to a page. It does not replace the engineer's
sign-off; it removes the clerical part of it.

## What I would do differently
Some rules (the rounding of weight per metre) rest on what the app's code does, not on a source
document. The skill says so in the text instead of presenting it as fact — I would add the same
"unconfirmed" flag to any rule that comes from code archaeology.
