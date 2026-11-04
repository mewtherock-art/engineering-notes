# 16 · Dev-environment incidents

Short post-mortems from my own tooling. Each follows reproduce → trace → falsify → fix.

## A. Scheduled job vanishes silently on WSL2
**Symptom:** a scheduled job simply did not run. No error, no traceback, not one log line for that
period — unlike a crash, which leaves evidence.
**Trace:** `journalctl --list-boots` showed the VM missing for hours; the previous boot ended with a
graceful `systemd-poweroff`, not a crash; Windows event logs showed the virtual NIC removed at the
same moment and no shutdown/restart/sleep events, so the PC itself had not gone down.
**Cause:** WSL2 powers its utility VM off about a minute after the last client disconnects, even with
background processes running. `nohup` and a cron `@reboot` do not help.
**Fix:** stop designing daemons inside WSL. Windows Task Scheduler runs `wsl.exe -d <distro> -- <script>`
at the scheduled time, so WSL boots on demand (three tasks replaced the daemon).
**Cost of not knowing:** time spent hunting a bug in code, tokens and the network when the cause was
outside Linux entirely.

## B. "API down" that was DNS
**Symptom:** a CLI failed with `ENOTIMP` while `curl` to the same host worked.
**Cause:** DNS resolution failing inside the CLI's own resolver library, not an API outage.
**Fix:** a local resolver stub (systemd-resolved) configured through `/etc/wsl.conf`, verified to
survive `wsl --shutdown`, with a rollback procedure written down first (remove the block *before* the
shutdown, or DNS is dead on next start).
**Lesson:** a test run inside a restricted sandbox returned a *fake* `ENOTIMP` too, so verification
has to happen outside the sandbox before drawing a conclusion.

## C. OAuth token deleted on a network blip
**Symptom:** a mail-to-chat bot died with `could not locate runnable browser` — it had fallen into an
interactive login on a headless machine.
**Trace:** the log had a name-resolution failure just before the browser error. Token refresh code
caught *every* exception and deleted the token file, so a brief DNS failure destroyed a good token.
**Fix:** separate a genuine `RefreshError` (credentials really bad → delete and re-login) from
transient errors (keep the token, skip this run).
**Aside:** a smaller model had "fixed" the symptom (disable the browser launch) and left a debug line
that caused restart spam. Fixing a symptom is not fixing the cause.
**Related bug:** the same bot sent empty summaries because the model API returned HTTP 200 with empty
content; added a guard that falls back to the next model.

## D. Corrupted git after a hard restart
**Symptom:** every git command failed with an object-file error after a memory-exhaustion crash.
**Cause:** the commit being written at crash time left **zero-byte object files** in `.git/objects`.
**Recovery (working tree = source of truth, never `reset --hard`):** back up `.git`; find the last good
commit and confirm it with `git cat-file -t`; remove the index and `logs/HEAD`; `git reset --mixed`;
**delete the empty object files before committing** (git recreates identical objects with the same
hash, sees the empty file already there, skips writing, then fails); commit with the saved message;
`git fsck`; run the tests; push. I also wrote a per-repo scan command to detect the same corruption
elsewhere.

## E. Machine freezes, but Linux shows no OOM
**Cause:** the Windows host ran out of memory (the WSL VM holds more than Linux reports), so Linux
logs were clean. Detail and fix in [case 06](../work/06-local-meeting-transcription.md).
**Lesson:** when the evidence you expect is missing, the cause may be on the other side of the
boundary.

## F. System drive nearly full
**Symptom:** C: had almost no free space.
**First guess (wrong):** the WSL virtual disk. I moved it to another drive with `wsl --manage --move`
(it copies, deletes the source, then updates the registry; I verified by comparing real data sizes,
not just `df`, which cannot detect a truncated image).
**Actual cause, after measuring:** Windows Update's download cache was ~35 GB (normally a few GB),
plus large simulator logs. Measured per folder from WSL with `robocopy /l` since directory-walking
tools were too slow; cleared the cache with a script that aborts unless elevated and unless the
update services are stopped; free space went from 43 to 85 GiB.
**Lessons:** measure before moving things; on a domain-managed PC "Run as administrator" can switch
to a *different user*, so paths under the user profile change; in batch files `if errorlevel N` means
"≥ N" and misses negative exit codes, so compare `%ERRORLEVEL%` to `0` explicitly.
