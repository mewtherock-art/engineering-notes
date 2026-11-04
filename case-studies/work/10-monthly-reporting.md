# 10 · Monthly reporting automation

**Stack:** FastAPI, openpyxl/pandas, HTML dashboards, Excel macros driven via COM

## Context
Several recurring monthly tasks: collecting results from departments into a workbook, KPI
workbooks, a forecast comparison dashboard, an announcement mail with attachments.

## Approach
- **Keep the output format people already trust.** The tool reproduces the existing workbook layout;
  only the manual steps go away.
- **A dashboard where the old process was e-mail.** Department filters, month range, accumulated KPI,
  CSV export; highlights tuned so the current month is readable **on a projector** in the meeting.
- **Import safety.** Cells left blank in Excel must clear the stored actual; cells whose formulas have
  no cached value must be kept, not wiped (a real bug, fixed and tested).
- **Excel macros built and tested by driving Excel through COM from WSL**, never by hand-assembling
  the binary macro container.

## Lessons
- Projector contrast, port conflicts on start-up (kill old processes first) and auto-installing
  dependencies matter as much as the calculation for a tool people open once a month.
- Document the system for the next maintainer (a short system reference file in the repo).

