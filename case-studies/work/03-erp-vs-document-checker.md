# 03 · Checking ERP data against engineering documents

**Stack:** Python, pymupdf, rapidocr, Tkinter + PyInstaller, PDF report · **Three tools, one pattern**

## Context
Staff key material, routing and master data into the ERP by hand from engineering documents
(drawings, specification sheets, cycle-time sheets). Mistakes are found late, in production.

## Problem
Compare "what should be" (documents) with "what was entered" (ERP exports) automatically — for
documents that are sometimes clean digital PDFs and sometimes poor scans.

## Approach
- Parse ERP exports directly (UTF-16 tab-separated files). **Classify files by column
  signature, not by file name**, because people name exports inconsistently.
- Digital PDFs: read the text layer. Scanned drawings: local OCR with cached results. **No LLM** —
  deliberately, for cost and so results are deterministic and reproducible.
- Read a value from the token **nearest its label** in space, not by a regex over the whole page.
- Rule engine (25 rules across three levels in the largest tool) producing a one-page PDF report
  with one row per part, colour-coded OK / NG / MANUAL.

## The key decision: when unsure, say MANUAL
Values OCR cannot read reliably (e.g. a speed or a model code) are reported as **MANUAL**, never
guessed and never passed silently. The history shows the discipline paying off: false failures
were fixed by switching to a more robust extraction (fill-based instead of text-anchored), and
false positives by tightening a model-code pattern — each one added as a test.

## Related tool
A third tool reads a scanned drawing, proposes the specification and customer, matches against a
master list, and drafts an approval letter. Its output is labelled "suggestion — check every
value" because the human remains accountable.

## What I would do differently
Collect a labelled set of tricky scans earlier; accuracy work only became measurable once I had one.
