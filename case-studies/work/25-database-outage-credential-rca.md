# 25 · An internal app that could not reach its database

## Context
An internal web app talks to a local MariaDB (XAMPP) on a department server I reach over SSH.
After a crash the app refused to start with `Access denied` (errors 1044/1045).

## Problem
Two unrelated faults produced one symptom, so fixing either alone looked like "still broken".
I also lost about ten rounds to avoidable mistakes in how I debugged, not in the database itself.

## Approach
1. **Trace the failing path, not the symptom.** Read the code that loads the credential before
   touching the database.
2. **Fault A — config loader vs. file.** The `.env` value was wrapped in quotes and the loader
   strips them (8 chars in the file, 6 after loading). I first set the DB password from the raw
   file value, which was wrong. The password the database needs is the one *the app's loader
   produces*, so test with the app's own client, not a hand-typed CLI.
3. **Fault B — system tables corrupted by force-kill.** Several Aria tables in `mysql.*` failed
   CRC checks after the server was killed, so grants silently disappeared (the `1044`).
   Repaired with `--skip-grant-tables` bound to localhost, `REPAIR TABLE`, then re-granted
   minimum privileges to the app user.
4. **Back up the data directory before every force-kill or repair.**

## Decision / trade-off
- Did **not** propose "point the app at another DB" or "reset the password" until a check
  (`CHECK TABLE`, `SHOW GRANTS`, log timestamps) showed what was actually broken.
- Shut the repair session down in one SQL batch ending in `shutdown;`. Running
  `FLUSH PRIVILEGES` first re-enables auth and locks out the admin account whose password nobody
  knows.
- Did not read or print the secret; reported only its length and character class.

## Result
App starts again. Remaining known issues are written down, not hidden: two extra grant hosts still
carry the raw password, the admin password is unknown, and the server occasionally crashes at start
for a cause I could not confirm (antivirus is a suspect, not a finding).

## What I would do differently
- Read the config loader first, every time.
- Do not record tool behaviour in notes until I have tested it. I had written that a control
  panel's autostart collided with my script; testing showed it did not even start the module.
- Remote scripts: send as a file and run `bash file </dev/null`. A `cmd.exe` inside the script
  swallowed the rest of stdin and I misread the result as a database crash.
- Never `pkill -f` (it matches its own shell); say which machine and which shell every
  instruction to a person targets.
