# 05 · Consolidating five forecast sources

**Stack:** Python, pandas/openpyxl, Tkinter + PyInstaller (`.exe`), tests

## Context
A customer's forecast arrives as several different CSV/Excel/ERP exports. Summing them by hand
per part per month, per plant, took a long time and was easy to get wrong.

## Problem
Build one tool a colleague can double-click: pick files, get a single workbook with quantity per
part per month, split by plant.

## Approach
1. **Requirements interview first.** A long question session confirmed every input, which are
   optional, how missing sources behave (zero, or "skip"), and how to treat the ERP order data
   (subtract it or not). The answers became `SPEC.md`.
2. **Normalise keys before matching.** Part numbers differ between sources in whitespace and a
   trailing dot: `"91406-SAA -J001  "` and `"91406-SAA-J001."` are the same part. Strip all
   whitespace, ignore the trailing dot for matching, and display the form the source used.
3. Shared structure with sibling tools (GUI + headless CLI + single-file build) so one
   `build.bat` process works for all.
4. Tests per source parser and for the whole pipeline.

## Lesson
Most "calculation bugs" in data-merging tools are really **key-matching bugs**. Spend the time on
normalisation rules and test them with the ugly real examples.

