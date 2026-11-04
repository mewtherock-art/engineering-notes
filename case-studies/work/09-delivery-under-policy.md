# 09 · Delivering software under a copy-file policy

## Context
Internal programs run on a Windows server managed by IT. The server has **no Git**; updates are
done by copying files. Production databases must never be overwritten, and the owning team must be
told before a service restart.

## Problem
Ship updates to colleagues repeatedly without breaking live data or the people using it.

## Approach
- **Package, don't repo.** Deliver full project files with the front end already built, so the
  server needs only Python (no Node, no Git, no WSL).
- **Never overwrite the live database.** The package states which database files must *not* be
  copied; runtime data lives outside the code folder.
- **One-page notes** on copying to a new version (e.g. which seed/data scripts must be carried over),
  kept in the repo because they were forgotten once.
- **Smoke test** before handing over; a **change request sheet** for the IT team.
- Windows launchers are `.bat` only: every step checks `errorlevel` and ends with `pause`, so a
  failure is readable. No Linux paths in deliverables; the launcher bootstraps its own environment.
- Read-only by default for sensitive tools; every write requires a server-side unlock.

## Why these rules
Each one maps to something that actually went wrong: a hidden-window launcher swallowing an error,
a hard-coded path, an ignored `dist` folder that left the server without a front end.

## Result
Repeatable hand-over; the rules are encoded in a checklist and a delivery skill so they are
followed without relying on memory.
