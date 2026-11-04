# 21 · A generated macro workbook that Excel refused to open

**Stack:** Python (openpyxl / raw OOXML), VBA, Excel driven through COM

## Context
A form-printing workbook with buttons and macros had been *assembled by a program* (zip + a
hand-built `vbaProject.bin`) instead of being saved by Excel.

## Problem
Symptoms arrived in layers: "Removed Records: Object", then "Compile error in hidden module",
then — after fixing those — Excel hanging, 12 pages printed, the wrong status circle ticked.

## Root cause (two groups)
- **Container:** the fake VBA project had no document modules and invented checksums; the button
  XML lacked the macro binding and fallback. Excel cannot repair-open that through COM.
- **Code that had never run:** Thai text in UTF-8 `.bas` files imported as ANSI (garbled messages);
  a property set on a merged cell raised; **pasting shapes through the clipboard** crashed Excel on
  page 2+; the "find the status circle" helper returned the first circle left of the label, which
  was the wrong side. The first group had been hiding the second.

## Fix
Strip VBA and controls, then import the modules into a real Excel instance and save from there;
convert Thai literals to code-point expressions; copy ranges with `Range.Copy Destination:=`
(shapes travel with the range, no clipboard); choose the circle by position relative to the label.

## Lesson
Never fabricate a binary format Excel owns — drive Excel and let it write the file. And when a
file fails to open, assume the bugs behind the first error have not been seen yet.
