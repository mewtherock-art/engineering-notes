# 22 · The small jobs folder: what one-off requests taught me

A working folder of one-off requests from colleagues and the factory. None is a product; together
they show the pattern: *get the rule written down, script the boring part, verify against the source.*

| Job | What it was | The useful idea |
|---|---|---|
| **Run-cost document register** | Summarise a year of numbered approval-document sheets (one sheet per document, scattered over several workbooks, some copies stale) into a part-number summary with monthly sheets. | **Source priority list**: when the same document appears in several files, the first source wins; template sheets for future months are read last. A regex defines what counts as a document sheet. |
| **Quote comparison** | Compare two quote revisions of one request and draft the reply mail. | Output has two sheets: the comparison and the email draft, so the numbers and the words cannot drift apart. |
| **Part-component cleanup** | Clean a component list kept for two years in inconsistent shapes. | Keep the *raw* sheet and add a *cleaned* one; never overwrite the source. |
| **Monthly GCI consolidation** | Merge department workbooks, with `.bak` copies kept at each step. | Cheap manual backups before each destructive merge (the files are large and shared). |
| **Training-report forms** | Fill the company's external-training report form (Thai + English edition) and export a PDF; also a one-page Thai summary of a conference. | Fill the form through **LibreOffice's UNO API** so shapes and layout survive (openpyxl would drop them); Thai PDF from **HTML + a Thai font** rather than a spreadsheet print. |
| **Meeting minutes** | Turn an email thread into a bilingual minute workbook plus a short digest of open questions. | The digest separates *what was decided* from *what someone asked and nobody answered* — the second list is the useful one. (Skill: `minutemeeting`, see [case 23](23-work-skills.md).) |
| **Signature to transparent PNG** | Photo of an ink signature → transparent PNG usable in Word / Excel / PDF. | Estimate the paper background locally (morphological close + blur) and subtract it, weight channels by how blue ink absorbs, gate on blue-minus-red to drop black print. The method is written in a `SIGNATURE_METHOD.md` so it can be repeated. |
| **Attendance analyser** | See [case 20](20-attendance-analyzer.md). | Glossary + two implementations. |
| **Macro form with a generated container** | See [case 21](21-program-built-macro-workbook-rca.md). | Let Excel write Excel files. |
| **Event check-in workbook** | See [case 17](17-event-registration-workbook.md). | One place to key each fact. |
| **Presentation decks** | Several revisions of a career-review deck (v2 → v6). | Keep every version, name by number; the deck is evidence of iteration, not a final file. |

## Conventions that held across all of them
- Machine-generated outputs go next to the script that made them; the script is committed, the
  confidential inputs are not.
- A `.gitignore` for lock files (`~$*`), Windows download-marker files and build output.
- Anything that touches a shared drive writes to a *copy* first.
