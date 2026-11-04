# 07 · Semantic search over department documents

**Stack:** Python, local embeddings, OCR for scans · **Corpus:** ~2.6k files, ~141k chunks

## Context
Years of quotations, reports and drawings sit on network drives. Finding "the one we did before"
meant remembering a folder.

## Problem
Make old documents searchable, returning the **source file and location** so a person can check,
without an LLM generating answers (no API key, no data leaving).

## Approach
Retrieval-only: incremental ingest of changed files, local embeddings, OCR for scanned files
(running a Windows binary from WSL to avoid memory limits), results show file + position.

## Honest evaluation (the interesting part)
I reviewed my own system for usability instead of declaring it done:
- **Semantic queries work** for meaning-based search.
- **Exact codes (part numbers, form numbers) are unreliable**: embeddings blur identifiers.
- **"No answer" cases are unsafe**: it always returns *something* that looks plausible.

Conclusion: a semantic index alone is the wrong primary tool for this department. The better design
is a **local hybrid search** (exact/keyword match plus semantic ranking) with a small web UI and a
visible score threshold.

## Lesson
A search tool's worst failure is a confident wrong hit. Evaluate on exact-code queries and
no-answer queries, not only on the happy path.

## Result
Index built and queryable; hybrid redesign planned.
