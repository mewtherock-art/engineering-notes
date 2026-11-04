# 01 · Replacing a master Excel workbook with a web app

**Stack:** FastAPI, React, SQLite, PDF/HTML report output · **Scale:** 300+ commits over the project

## Context
Product cost breakdowns were built in one large "master" Excel workbook. Anyone could break a
formula, versions drifted between copies, and the numbers could not be searched or audited.

## Problem
Move the logic into a central web app **without** users losing trust in the numbers they were
used to seeing in Excel.

## Approach
1. Extract the reference sheets (material, process, cycle-time tables) into a database with
   migration scripts, so the web app and the old workbook started from the same data.
2. Rebuild the calculation as a service layer with the Excel formulas as the reference.
3. Guided multi-step form (a step indicator, reference lookups, manual-override tags) so people
   keep the flexibility they had in Excel for special cases.
4. Print/export layout matching the existing controlled form, so the output could still go into
   the existing document system.

## Decisions and trade-offs
- **Manual override as a first-class concept.** Real parts have exceptions. Fields that must be
  hand-entered are marked as manual instead of being silently recalculated (several fixes in the
  history are of the form "preserve manual value", "keep row when empty").
- **Export keeps formulas.** Users still want a spreadsheet; the export writes real formulas to
  the right input columns, not pasted values.
- **SQLite first, PostgreSQL later.** Few concurrent writers and a single host made SQLite enough;
  the schema avoids SQLite-only features so the move stays possible.

## Bugs worth remembering
- A deadline shown one day early on weekends: UTC conversion shifted the date. Fixed by treating
  it as a date, not a timestamp.
- Unit hard-coded as "PC" where the component unit was KG/M: made the unit data-driven.

## Result
- Replaced the workbook for the department's cost-breakdown flow.

## What I would do differently
Write the golden-case comparison as automated tests from day one; I did it manually at first.
