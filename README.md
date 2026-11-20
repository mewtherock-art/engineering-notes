# Engineering Notes

How I work and how I solve problems — told through case studies.

I am a cost-planning engineer at an automotive-parts manufacturer who builds internal tools
for my own department, plus personal projects on the side. The company code, data and names
are private, so these notes describe **the problem, the thinking and the fix** with all
customer, part, price and system details removed. Where a number is shown it comes from tests
or repo history, not from company data.

Start with [how-i-work.md](how-i-work.md), then pick a case.

## Work (internal tools, details anonymised)

| # | Case | What it shows |
|---|---|---|
| 01 | [Replacing a master Excel workbook with a web app](case-studies/work/01-excel-master-to-web-app.md) | Translating spreadsheet logic into a system without breaking trust in the numbers |
| 02 | [Multi-department approval workflow](case-studies/work/02-approval-workflow.md) | Roles, SLA clocks, idempotent notifications, "no self-approval" |
| 03 | [Checking ERP data against engineering documents](case-studies/work/03-erp-vs-document-checker.md) | OCR you can trust: say "manual" instead of guessing |
| 04 | [A SQLite file on a shared drive](case-studies/work/04-sqlite-on-shared-drive.md) | Failure-mode thinking: SMB locks, corruption vs lost write |
| 05 | [Consolidating five forecast sources](case-studies/work/05-forecast-consolidation.md) | Running a requirements interview, normalising dirty keys |
| 06 | [Local-only meeting transcription](case-studies/work/06-local-meeting-transcription.md) | Confidentiality by design, resource guards after OOM |
| 07 | [Semantic search over department documents](case-studies/work/07-document-search.md) | Honest evaluation: what retrieval can and cannot be trusted for |
| 08 | [Automating a mail client that has no API](case-studies/work/08-legacy-mail-automation.md) | COM quirks, root-cause write-up |
| 09 | [Delivering software under a copy-file policy](case-studies/work/09-delivery-under-policy.md) | Working inside constraints, protecting production data |
| 10 | [Monthly reporting automation](case-studies/work/10-monthly-reporting.md) | Repetitive spreadsheet work to a one-click tool |
| 17 | [A check-in workbook that survives changing seating plans](case-studies/work/17-event-registration-workbook.md) | One place to key each fact, derived views, check-in data kept apart from the plan |
| 18 | [An AI skill that audits a quotation package](case-studies/work/18-quotation-package-audit-skill.md) | Verdict + evidence, false-positive list built from corrections, "not readable" over guessing |
| 19 | [Mail skills with guardrails](case-studies/work/19-mail-skills-with-guardrails.md) | Who may press send: dry-run, one send per approval, self-CC |
| 20 | [Analysing gate-swipe records](case-studies/work/20-attendance-analyzer.md) | Glossary first, then two independent implementations |
| 21 | [A generated macro workbook Excel refused to open](case-studies/work/21-program-built-macro-workbook-rca.md) | RCA: errors hide other errors; let the owning app write its own file |
| 22 | [The small-jobs folder](case-studies/work/22-little-jobs-digest.md) | Pattern across one-off requests: source priority, raw vs cleaned, local transparency |
| 23 | [Skills for recurring work](case-studies/work/23-work-skills.md) | Delivery, KPI, minutes, controlled-form PDFs as checklists with hard rules |
| 25 | [An internal app that could not reach its database](case-studies/work/25-database-outage-credential-rca.md) | Two faults, one symptom: read the config loader, back up before force-kill, test tool behaviour before claiming it |
| 26 | [Revising controlled quality procedures](case-studies/work/26-controlled-document-rev-up.md) | Re-runnable pipeline, request→page→action table, measure vs look, report what stays OPEN |

## Personal projects

| # | Case | What it shows |
|---|---|---|
| 11 | [JLPT Word Map](case-studies/personal/11-jlpt-word-map.md) | Large-data UI (218k words), copyright model, honest "what was verified" doc |
| 12 | [Restaurant ordering app](case-studies/personal/12-restaurant-ordering-app.md) | Idempotent orders and row-level locking in SQL |
| 13 | [Evaluating trading strategies without fooling myself](case-studies/personal/13-trading-evaluation.md) | Risk-adjusted metrics, outlier tests, live vs backtest |
| 14 | [Tooling for working with AI agents](case-studies/personal/14-ai-workflow-tooling.md) | Token meter, skill registry, delegation by model size |
| 15 | [Small tools](case-studies/personal/15-small-tools.md) | Breadth: budget app, used-car DMS, batch printer, browser bridge |
| 16 | [Dev-environment incidents](case-studies/personal/16-dev-environment-incidents.md) | WSL daemon lifetime, DNS vs API outage, git recovery |
| 24 | [Catalogue of skills and agents](case-studies/personal/24-skills-and-agents-catalog.md) | Every skill kept, grouped by purpose; no agent approves its own work |

## Conventions

Every case follows the same template: **Context → Problem → Approach → Decision / trade-off →
Result → What I would do differently.**
