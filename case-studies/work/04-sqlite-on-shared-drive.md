# 04 · A SQLite file on a shared drive

**Stack:** Python, SQLite, SMB file shares, Windows

## Context
Some tools had to run on a network drive so several people could start them, with no database
server available.

## Problem
SQLite's locking relies on the local filesystem. Across two machines over SMB, that lock state is
**not shared**. Two servers can both believe they hold the write lock and overwrite each other —
which does not just lose a record, it can **corrupt the file**.

## Approach
- Write down the failure first: lost update vs corrupted file, and which one is acceptable.
- SMB *does* honour **byte-range locks** across hosts. So at start-up the app takes a byte-range
  lock on the first byte of a lock file next to the database and writes its identity after that
  byte (outside the locked region), so a server that loses can still read who holds it.
- The lock is released when the process dies — cleanly or killed — so no stale-lock clean-up.
- A second instance **refuses to start** and tells the user which machine holds the database.
- Keep the virtual environment on the local machine, not on the share.

## Trade-off
Single writer host at a time. For a department tool with a handful of users that is acceptable and
far simpler than adding a database server.

## Lesson
Test with the real service identity and real UNC paths, not a local path on a developer machine —
a Windows Service runs as a different user and sees different drives.

## Caveat (kept honest)
Cross-machine behaviour was reasoned from the protocol and tested locally; a multi-PC test on the
real share was still pending when I wrote this.
