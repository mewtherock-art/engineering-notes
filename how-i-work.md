# How I work

## 1. Ask what is being built before how
Before building I check I am making the right *thing*, not only building it the right way.
I once built a multi-agent orchestrator that passed every check and was never used; the
checks asked "is it built correctly", nobody asked "is it the right product". Since then:
- ship the first slice and let the user use it before building slice two;
- every word in a requirement needs an acceptance criterion, otherwise delete the word;
- read all constraints together, because each can pass alone and still make the product impossible.

## 2. Interview the requirement
For tools used by other people I run a structured question session first (the forecast tool
in case 05 was specified this way) and write the answers into a `SPEC.md` / `CONTEXT.md`
glossary so later decisions use the same words.

## 3. Fail safe, not fail silent
Checkers I build return **OK / NG / MANUAL**. When OCR or parsing is not certain the answer is
`MANUAL`, never a silent pass. A wrong "PASS" costs more than a human glance.

## 4. Debug by tracing, not guessing
Reproduce → trace the failing path → try to falsify my hypothesis → cross-check every
breadcrumb. Then write a short post-mortem (root cause, fix, validation, how it slipped
through). Examples: [case 08](case-studies/work/08-legacy-mail-automation.md),
[case 16](case-studies/personal/16-dev-environment-incidents.md).

## 5. Treat production data as sacred
Never overwrite a live database on deploy, keep backups outside git, make destructive steps
explicit, recover with the least destructive command (`git reset --mixed`, not `--hard`).

## 6. Verify with real output
I do not trust "done" from a tool or an agent. I re-run tests, open the lines claimed, and
cross-check counts. Delegated work (to sub-agents or other models) is sized to the smallest
model that can do it, and I re-verify the result myself.

## 7. Evaluate honestly
Documentation says what was **run** versus only **written** (see the verification table in
[case 11](case-studies/personal/11-jlpt-word-map.md)). Strategy results are judged risk-adjusted
and after removing the top winners ([case 13](case-studies/personal/13-trading-evaluation.md)).

## 8. Build for the person who will run it
Non-technical colleagues get a double-click `.exe` or `.bat` that bootstraps itself, prints a
readable error and pauses. Delivery rules are written down and followed
([case 09](case-studies/work/09-delivery-under-policy.md)).
