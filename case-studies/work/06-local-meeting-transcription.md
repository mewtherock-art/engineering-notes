# 06 · Local-only meeting transcription

**Stack:** FastAPI, faster-whisper (Thai), pyannote diarization, local translation model, optional RAG minute draft · **Tests:** 47+ at the time of writing

## Context
Meetings are in Thai, English and Japanese, and the content is confidential. Cloud
speech-to-text was not acceptable.

## Problem
Produce a transcript with speakers, correct company terminology, translations and a draft of the
minutes — with audio never leaving the machine.

## Approach
Pipeline: audio upload → speech-to-text → speaker diarization → terminology correction (a registry
of aliases → canonical terms) → translation that keeps protected terms → review + export
(TXT/JSON/CSV/SRT) → optional minute draft grounded by retrieval.

## Decisions
- **Confidentiality by design.** Everything touching audio runs on CPU locally. Only *text*, and
  only when the user presses a button, can go to a cloud model through their own logged-in CLI.
  Audio, transcripts and data are git-ignored.
- **Terminology registry** instead of hoping the model knows internal jargon; re-applying terms
  keeps translations of unchanged segments.
- **Grounded draft.** The minute draft cites retrieved evidence rather than free-generating.

## Problems and fixes
- **Hallucination loop.** Decoding without timestamps made the Thai model emit one repeated credit
  line for about 43% of a 35-minute file. Fix: decode *with* timestamps and add repetition guards;
  the same file then produced no repeated segments, and a test locks it in.
- **The crash was not a Linux OOM.** The Windows host ran out of memory while an unrelated package
  install overlapped a large model run. Linux saw ~2.3 GiB in use while Windows showed the WSL VM
  at 6.7 GiB, and the old boot log had no OOM record. The original guard only checked the guest's
  memory once before loading a model.
  Fix: a cap on the WSL VM's memory, a global lock so memory-heavy jobs queue instead of overlapping
  (the rule had lived only in one repo's notes, so other sessions never saw it), and no sub-agents
  during heavy runs. Verified after a restart.
- Audio decoding for diarization failed on some formats → decode with PyAV.

## What I would do differently
Add the resource guard before the first benchmark, not after the first crash.
