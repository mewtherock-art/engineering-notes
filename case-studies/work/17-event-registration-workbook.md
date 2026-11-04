# 17 · A check-in workbook that survives changing seating plans

**Stack:** Excel (.xlsm), VBA UserForm, openpyxl for generation, Excel driven through COM for build and test

## Context
A company event needed a day-of check-in tool: staff scan or type an employee ID, the screen shows
who has arrived at which table, and a monitoring sheet shows how many seats are still waiting. The
previous year's workbook did this, but every change in the plan (a table moved, a group renamed,
a table's capacity changed) meant editing several sheets and hard-coded lists by hand.

## Problem
Three things were maintained separately: the employee list, the "confirmed" list, and a control sheet of
extra seats. Names of tables were typed into the monitoring sheet and the floor plan. Re-using the file for
the next event meant re-typing most of it, and one wrong edit during the event could wipe check-in data.

## Approach
- **One place to key each fact.** Attendance is a single cell: an employee with a table number is
  confirmed, a blank means not confirmed. Table capacity, walk-in permission, zone and group live on a
  separate table sheet, one row per table. Nothing else is typed.
- **Everything else is derived.** Roster, monitoring blocks and floor plan are rebuilt by a Refresh
  button from those two sheets. Layout is by zone blocks rather than a drawn map, so any number of
  tables works.
- **Check-in data is stored apart from the plan.** A log sheet keyed by employee ID holds the arrival
  time. Refresh never touches it, so re-planning during the event is safe. A separate "new event"
  button backs up the whole file, requires typing a confirmation word, then clears the log.
- **Explicit rules for the awkward cases.** Unknown ID: say so and log it, never create a person.
  Walk-in without a table: choose from tables flagged for walk-ins that still have a free seat.
  Full table: blocked, not warned.

## Decision / trade-off
Deriving the group name from the table (instead of per person) removes hundreds of duplicate entries and
spelling drift, at the cost of not supporting two groups sharing one table. I checked the real data first:
group and table were one-to-one. If that stops being true, the group becomes a per-person column.

## Result
Changing a table's capacity, renaming a group or adding a zone is one edit plus Refresh. The workbook was
built and tested by driving real Excel from a script: 31 assertions covering check-in, duplicate scan,
unknown ID, walk-in, full-table block, idempotent Refresh, log preserved across Refresh, and new-event
backup, plus a headless run of the form's own event handlers. A fresh instance then opened the saved file
to confirm macros load and no external links remain.

## What I would do differently
- Ask earlier how the old and new table naming schemes map to each other; the floor plan and the roster used
  different codes and I could not infer the mapping, so the plan-label column was left for the owner.
- Test with the real scanner and the real event laptop's Excel version before the day, not only headless.
