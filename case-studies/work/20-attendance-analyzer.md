# 20 · Analysing gate-swipe records: glossary first, two implementations

**Stack:** VBA macro in a macro-enabled workbook, Python (pandas) as an independent check

## Context
The factory gate exports every swipe — all visitor types, in and out. The task: show how each
manager arrived and left each day, and flag late / early / missing swipes.

## Problem
The raw data is ambiguous in ways that change the answer: one person holds several cards, only
"in" rows carry a name, lunch is not an outing, and a day nobody swiped is a holiday, not an absence.

## Approach
1. Wrote a **domain glossary first** (`CONTEXT.md`): Swipe, Card, Manager, Daily record, Lunch break,
   Outing, Late, Early leave, Missing IN/OUT, Company holiday, Absent, Sunday work, Day in progress —
   each with an "avoid" synonym, so the code and the conversation use the same words.
2. Implemented the rules as a VBA macro (the colleague's tool is a workbook).
3. Wrote a **second implementation in Python** from the same glossary, used only to diff against the
   workbook's daily sheet.

## Decision / trade-off
Two implementations cost more, but when both agree the rules were read the same way twice; when they
differ, the disagreement points at an ambiguous rule, not a typo. A name-alias table handles one
person recorded with two spellings.

## Result
Rules are written down once; the macro is checked by something that does not share its code.

## What I would do differently
Put the diff in an automated test with a small synthetic swipe file, so changing a rule shows
immediately which days change.
