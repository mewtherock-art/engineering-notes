# 23 · Skills for recurring work: delivery, KPI, minutes, work instructions

**Stack:** Claude Code skills with bundled scripts (`scripts/`, `REFERENCE.md`, examples)

## Context
Four jobs come back on a schedule or on request. Each had been done from memory, with a different
mistake each time. I turned each into a skill with its **hard rules at the top** and a checklist.

## The four skills

### `deliver-to-mis` — hand a program to the server team
Rules that must never be broken: deploy by **copying files** (no Git on the server); send the
**full project**, not just changed files; **build the front-end first** (the server has no Node);
**never overwrite the live database** — the server team copies only the database files I name,
from the *server's* version, never from my dev machine; tell the department **before** restarting
the service. Workflow: confirm scope and the version currently running → find database and
runtime files that are not in Git → build → make a package from a clean `git clone` and diff it
against the dev folder → smoke-test on a spare port with a *copy* of the database → draft a
"Job Update Version" sheet on the Desktop → report what was verified and what was not.
Banned: committing my unfinished work for me, and writing "tested on Windows" when it ran on Linux.
(Background: [case 09](09-delivery-under-policy.md).)

### `kpi2026` — monthly KPI workbooks
Fills two KPI workbooks from the monthly gap-cost file and exports that file to one continuous PDF.
The files are often open on someone else's machine, so the skill **never writes onto the shared
drive**: it exports to the Desktop and I copy it back.

### `minutemeeting` — bilingual minutes
Always asks first: date, time, room, attendees (the four things that cannot be inferred). Produces
a workbook with an English and a Thai sheet in the same layout; items added after the meeting are
highlighted so the reader sees what changed.

### `wi-generator` — controlled-form PDFs
Generates Work Instruction / Support Document / Quality Procedure PDFs in the company's numbered
form revisions from a JSON spec: **HTML for layout, headless Chromium to print**. The original
spreadsheet form is *not* filled with a library, because that drops the logo and shapes and the
text overflows cells. Also covers revising an approved document to a new revision (page surgery,
revision markers, English edition).

## Decision / trade-off
Skills cost context tokens whenever they load, so each one has a narrow description and loads
only on its trigger phrase. A longer skill is fine if every line came from a real mistake.

## What I would do differently
Add a dry-run self-test for each script so a skill can say "scripts verified today" instead of
trusting that nothing moved on the shared drive since last month.
