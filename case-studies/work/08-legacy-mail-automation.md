# 08 · Automating a mail client that has no API

**Stack:** Python, pywin32 / COM, CLI tools

## Context
The company mail system is a desktop client plus a web client, with no usable API. Sending
recurring announcements and reading mail by hand was repetitive.

## Problem
Send, reply, attach files and look up groups/people programmatically — and be able to *prove*
that a mail actually left.

## Root-cause write-up (short form)
**Failure:** the first implementation could not reach `Document.Send`.
**Root cause:** through pywin32 late binding, zero-argument COM members such as `OpenMail` and
`CreateDocument` behave as **properties**, not callable methods; and this profile had no
`MailFile` value, so an empty `GetDatabase` call could not find the mailbox.
**Fix:** access zero-argument members without parentheses; try the configured mail database first,
then the database currently open in the client UI.
**Validation:** ran on the real client; the send completed and the script printed the recipient.

## Design decisions
- Interactive confirmation by default; `--yes` only for explicit automation.
- **Always CC myself** on scripted mail: the only way to verify delivery and to resend.
- Browser automation is a **fallback** only when the COM probe fails, never the default.
- Directory lookup for groups so "send to everyone" resolves a real group, not a typed list.

## Lesson
When a library "should" work and does not, check how it binds, not just what it calls. And write
the RCA while it is fresh — it was needed again later.
