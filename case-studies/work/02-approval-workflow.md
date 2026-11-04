# 02 · Multi-department approval workflow

**Stack:** React, FastAPI, MySQL, PDF generation · **Scale:** ~250 commits

## Context
A quotation request passes through several departments (sales, quality, engineering,
purchasing, cost planning, managers). It used to travel by e-mail and spreadsheet, so nobody
knew where a request was stuck or how long each department took.

## Problem
Model the real process faithfully — including returns, parallel work and exceptions — and make
status and elapsed time visible without creating new ways to cheat.

## Approach
- Ran a long requirements interview and wrote a glossary plus a decision log before coding
  (login choice, roles, notifications, attachments, cutover plan).
- Workflow is **per part**, not per request; a request with many parts completes only when all
  parts do.
- Two kinds of role that can co-exist on one person: **business role** (what you do) and
  **approval role** (what you may approve). Permissions and tasks stay separate.
- **No self-approval** for normal users; only the developer test account is exempt.
- Approval stamps placed on the PDF approval boxes, showing the approver's full name and timestamp;
  multiple approvers supported.

## Interesting problems
- **SLA clocks.** Each department has its own clock. Total lead time keeps running while a request
  sits back with sales, but the department clocks pause, including for a department that is waiting
  *behind* the returned one (e.g. chief → engineer → sales). Several engineers in one function share
  a single department clock: a bug let the 2nd and 3rd engineer restart it because each step computed
  its due time from its own start. Getting this wrong makes departments look slower or faster than
  they are, so each rule is a test.
- **A multi-part request that never completed.** The per-part projection overwrote a finished part
  with the request-level status, so a request with several parts could never reach "complete". Fixed
  in the same change as a double-click guard (the task row is locked on submit, so a double click
  cannot approve twice) and a regression test.
- **Notifications.** Mail is queued in an encrypted outbox table and drained by a worker with retry;
  a message counts as "sent" only when its status says so, and exhausted retries become "failed"
  with an error log. Tests guard against duplicate reminders.
- **Attachment upload path:** quarantine → antivirus scan → signature/type validation → PDF merge
  validation → storage. If the scanner is unavailable the upload is refused (fail-closed), and
  password-protected or corrupt PDFs are blocked.

## Decision / trade-off
Application-level login (users table, hashed passwords, sessions, roles) was chosen over relying
on the web server's Windows authentication, so the app does not depend on the web server's authentication being available. Cost: I
own password hashing, sessions and admin user management.

## What I would do differently
Model the clock rules as a table of examples with the department owners *before* implementing —
they were the most intricate rules in the system.
