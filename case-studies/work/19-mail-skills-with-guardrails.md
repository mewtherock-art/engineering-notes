# 19 · Mail skills with guardrails: read, reply, and the monthly announcement

**Stack:** two Claude Code skills (`company-mail`, `gci-send-mail`) over the COM tool from [case 08](08-legacy-mail-automation.md)

## Context
Case 08 made a mail client scriptable. The next question was *who is allowed to press send*.
Mail to the whole company is irreversible and public.

## Problem
Let an agent read, summarise, draft and send mail for me — safely.

## Approach
**`company-mail` (general):**
- Never send before I confirm the draft. Reading others' mail is read-only.
- **Backend first, browser as fallback**: probe the COM backend once per session; if it fails,
  fall back to browser automation immediately and do **not** retry in a loop.
- "Sent" needs evidence at the destination (a Sent-folder record), not just "no error".
- One backend script that sends content to an external summarising service is **banned** for
  company mail — a name that sounds local can still send data out.
- Dry-run is mandatory before a real send: it prints the *resolved* recipients and attachment sizes.

**`gci-send-mail` (one recurring job):**
- Each month an announcement asks every department to fill in two workbooks by a due date.
- The month and due date are **always supplied by me** — the skill refuses to compute or guess them.
- The skill finds the newest attachments, shows name / modified date / size, and warns if the file is old.
- Recipient lists live in data files; "send to the team" is defined once.
- **One real send per one "send" from me.** After a test mail was once re-sent by the agent on its
  own, the rule was written down: editing the body means dry-run again and wait.
- Self-CC unless I am already a recipient, so delivery can be verified and the mail resent.

## Decision / trade-off
Short names passed the dry-run but failed at send ("no match in the address book"), so the
rule became *full addresses only*. A dry-run that resolves is not proof that the send resolves.

## Result
A monthly task that used to be manual is a three-step conversation (draft → dry-run → "send").

## What I would do differently
Encode the "resolved ≠ deliverable" lesson as a test against the address book, not a sentence in a skill.
